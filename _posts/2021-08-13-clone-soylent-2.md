---
title: clone-soylent-2
author: YHole
date: 2021-08-13 +0900
categories: [Front end, practical exercise]
tags: [clone, soylent, practical]
---

## clone 내용

![section](/assets/img/soylent/section1.PNG)

## 파악하기

- hover시 움직이는 border
- hover시 움직이는 화살표
  - 화살표의 경우 일부만 늘어나기 때문에 그림 파일 사용 불가

## 구조 만들기

- 뒷 배경 부분

- 박스 부분
  - 왼쪽에 위치할 박스
  - border, background-color

- 박스 (내부)
  - h1 제목
  - p 내용
  - SHOP NOW 박스

- SHOP NOW 박스
  - hover 시 작아지는 border
  - hover 시 늘어나는 arrow

## 스타일 만들기

### background

- 뒷 배경 설정
  - 높이 설정
  - -image url('') 주소를 통한 이미지 삽입
  - -repeat stretch 반복 여부를 늘려주기로
  - -size cover 크기는 영역 커버
  - -position left 왼쪽을 기준으로 설정

### 박스 만들기

```css
{
  width: 505px;
  height: 495px;
  border: 4px solid #212322;
  background-color: #B9E8A2;
  margin-top: 5.9%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  opacity: 0.98;
  box-sizing: border-box;
}
```

### 움직이는 border

- 기본 박스의 자식 요소로 박스를 하나 더 만든다
  - 기본 박스 = border 없는 박스
  - 자식 박스 = border 만 있는 박스

```css
.border-move{
  position: absolute;
  border: 2px solid #212322;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  margin: auto;
  transition: width 0.4s,
              height 0.4s;
}
a:hover .border-move{
  width: 92%;
  height: 70%;
}
```

### 화살표 만들기

- 실무에서 사용하는 방법인지는 모르겠다
- 움직이는 화살표를 만들기 위해서는  
머리와 몸통을 따로 만들어야만 한다

```css
.arrow-grow{
  position: absolute;
  top: 3px;
  left: 140px;
  width: 10px;
  height: 10px;
  margin-top: 18px;
}
.arrow-body{
  border-top: 2px solid transparent;
  position: absolute;
  top: 5px;
  left: 0;
  width: 34px;
  background: #212322;
  transition: .4s;
}
.arrow-body .arrow-head {
  border-top: 2px solid #212322;
  border-right: 2px solid #212322;
  width: 10px;
  height: 10px;
  top: -6px;
  left: 24px;
  position: absolute;
  transform: rotate(45deg);
  box-sizing: border-box;
  background: transparent;
  transition: .4s;
}
```

### 움직이는 화살표

- 따로 만든 부분에 효과를 주면 완성
  - 몸통은 늘이는 방법으로
  - 머리는 이동하는 방법으로

- 자연스럽게 화살표가 늘어나는 것처럼 보인다

```css
a:hover .arrow-body{
  width : 80px
}
a:hover .arrow-head{
  left: 70px;
}
```

## Javascript

- 불필요


## 느낀점

- 내용 자체는 매우 간단했고 header 영역의 작업에 간 보다 적게 걸렸다
- 콘텐츠가 많지 않다 보니 구조 작성도 간단했다

- 의외의 복병은 움직이는 border와 화살표였다
- 아이디어를 생각해 내야 하는 부분과 참조를 찾는 데에 시간이 걸렸다
- 독특한 효과를 빠르게 파악하는 능력이 필요할 것 같다
