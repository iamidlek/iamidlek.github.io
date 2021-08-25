---
title: 정규 표현식(Regular Expression)
author: YHole
date: 2021-08-22 +0900
categories: [Blog, supplementary data]
tags: [regular expression, regexp]
---

## 정규 표현식

- 역할
  - 문자 검색
  - 문자 치환
  - 문자 추출

- -1은 해당사항이 없을 경우 반환

### 만들기

- 생성자 함수 방식
  - const regexp = new RegExp("표현식","옵션");

- 리터럴 방식
  - const regexp = /표현식/옵션;

### 메서드

- 원본에 영향 없음

<table width="700" border="1">
  <thead>
    <tr align="center">
      <th width="200">메서드</th>
      <th width="500">설명</th>
    </tr>
  </thead>
  <tbody align="center">
    <tr>
      <td>reg<code>.test</code>('')</td>
      <td>일치/불일치 (Boolean 반환)</td>
    </tr>
    <tr>
      <td>''<code>.match</code>(reg)</td>
      <td>매치되는 문자열을 Array로 반환</td>
    </tr>
    <tr>
      <td>''<code>.replace</code>(reg,replace)</td>
      <td>'' → replace 후 반환</td>
    </tr>
  </tbody>
</table>

- 그 외 메서드
  - reg`.exec`('') : 제일 처음 일치하는 단어의 정보 반환 (배열)

  - ''`.search`(reg) : 일치하는 단어의 첫 인덱스번호 반환

  - ''`.split`(reg) : 해당 단어를 기준으로 요소를 자르고  
  해당 단어는 포함안된 배열을 반환

  - reg.toString() : 생성자 함수 방식을 리터럴 방식의 문자열로 반환

### 플래그

- g
  - 모든 문자와 여러 줄 일치(global)
  - 문자를 전부 다 훑는다
- i	
  - 대소문자를 구분하지 않음(insensitive, ignore case)
- m	
  - 여러 줄 일치(multi line)
  - 변수 내부의 line은 시작과 끝이 처음과 마지막으로 하나이지만  
  줄바꿈이된 line별로 시작과 끝을 두어 각 line을 확인
- u	
  - 유니코드(unicode)
  - 정규식을 범위로 작성할때 유니코드의 순서가 적용된다


### 사용 예제

- 기호의 의미를 하나하나 확인하기 전 예제를 보자


1. /ab/g
  - 전체 문자 중 `ab`가 포함되어 있는지 확인
1. /[ab]/g
  - 전체 문자 중 `a`,`b`가 포함되어 있는지 확인
1. /[0-9]/g
  - 전체 문자 중 0~9중의 숫자가 포함되어 있는지 확인

- 유효성 검사

1. /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/
  - 핸드폰 번호
1. /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/i
  - 이메일
1. "/^\d{3}-?\d{3}$/u"
  - 우편 번호
1. "/^[a-zA-Z]\w{2,7}$/u"
  - 아이디
1. "/^\d{2}[0-1]\d[0-3]\d-?[1-6]\d{6}$/u"
  - 주민등록 번호 


### 특수기호(패턴)

<table width="700" border="1">
  <thead>
    <tr align="center">
      <th width="40">패턴</th>
      <th width="100">사용 예</th>
      <th width="560">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">^</td>
      <td align="center">/^ab/</td>
      <td>line에서 맨 앞이 ab로 시작</td>
    </tr>
    <tr>
      <td align="center">$</td>
      <td align="center">/ab$/</td>
      <td>line에서 맨 뒤가 ab로 끝남</td>
    </tr>
    <tr>
      <td align="center">.</td>
      <td align="center">/a./</td>
      <td>단어에서 a 뒤에 임의의 한 문자가 온다</td>
    </tr>
    <tr>
      <td align="center">|</td>
      <td align="center">/a|b/</td>
      <td>단어에서 둘 중 하나에 해당하는지 확인</td>
    </tr>
    <tr>
      <td align="center">*</td>
      <td align="center">/a*/g</td>
      <td>*앞의 한개의 문자가 0회 이상 중복되는 부분을 최대한 묶음 papaa → ['','a','','aa']</td>
    </tr>
    <tr>
      <td align="center">*?</td>
      <td align="center">/a*?/g</td>
      <td>a*의 탐욕의 반대 게으름으로(lazy) papaa → ['','','','','']</td>
    </tr>
    <tr>
      <td align="center">+</td>
      <td align="center">/a+/g</td>
      <td>+앞의 한개의 문자가1회 이상 중복되는 부분을 최대한 묶음 papaa → ['a','aa']</td>
    </tr>
    <tr>
      <td align="center">+?</td>
      <td align="center">/a+?/g</td>
      <td>1회 이상 중복되는 부분을 묶지 않음 papaa → ['a','a','a']</td>
    </tr>
    <tr>
      <td align="center">?</td>
      <td align="center">/abc?/g</td>
      <td>?앞의 한개의 문자가 없거나 1회 있거나 abcabdabcc → ['abc','ab','abc']</td>
    </tr>
    <tr>
      <td align="center">??</td>
      <td align="center">/abc??/g</td>
      <td>마찬가지로 적은 쪽(없거나)을 선택 abcabdabcc → ['ab','ab','ab']</td>
    </tr>
    <tr>
      <td align="center">{}</td>
      <td align="center">/abc{2}/g</td>
      <td>앞의 한개의 문자의 반복 개수 abcabdabcc → ['abcc']  {2,}는 2번이상 반복 {2,5} 2번이상 5번 이하 {2,5}? 는 2번이 된다</td>
    </tr>
    <tr>
      <td align="center">()</td>
      <td align="center">/(ab)+/g</td>
      <td>ab를 그룹으로 취급 ababab → ['ababab'] abbbabbb → ['ab','ab']</td>
    </tr>
    <tr>
      <td align="center">()</td>
      <td align="center">/(\d+)(\w))/</td>
      <td>순서대로 캡쳐된다 123abc → ["123a","123","a"] 캡쳐 없는 경우/\d+\w/ → ["123a"] 캡쳐된 부분은 독립적으로도 추출</td>
    </tr>
    <tr>
      <td align="center">()</td>
      <td align="center">/(a)\1/</td>
      <td>캡쳐1은 \1에 변수처럼 할당된다(9까지 가능) 즉 aa와 독립적 a 가 실행된다 aabb → ['aa','a']</td>
    </tr>
    <tr>
      <td align="center">()</td>
      <td align="center">/((b))\1\2/</td>
      <td>캡처 푼 것과 캡처 1,2로 bbb ((b)) 는 b,b가 된다 aabbbcc → ['bbb','b','b']</td>
    </tr>
    <tr>
      <td align="center">(?&lt;&gt;)</td>
      <td align="center">/?&lt;test-case&gt;/</td>
      <td>/?&lt;test-case&gt;a/  → groups.test-case = a 가 된다 (‘object’ 타입이므로 ‘camelCase’)</td>
    </tr>
    <tr>
      <td align="center">(?:)</td>
      <td align="center">/(?:ab)+/</td>
      <td>캡쳐를 하지 않는 그룹 ababab → ['ababab'] 캡쳐를 한다면 ab+ 가 실행되어 'ab'가 포함된다</td>
    </tr>
    <tr>
      <td align="center">(?=)</td>
      <td align="center">/ab(?=c)/</td>
      <td>(?=c) 앞의 ab를 찾고 뒤에 c가 오는지 확인 abc → ['ab'] abd → null</td>
    </tr>
    <tr>
      <td align="center">(?!)</td>
      <td align="center">/ab(?!c)/</td>
      <td>(?!c) 앞의 ab를 찾고 c가 오면 null 이외는 ab 반환 (ab뒤에 c가 오는 경우만 배제)</td>
    </tr>
    <tr>
      <td align="center">(?&lt;=)</td>
      <td align="center">/(?&lt;=a)bc/</td>
      <td>확인 순서는 a가 오는지 확인이 먼저 abc → ['bc'] zbc → null</td>
    </tr>
    <tr>
      <td align="center">(?&lt;!)</td>
      <td align="center">/(?&lt;!a)bc/</td>
      <td>앞에 a가 오면 null 이외는 bc 반환 (bc앞에 a가 오는 경우만 배제)</td>
    </tr>
    <tr>
      <td align="center">[]</td>
      <td align="center">/[a-z]/</td>
      <td>[]안은 or의 의미, -는 범위를 의미 즉 a,b,c...z 중 일치하는지 찾는다</td>
    </tr>
    <tr>
      <td align="center">[]</td>
      <td align="center">/[^.]/</td>
      <td>[]안에서의 ^는 제외의 의미, [] 안에서 이스케이프\를 붙이지 않고 특수문자를 사용할 수 있다</td>
    </tr>
    <tr>
      <td align="center">[]{}</td>
      <td align="center">/ab[5-9]{2}/</td>
      <td>ab를 포함하며 5-9로 이루어진 2자리수를 포함하는 것과 매치</td>
    </tr>
  </tbody>
</table>

### 특수문자

- 이스케이프
  - &#92;., \?, \/, \$, \^
- \b	
  - 영문 대소문자 52개 + 숫자 10개 + _(underscore) 가 아닌 나머지 문자에 일치하는 경계(boundary) (공백을 의미)
- \B
  - 반대의 의미 문자를 의미
- \d
  - digit 숫자를 의미
- \D
  - 숫자가 아닌 문자를 의미
- \w	
  - Word, 영문 대소문자 + 숫자 + _ 를 의미
- \W	
  - \w와 반대를 의미
- \s
  - 공백(Space, Tab 등)을 의미
- \S	
  - 공백이 아닌 문자를 의미
- \n	
  - 줄 바꿈(LF, U+000A)

> 이하의 내용은 참고

- \x	16진수 문자를 의미, /\x61/는 a에 일치
- \0	8진수 문자에 일치, /\141/은 a에 일치
- \u	유니코드(Unicode) 문자에 일치, /\u0061/는 a에 일치
- \r	캐리지 리턴(CR, U+000D) 문자에 일치
- \t	탭 (U+0009) 문자에 일치

### 우선순위

<table width="500" border="1">
  <thead>
    <tr align="center">
      <th width="90">우선순위</th>
      <th width="410">패턴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">1</td>
      <td align="center">\</td>
    </tr>
    <tr>
      <td align="center">2</td>
      <td align="center">(), (?:), (?=), []</td>
    </tr>
    <tr>
      <td align="center">3</td>
      <td align="center">*, +, ?, {}</td>
    </tr>
    <tr>
      <td align="center">4</td>
      <td align="center">^, $, \b</td>
    </tr>
    <tr>
      <td align="center">5</td>
      <td align="center">|</td>
    </tr>
  </tbody>
</table>


### 기타 활용


> 이하의 사이트를 참조함

[참조](https://myeonguni.tistory.com/1555)

- iframe 제거

  ```javascript
  $STRING=preg_replace("!<iframe(.*?)<\/iframe>!is","",$STRING);
  ```

- `&nbsp;` 제거

  ```javascript
  $STRING=str_replace("&nbsp;"," ",$STRING);
  ```

- 복수 공백 합치기

  ```javascript
  $STRING=preg_replace("/\s{2,}/"," ",$STRING)
  ```
  
- style="" 제거
  
  ```javascript
  $STRING=preg_replace("/ style=(\"|\')?([^\"\']+)(\"|\')?/","",$STRING);
  ```
