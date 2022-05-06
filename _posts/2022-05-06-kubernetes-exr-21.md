---
title: 쿠버네티스 - exr21 - VIEW CERTIFICATE DETAILS
author: YHole
date: 2022-05-06 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr21 VIEW CERTIFICATE DETAILS

```bash
# Identify the certificate file used for the kube-api server

# kind: Pod
# name: kube-apiserver

cat /etc/kubernetes/manifests/kube-apiserver.yaml
# look for the line --tls-cert-file

- --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
```

```bash
# Identify the Certificate file used to authenticate kube-apiserver as a client to ETCD Server

cat /etc/kubernetes/manifests/kube-apiserver.yaml

# etcd-certfile

spec:
  containers:
  - command:
    - kube-apiserver
    - --advertise-address=10.29.209.8
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction
    - --enable-bootstrap-token-auth=true
    - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt # 확인
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
    - --etcd-servers=https://127.0.0.1:2379

/etc/kubernetes/pki/apiserver-etcd.crt
/etc/kubernetes/pki/apiserver-kubelet-client.crt
/etc/kubernetes/pki/apiserver-etcd-client.key
/etc/kubernetes/pki/apiserver-etcd-client.crt # 정답
/etc/kubernetes/pki/apiserver.crt
```

```bash
# Identify the key used to authenticate kubeapi-server to the kubelet server

# Look for kubelet-client-key option
- --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key

/etc/kubernetes/pki/apiserver.key
/etc/kubernetes/pki/apiserver-kubelet-client.key # 정답
/etc/kubernetes/pki/apiserver-etcd-client.key
/etc/kubernetes/pki/apiserver-kubelet-client.crt
/etc/kubernetes/pki/front-proxy-client.key
```

```bash
# Identify the ETCD Server Certificate used to host ETCD server

# Look for cert-file option in the file /etc/kubernetes/manifests/etcd.yaml
vi /etc/kubernetes/manifests/etcd.yaml

  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://10.29.209.8:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt # 정답
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd
    - --initial-advertise-peer-urls=https://10.29.209.8:2380
    - --initial-cluster=controlplane=https://10.29.209.8:2380
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://10.29.209.8:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://10.29.209.8:2380
    - --name=controlplane
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
```

```bash
# Identify the ETCD Server CA Root Certificate used to serve ETCD Server

# ETCD can have its own CA. So this may be a different CA certificate than the one used by kube-api server.

# Look for CA Certificate (trusted-ca-file) in file /etc/kubernetes/manifests/etcd.yaml

/etc/kubernetes/pki/ca.crt
/etc/kubernetes/pki/etcd/ca.crt # 정답
/etc/kubernetes/pki/apiserver-kubelet-client.crt
/etc/kubernetes/pki/etcd/server.crt
```

```bash
# What is the Common Name (CN) configured on the Kube API Server Certificate?

# OpenSSL Syntax: openssl x509 -in file-path.crt -text -noout

# Run the command openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text and look for Subject CN.

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

# Subject: CN = kube-apiserver
```

```bash
# What is the name of the CA who issued the Kube API Server Certificate?
# look for issuer

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

# Issuer: CN = kubernetes
```

```bash
# Which of the below alternate names is not configured on the Kube API Server Certificate?
# look at Alternative Names

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

X509v3 Subject Alternative Name:
  DNS:controlplane, DNS:kubernetes, DNS:kubernetes.default, DNS:kubernetes.default.svc, DNS:kubernetes.default.svc.cluster.local, IP Address:10.96.0.1, IP Address
```

```bash
# What is the Common Name (CN) configured on the ETCD Server certificate?
# look for Subject CN.

openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text

Subject: CN = controlplane
```

```bash
# How long, from the issued date, is the Kube-API Server Certificate valid for?

# File: /etc/kubernetes/pki/apiserver.crt
# check on the Expiry date.

openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

Validity
  Not Before: May  4 12:13:22 2022 GMT
  Not After : May  4 12:13:22 2023 GMT
```

```bash
# How long, from the issued date, is the Root CA Certificate valid for?
# look for validity

openssl x509 -in /etc/kubernetes/pki/ca.crt -text

Validity
  Not Before: May  4 12:13:22 2022 GMT
  Not After : May  1 12:13:22 2032 GMT
```

```bash
# Kubectl suddenly stops responding to your commands. Check it out! Someone recently modified the /etc/kubernetes/manifests/etcd.yaml file

# You are asked to investigate and fix the issue. Once you fix the issue wait for sometime for kubectl to respond. Check the logs of the ETCD container.

# 힌트
# The certificate file used here is incorrect. It is set to /etc/kubernetes/pki/etcd/server-certificate.crt which does not exist. As we saw in the previous questions the correct path should be /etc/kubernetes/pki/etcd/server.crt.

ls -l /etc/kubernetes/pki/etcd/server* | grep .crt

-rw-r--r-- 1 root root 1188 May  4 12:13 /etc/kubernetes/pki/etcd/server.crt

# Update the YAML file with the correct certificate path and wait for the ETCD pod to be recreated. wait for the kube-apiserver to get to a Ready state

# 실제 수정
vi /etc/kubernetes/manifests/etcd.yaml

- --cert-file=/etc/kubernetes/pki/etcd/server-certificate.crt
=> --cert-file=/etc/kubernetes/pki/etcd/server.crt
```

```bash
# The kube-api server stopped again! Check it out. Inspect the kube-api server logs and identify the root cause and fix the issue.

# Run docker ps -a command to identify the kube-api server container. Run docker logs container-id command to view the logs.

docker ps -a | grep kube-apiserver
docker logs 8af74bd23540  --tail=2

# "transport: authentication handshake failed: x509: certificate signed by unknown authority"

# This indicates an issue with the ETCD CA certificate used by the kube-apiserver. Correct it to use the file /etc/kubernetes/pki/etcd/ca.crt.

vi /etc/kubernetes/manifests/etcd.yaml

# 모르겠음
```
