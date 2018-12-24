---
layout: post
title:  "자바스크립트의 데이터타입"
date:   2018-09-29 14:42:00 +0900
categories: jekyll update
comments : true
---

# 문자열
## 자바스크립트에서의 따옴표

 문자열을 나타내는데 큰따옴표나 작은 따옴표나 상관이 없다.

## 문자열 인덱스

```
"James"[0];
```

> Returns: "J"

```
var name = "James";
name[0];
```

>Returns: "J"

## escaping strings

|code|Character|
|----|---------|
|\\\\ | \\(backslash)|
|\\" | "(double quote)|
|\\' | '(single quote)|
|\n | new line|
|\t | tab|

---
# null, undefined, NaN

**null**은 아무것도 없다는 값이고

```var x = null;```

**undefined**는 값이 없다는 뜻입니다.
```
var x;
console.log(x);
```
> Returns: undefined


**NaN**
"Not-A-Number"라는 뜻으로 주로 숫자 연산의 오류를 나타냅니다.
```
// calculating the square root of a negative number will return NaN
Math.sqrt(-10)

// trying to divide a string by 5 will return NaN
"hello"/5
```

---
# Equality

자바 스크립트에서 자료형은 var로 통일입니다. 그래서 다음과 같은 신기한 결과를 내기도 합니다.
```
"1" == 1
```
>Returns: true

```
0 == false
```
>Returns: true

하지만 강한 타입 비교를 해야 할때도 있습니다. 그럴때는 '==='를 사용합니다.

```
"1" === 1
```
>Returns: false

```
0 === false
```
>Returns: false

참조(reference) : udacity
