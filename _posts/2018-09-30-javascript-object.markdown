---
layout: post
title:  "자바스크립트의 오브젝트"
date:   2018-09-30 09:38:00 +0900
categories: jekyll update
comments : true
---

# object 작성시 유의사항

프로퍼티 이름에 숫자를 맨앞에 쓰거나 특수문자, 공백을 피하고 따옴표를 프로퍼티 이름에 사용할수 있으나 사용하지 말자.

---

# Prototype chain

프로토타입 체인이란 다른오브젝트와 닯은 오브젝트를 만드는 메카니즘이다.

## 1-time prototype copying

```
var gold = {a:1};

var blue = extend({},gold);
```

extend함수는 지속적인 오브젝트 복사가 아니라 단 한번만 복사가 이루어진다. 복사한후 gold의 프로퍼티를 수정하면 blue에는 아무런 변화가 없다.

## ongoing look-up time delegation

```
var gold = {a:1};

var blue = extend({},gold);

var rose = Object.create(gold);
rose.b = 2;
```

Object.create함수는 새로운 object rose를 만들어 주는 동시에 rose가 gold를 delegate field lookup할수 있도록 한다.
rose.a는 gold.a를 참조하며 rose.b는 gold.b에 없다.
![Prototype chain](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/javascript/prototype_chain.png?raw=true)


모든 object는 'Object'프로토타입이라는 공통 조상을 가집니다.
