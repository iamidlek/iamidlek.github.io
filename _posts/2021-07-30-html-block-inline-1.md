---
title: HTML Block&Inline
author: YHole
date: 2021-07-30 +0900
categories: [Front end, html]
tags: [html, semantic, block, inline, element, tag]
---

# 블럭 요소

전역 속성을 사용한다.

## &lt;dl&gt; - &lt;dt&gt; - &lt;dd&gt;

- Discription List - Definition Details - Definition Term

    ```html
    키와 값 형태를 표시할 때 유용
    <dl>
      <dt>단어</dt>
      <dd>정의</dd>
    </dl>
    ```

## &lt;ol&gt; - &lt;ul&gt; - &lt;li&gt;

- Ordered List - Unordered List - List Item

    ```html
    <ol>
      <li>단어</li>
      <li>정의</li>
    </ol>
    <ul>
      <li>단어&lt;/li>
      <li>정의&lt;/li>
    </ul>
    ```

## &lt;p&gt;

- Paragraph 하나의 문단

## &lt;hr /&gt;

- Horizontal Rule 수평선이 표시되나 의미적으로만 사용

## &lt;pre&gt;

- Preformatted Text 줄바꿈과 공백이 그대로 반영됨, Monospace 계열 글꼴로 표시됨

## &lt;blockquote&gt;

- Block Quotation 인용문에 사용

    cite 속성을 적고 값으로 URL 을 작성

## &lt;div&gt;

- Division 의미 없음, 콘텐츠 영역을 나누기 위함

-----------

# 인라인 텍스트

## &lt;a&gt;

- Anchor 파일, 이메일, 전화 등 다른 URL로 연결 하는 하이퍼링크
    - href 값으로 경로가 있을 때 download 를 적으면 해당 소스가 다운로드 됨
    - download 에 값을 지정하면 그 지정한 이름으로 파일이 저장됨
    - href 에 페이지 내의 id 값을 넣으면 페이지 내에서 이동
    - target, rel, ping, type 등의 속성 사용 가능

## &lt;abbr&gt;

- Abbreviation 약어를 감싸는 태그
    - title 속성의 값에 본딧말 정보를 넣는다

## &lt;b&gt;

- Bring Attention 독자의 주의를 끌기위한 태그
    - 글씨가 굵어지지만 꾸미기 용도로 사용해선 안된다
    - 적절한 태그가 없을 때 최후의 수단으로 사용한다

## &lt;mark&gt;

- 형광팬을 칠한 모습으로 하이라이트, 중요한 부분을 표시
    - 연관성이 높은 곳에 태그한다
    - 적절한 태그가 없을 때 최후의 수단으로 사용한다
    - ::before,after 의 값으로 content: '[강조 시작]' 을 작성하여 접근성을 고려한다

## &lt;em&gt;

- Emphasis 콘텐츠 강조
    - 중첩이 가능하며 중첩될수록 강조 의미가 강해진다
    - 정보 통신 보조기기 등에서 구두 강조로 발음된다
    - 이탈릭체로 표현된다

## &lt;i&gt;

- Italic type 이탈릭체
    - &lt;em&gt;, &lt;strong&gt; &lt;mark&gt; &lt;cite&gt; &lt;dfn&gt; 에 의미적으로 부합하지 않을 때 사용
    - 기술 용어, 외국어 구절, 등장인물의 생각 등에 사용

## &lt;strong&gt;

- Strong Importance 의미의 중요성을 나타냄
    - 중대하거나 급한 내용에 사용
    - bold 가 적용됨

## &lt;dfn&gt;

- Definition 문장에서 정의하고 있는 용어를 나타낸다
    - 설명 뒤에 그 설명에 관한 용어가 나오면 그 용어를 태그한다
    - &lt;p&gt;, &lt;dt&gt;/&lt;dd&gt;, &lt;section&gt; 를 조상으로 두고 용어의 정의가 담긴다
    - &lt;dfn&gt;&lt;abbr title="본딧말"&gt;약어&lt;/abbr&gt;&lt;/dfn&gt;
    - 이탈릭체가 된다
    - title 속성에 값이 있고 dfn 콘텐츠가 비어있으면 값이 용어가 된다

## &lt;cite&gt;

- 저작물의 출처를 표기할 때 사용하며, 저작물의 제목을 반드시 포함해야 함
    - 참조를 설정
    - 이탈릭체가 적용됨

## &lt;q&gt;

- Inline Quotation 짦은 인용문을 설정
    - cite 속성을 사용하여 값으로 URL을 넣는다
    - 이탈릭체가 적용됨

## &lt;u&gt;

- Underline 밑줄 만들기
    - 꾸미기 용도
    - span 에 text-decoration : underline 이 더 선호된다
    - &lt;a&gt; 와 혼동을 주어선 안된다

## &lt;code&gt;

- Inline Code 코드 범위를 설정
    - 짧은 코드 조각을 나타낼 때 사용
    - 고정폭(Monospace) 글꼴 계열로 표시됨
    - &lt;pre&gt; 안에서 사용하면 여러줄 작성 가능

## &lt;kbd&gt;

- Keyboard Input 사용자의 키보드 모양으로 나옴
    - kbd&gt;Ctrl&lt;/kbd&gt; + &lt;kbd&gt;Shift&lt;/kbd&gt; + &lt;kbd&gt;R&lt;/kbd&gt; 과 같이 사용
    - 고정폭(Monospace) 글꼴 계열로 표시됨

## &lt;time&gt;

- 날짜나 시간을 나타내기 위한 목적으로 사용
    - datetime 속성의 값으로 Date 를 입력
    - 2011-11-18T14:54:39.929-0400 , PT4H18M3S 등이 유효한 Date

## &lt;sup&gt; - &lt;sub&gt;

- Superscripted text 위 첨자  -  Subscript text 아래 첨자
    - 지수나 로그와 같은 수식표현에 사용

## &lt;span&gt;

- 콘텐츠 영역을 설정 &lt;div&gt; 의 인라인 버전과 같다

## &lt;br /&gt;

- 줄바꿈

## &lt;del&gt; - &lt;ins&gt;

- 삭제로 변경된, 추가로 변경된 텍스트의 범위를 지정
    - cite 의 값으로 어디서 참조하는지, date 의 값으로 언제 수정되었는지 지정
    - 속성의 값은 유저에게 보여지지 않음
