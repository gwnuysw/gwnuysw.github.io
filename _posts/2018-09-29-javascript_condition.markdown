---
layout: post
title:  "자바스크립트의 Conditionals"
date:   2018-09-29 15:19:00 +0900
categories: jekyll update
comments : true
---


**Truthy and falsy**

자바스크립트의 모든 변수값은 boolean정보가 내포되어있습니다.

**falsy value**

1. ""
2. null
3. undefined
4. 0
5. false
6. NaN

**truty value**

1. 0 , 0.0 을 제외한그냥 숫자
2. 문자열안에 문자가있을때 "null", "undefined"모두 true
3. 중괄호와 대괄호안에 아무것도 없어도 truthy {}, []

**switch**

자바스크립트의 switch 문은 c언어에서와 유사하게 break문과 default문이 있다. break 문을 사용하지 않는다면 falling-thruogh된다.

참조(reference) : udacity
