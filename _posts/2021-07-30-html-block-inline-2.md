---
title: HTML Media&Script
author: YHole
date: 2021-07-30 +0900
categories: [프론트 엔드, html]
tags: [html, semantic, media, script, element, tag]
---

# 멀티 미디어

전역 속성을 사용한다.

## &lt;img /&gt;

- 문서에 이미지를 삽입(인라인)

    ```
    src    : URL 또는 경로
    alt    : 이미지에 대한 대체 택스트
    width  : 가로 너비
    height : 세로 너비
    srcset : 브라우저(user agent)에게 제시할 이미지 URL과 원본 크기의 목록을 정의
    sizes  : 미디어 조건에 따라 이미지 최적화 크기의 목록을 정의
    ```

## &lt;audio&gt;

- 음악, 소리를 삽입(인라인)

    ```
    src      :  URL 또는 경로
    preload  :  metadata(default), none, auto(전체파일 로드) 값으로 설정한다
    autoplay    준비되면 바로 재생
    controls    제어 메뉴 표시
    loop        반복(전체반복)
    muted       초기 상태 음소거

    autoplay 가 있으면 preload는 무시된다
    ```

## &lt;video&gt;

- 영상을 삽입(인라인)

    ```
    src      :  URL 또는 경로
    preload  :  metadata(default), none, auto(전체파일 로드) 값으로 설정한다
    autoplay    준비되면 바로 재생
    controls    제어 메뉴 표시
    loop        반복(전체반복)
    muted       초기 상태 음소거
    poster   :  동영상의 썸네일이미지 URL 또는 경로
    width    :  영상의 가로 너비
    height   :  영상의 세로 너비

    autoplay 가 있으면 preload는 무시된다
    ```
    <details style='margin-bottom:8px;'>
    <summary>
    track
    </summary>
    <ul style='list-style:none;padding-left:28px;'>
    <pre>kind     :  값으로 subtitles, captions, descriptions, chapters, metadata 가 있다
src      :  kind에 따른 소스의 URL 또는 경로
srclang  :  텍스트 트랙의 언어태그 subtitles의 경우 필수 작성
label    :  브라우저에서 사용 가능한 트랙의 이름 목록을 생성
    <code>
    &lt;video controls poster="/images/sample.gif"&gt;
       &lt;source src="sample.mp4" type="video/mp4"&gt;
       &lt;source src="sample.ogv" type="video/ogv"&gt;
       &lt;track kind="captions" src="sampleCaptions.vtt" srclang="en"&gt;
       &lt;track kind="descriptions"
         src="sampleDescriptions.vtt" srclang="en"&gt;
       &lt;track kind="chapters" src="sampleChapters.vtt" srclang="en"&gt;
       &lt;track kind="subtitles" src="sampleSubtitles_de.vtt" srclang="de"&gt;
       &lt;track kind="subtitles" src="sampleSubtitles_en.vtt" srclang="en"&gt;
       &lt;track kind="subtitles" src="sampleSubtitles_ja.vtt" srclang="ja"&gt;
       &lt;track kind="subtitles" src="sampleSubtitles_oz.vtt" srclang="oz"&gt;
       &lt;track kind="metadata" src="keyStage1.vtt" srclang="en"
         label="Key Stage 1"&gt;
       &lt;track kind="metadata" src="keyStage2.vtt" srclang="en"
         label="Key Stage 2"&gt;
       &lt;track kind="metadata" src="keyStage3.vtt" srclang="en"
         label="Key Stage 3"&gt;
       &lt;!-- Fallback --&gt;
       ...
    &lt;/video&gt;
    </code></pre></ul></details>



## &lt;figure&gt;

- 이미지 삽화, 도표, 코드 조각, 인용문, 시 등의 영역을 설정(블럭)

    ```html
    <figure>
        <img src=""
             alt="">
        <figcaption>설명</figcaption>
    </figure>

    <figure>
      <figcaption><cite>설명</cite></figcaption>
      <blockquote>인용 블록</blockquote>
    </figure>
    ```

## &lt;map&gt;

- <area> 와 함께 이미지 맵(클릭 가능한 링크 영역)을 정의할 때 사용(인라인)

    [MDN에서 예제 확인](https://developer.mozilla.org/ko/docs/Web/HTML/Element/map)

-----------
# 내장 콘텐츠

일반적인 멀티미디어 콘텐츠 외에도 다양한 종류의 기타 콘텐츠를 포함할 수 있다  

전역 속성을 사용한다.

## &lt;iframe&gt;

- 다른 HTML 페이지를 현재 페이지에 삽입 (중첩된 브라우저 컨텍스트를 표시)

    ```
    src         : 포함할 문서의 URL 또는 경로
    name        : 프레임의 이름
    width       : 프레임의 가로 너비
    height      : 프레임의 세로 너비
    frameborder : 1 프레임 테두리 사용(default) , 0 프레임 테두리 사용 안함
    srcset      : 브라우저(user agent)에게 제시할 이미지 URL과 원본 크기의 목록을 정의
    sandbox       보안을 위한 읽기 전용으로 삽입
    sandbox     : allow-form, allow-scripts, allow-same-origin 을 설정할 수도 있다
    allowfullscreen  전체 화면 모드 사용 여부
    ```

## &lt;object&gt;

- 이미지나, 중첩된 브라우저 컨텍스트,  

플러그인에 의해 다뤄질수 있는 리소스 외부 리소스를 나타낸다  
&lt;param&gt; 은 &lt;object&gt;의 매개변수를 정의한다  

```html
    <object type="application/pdf"
        data="/media/examples/In-CC0.pdf"
        width="250"
        height="200">
    </object>

    <!-- Embed a flash movie -->
    <object data="move.swf" type="application/x-shockwave-flash"></object>

    <!-- Embed a flash movie with parameters -->
    <object data="move.swf" type="application/x-shockwave-flash">
      <param name="foo" value="bar">
    </object>
```

## &lt;embed /&gt; 

- 영상을 삽입(인라인)

    ```html
    src      :  리소스의 URL 또는 경로
    type     :  인스턴스화할 플러그인을 고르기 위해 사용되는 MIME 타입
    width    :  표시될 가로 너비
    height   :  표시될 세로 너비

    <embed src="/examples/media/sample.pdf">
    <embed type="video/quicktime" src="movie.mov" width="640" height="480">
    ```

- MIME 타입

    ```
    Multipurpose Internet Mail Extensions

    바이너리파일? 이진 데이터 형태 그대로 저장한 것

    인코딩 → 바이너리 파일에서 텍스트(이미지,영상,...) 파일로 변환
    디코딩 → 텍스트 파일에서 바이너리 파일로 변환

    MIME 로 인코딩한 파일은 Content-type 정보를 파일의 앞부분에 담게 된다
    특정 타입은 파일을 웹서버로 부터 전달 받아 웹브라우저에서 열수있다

    text/html, text/javascript, video/webm 등과 같은 형식
    ```

---

## 스크립트

## &lt;script&gt;

- JavaScript, WebGL의 GLSL 셰이더 프로그래밍 언어, JSON 등 외부 스크립트 등 참조

    ```
    src      :  리소스의 URL 또는 경로
    type     :  text/javascript(default) 등 MIME 타입
    defer       문서 파싱(구문 분석) 후 작동 여부
    async       스크립트의 비동기적(Asynchronously) 실행 여부

    src 속성이 있을 때 defer, async 를 사용할 수 있다
    script 참조를 헤드 부분에서 할때 defer를 사용한다
    ```

## &lt;canvas&gt;

- Canvas API이나 WebGL API를 사용하여 그래픽이나 애니메이션을 랜더링  
id 와 보여질 크기를 설정하고 script 에서 그려주면 된다

    ```html
    <!DOCTYPE html>
    <html>
     <head>
      <meta charset="utf-8"/>
      <script type="application/javascript">
        function draw() {
    			// <canvas>태그가 이용가능한 브라우저라면 ID 가 인식되어 값이 할당됨
          var canvas = document.getElementById("canvas");
    			// 값이 있을경우 if 문은 true
          if (canvas.getContext) {
            var ctx = canvas.getContext("2d");
    			// 2d 요소를 그려준다
            ctx.fillStyle = "rgb(200,0,0)";
            ctx.fillRect (10, 10, 50, 50);

            ctx.fillStyle = "rgba(0, 0, 200, 0.5)";
            ctx.fillRect (30, 30, 50, 50);
          }
        }
      </script>
     </head>
     <body onload="draw();">
       <canvas id="canvas" width="150" height="150"></canvas>
     </body>
    </html>
    ```

## &lt;noscript&gt;

- 스크립트를 지원하지 않는 경우에 삽입할 HTML을 정의  
javascript가 활성화 된 상태에서는 태그 안의 콘텐츠가 표시되지 않는다

    ```html
    <noscript>
      <p>Your browser does not support JavaScript!</p>
    </noscript>
    ```
