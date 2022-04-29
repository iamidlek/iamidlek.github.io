---
title: 쿠버네티스 - exr14 - Managing Application Logs
author: YHole
date: 2022-04-29 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr14 Managing Application Logs

```bash
# We have deployed a POD hosting an application. Inspect it. Wait for it to start.

k get pods

NAME       READY   STATUS    RESTARTS   AGE
webapp-1   1/1     Running   0          64s
```

```bash
# A user - USER5 - has expressed concerns accessing the application. Identify the cause of the issue. 애플리케이션 액세스에 대한 우려를 나타냄
# Inspect the logs of the POD

kubectl logs webapp-1

22-04-26 04:43:01,098] INFO in event-simulator: USER3 is viewing page2
[2022-04-26 04:43:02,098] INFO in event-simulator: USER2 is viewing page2
[2022-04-26 04:43:03,099] INFO in event-simulator: USER3 logged out
[2022-04-26 04:43:04,101] INFO in event-simulator: USER3 is viewing page2
[2022-04-26 04:43:05,102] WARNING in event-simulator: USER5 Failed to Login as the account is locked due to MANY FAILED ATTEMPTS.

# USER5의 내용을 확인해 보자
```

```bash
# A user is reporting issues while trying to purchase an item. Identify the user and the cause of the issue.
# Inspect the logs of the webapp in the POD

kubectl logs webapp-2 -c simple-webapp

...
[2022-04-26 04:55:59,185] WARNING in event-simulator: USER30 Order failed as the item is OUT OF STOCK.
...
```
