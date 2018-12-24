---
layout: post
title:  "자바스크립트의 오브젝트"
date:   2018-09-30 09:38:00 +0900
categories: jekyll update
comments : true
---

# object 작성시 유의사항

프로퍼티 이름에 숫자를 맨앞에 쓰거나 특수문자, 공백을 피하고 따옴표를 프로퍼티 이름에 사용할수 있으나 사용하지 말자.

```
var obj = {
  1property : 1,
  @property : 2,
  thrid property : 3,
  "fourth"property : 4s
}
```
위와 같은 형식을 사용하지 말자 당연한 얘기

---

# Prototype chain

프로토타입 체인이란 다른오브젝트와 닯은 오브젝트를 만드는 메카니즘이다.

## ongoing look-up time delegation


```
var gold = {a:1};

var rose = Object.create(gold);

rose.b = 2;

console.log(gold.a);
console.log(gold.b);

console.log(rose.a);
console.log(rose.b);
```
>1
>
>undefined
>
>1
>
>2

Object.create함수는 새로운 rose를 만들어 주는 동시에 rose가 gold를 참조할 수 있도록 한다.
rose.a는 gold.a를 참조하며 rose.b는 gold.b에 없다.
![Prototype chain](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/javascript/prototype_chain.png?raw=true)

반대로 참조되고 있는 오브젝트의 프로퍼티를 추가한 경우
```
var gold = {a:1};

var rose = Object.create(gold);

rose.b = 2;

gold.z = 3;

console.log(gold.z);
console.log(rose.z);
```
>3
>
>3



모든 object는 'Object'프로토타입이라는 공통 조상을 가집니다.
