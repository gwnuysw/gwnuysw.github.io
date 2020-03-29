---
layout: post
title:  "Javascript Object Basics from MDN docs"
date:   2020-03-29 19:44:00 +0900
categories: jekyll update
comments : true
---

## Object Basics

```
const person = {
  name: ['Bob', 'Smith'],
  age: 32,
  gender: 'male',
  interests: ['music', 'skiing'],
  bio: function() {
    alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  },
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
```

이건 **Object literal** 이고 name, gender, interests는 **properties**고, 그 아래 함수 두개는 **methods**다. 나중에 나오는 클래스와 인스턴스와는 다른 것이니 잘 기억하자. 이름과 값은 콜론으로 구분하고 각 이름/값 쌍은 콤마로 구분한다.

---

## Dot Notation

Object의 properties와 methods는 Dot Notation으로 접근 가능

```
person.age
person.interests[1]
person.bio()
```

person 오브젝트의 name을 배열이 아니라 오브젝트로 바꾼다면
```
name : {
  first: 'Bob',
  last: 'Smith'
},
```

접근 하는 방법은 아래와 같다.
```
person.name.first
person.name.last
```

---
## Bracket Notation

```
person.age
person.name.first
```

```
person['age']
person['name']['first']
```

위와 아래 표현은 object의 properties에 접근하는 똑같은 표현이다. 배열에 접근하는 방법과 유사한데 그래서 가끔 object를 **associative array** 라고도 불린다.

---

## Setting object members

```
person.age = 45;
person['name']['last'] = 'Cratchit';
```
이런 식으로 object 값을 변경할 수도 있고
```
person['eyes'] = 'hazel';
person.farewell = function() { alert("Bye everybody!"); }
```
이런 식으로 새로운 property와 method를 만들수도 있다.

---

## What is "this"?

자바 같은 oop언어에서의 this와 조금 다르다. 그냥 `person1.greeting()` 이때 greeting 안에 this가 있다면 그 this는 온점 왼쪽의 object를 가리킨다. (제 3의 매개변수라고 생각하세요)

>참고 : https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics
