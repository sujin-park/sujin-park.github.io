---
title: 'CSS 전처리기 (CSS Preprocessor) - SASS'
date: 2020-07-23 15:30:00
category: 'web'
draft: false
---

CSS 전처리기 (CSS Preprocessor) - SASS
========================

### CSS 전처리기란
---

CSS 전처리기(CSS Preprocessor)는 모듈별로 특별한 Syntax를 가지고 있다. 믹스인(mixin), 중첩 셀렉터(nesting selector), 상속 셀렉터(inheritance selector) 등을 통해 CSS를 DRY 방식으로 구조적으로 작업할 수 있게 해준다. 이 CSS 전처리기 자체만으로는 웹 서버가 인지하지 못하기 때문에 각 CSS 전처리기에 맞는 Compiler를 사용해야 하고 컴파일을 하게 되면 실제로 우리가 사용하는 CSS 문서로 변환이 된다.

CSS 전처리기의 종류는 Sass, Less, Stylus가 있으며 이 포스트에는 Sass 를 중점적으로 다룰 예정이다.

### SASS란 무엇인가 ?

---

SASS (Syntactically Awesome Stylesheets)는 CSS 전처리기로 작성하는 스타일시트와 브라우저에서 해석 할 .css 파일 중간에 위치하는 하나의 계층이다. SASS는 DRY (Don't Repeat Yourself) 방식을 적용해서 더 빠르고 효율적으로 코드를 작성하고 쉽게 유지보수 할 수 있도록 도와준다.

### 장점

---

CSS 전처리기 (CSS Preprocessor)의 장점은 재사용성, 시간적 비용 감소, 유지 관리가 있다.


### SASS 문법

---

SASS에는 2개의 문법이 있다. 최신 문법인 SCSS와 SASS가 있고 SCSS 파일은 .scss 파일 확장자를 사용한다.

**SCSS 에제**

```scss
$black: #000;

p {
    font-size: 12px;
    color: $black;
}
```

**초기 SASS 문법**

괄호와 세미콜론을 사용하지 않고, 공백과 들여쓰기로 구조를 만든다. 

```scss
$black: #000;

p
    font-size: 12px
    color: $black
```


### SASS 사용하기

CSS 전처리기에서 중첩(Nesting Rules)과 상속(Extend)을 통해 구조화 할 수 있다.

**중첩(Nesting Rules)**

```scss
header {
    margin: 24px 0 36px;
    border-bottom: solid 1px #333;

    h1 {
        padding: 15px 0;
        font-size: 50px;
        font-weight: bold;
    }   
}
```

위 코드는 다음과 같이 컴파일된다.

```scss
header {
    margin: 24px 0 36px;
    border-bottom: solid 1px #333;
}
header h1 {
    padding: 15px 0;
    font-size: 50px;
    font-weight: bold;
}   
```
**네임스페이스 속성 중첩하기**

Sass에서는 공통된 네임스페이스의 속성도 중첩이 가능하다.

```scss
header {
    padding: 16px 0;
    font: {
        size: 50px;
        family: Jubilat, Georgia, serif;
        weight: bold;
    }
    line-height: 1;

    background: {
        transform: uppercase;
        decoration: underline;
        align: center;
    }
}
```

**&로 부모 선택자 참조하기**

&를 사용해서 부모 선택자를 참조할 수 있다.

```scss
a {
    font-weight: bold;
    text-decoration: none;
    &:hover {
        color: #000;
        text-decoration: underline;
    }
}
```

```scss
section {
    margin: 0 0 20px 0;
    font-size: 16px;
    line-height: 1.5;

    body.store & {
        font-size: 14px;
        line-height: 1;
    }
}
```

위 코드는 아래와 같이 컴파일된다.

```scss
section {
    margin: 0 0 20px 0;
    font-size: 16px;
    line-height: 1.5;
}
body.store section {
    font-size: 14px;
    line-height: 1;
}
```

### 믹스인
믹스인은 Sass 기능 중 가장 많이 사용하는 기능으로 여러가지 반복적인 작업을 @mixin 을 통해 중복 코드를 줄일 수 있다.

mixin 을 선언할 때는 `@mixin` 지시자를 사용하며, 적용 시 `@include` 지시자를 사용한다.

```scss
// mixin 선언
@mixin title-style {
    margin: 20px 0;
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
}

// mixin 사용
section.main h2 {
    @include title-style;
}
```

위 코드는 아래와 같이 컴파일된다.

```scss
section.main h2 {
    margin: 20px 0;
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
}
```

**믹스인 인자**

믹스인을 호출하면서 인자를 전달할 수 있다. 믹스인을 정의할 때 괄호 안에 변수를 넣어 인자를 지정한다. 인자는 기본값을 정의할 수 있으며 mixin을 사용할 때 인자로 다른 값으로 대체할 수 있다.
```scss
// green 으로 기본값 지정

@mixin title-style($color: green) {
    margin: 20px 0;
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
    color: $color;
}

section.main h2 {
    @include title-style(#000);
}
```

위 코드는 아래와 같이 컴파일된다.

```scss
section.main h2 {
    margin: 20px 0;
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
    color: #000;
}
```

### 참고
- 웹디자이너를 위한 SASS | 댄 시더홈 저
- [CSS 전처리기(Pre-Processor) 배우기!](https://kdydesign.github.io/2019/05/12/css-preprocessor/)