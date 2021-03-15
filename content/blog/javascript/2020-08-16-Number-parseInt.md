---
title: 'Number와 parseInt 의 차이'
date: 2020-08-16 16:30:00
category: 'javascript'
draft: false
---

### Number

Number는 문자열을 인자로 받으면 해당 문자열을 숫자로 변환해준다.

아래의 코드는 '10000' 라는 문자열을 10000이라는 숫자로 형변환하여 변수 num에 담는 코드이다.

```jsx
const num = Number('10000');
```

하지만 아래처럼 문자열이 **숫자가 아닌 경우 num에는 NaN이 저장된다.**

```jsx
const num = parseInt('10000원'); // NaN
```


### parseInt

기본적으로 Number(str)와 동일하게 문자열을 인자로 받으면 해당 문자열을 숫자로 바꿔준다.

아래의 코드는 '10000' 라는 문자열을 10000이라는 숫자로 형변환하여 변수 num에 담는 코드이다.

```jsx
const num = parseInt('10000');
```


### Number와 parseInt 의 차이

문자열이 숫자가 아닌 경우가 Number()와 조금 다른데 parseInt는 문자열이 **숫자로 시작하는 경우에는 숫자가 끝날때 까지만 형변환을 하여 num에 저장**된다. 시작이 숫자가 아니면 Number()와 마찬가지로 num에 NaN이 저장된다.

```jsx
const num = parseInt('1000원'); // 1000
const num = parseInt('가격:10000원'); // NaN
```