---
layout: post
title:  "Javascript prototype chain"
date:   2020-03-29 20:31:00 +0900
categories: jekyll update
comments : true
---

## Object.create()
prototype chain은  다른 object를 닮은 object를 만드는 도구 입니다. 메모리를 아끼거나 코드를 줄이기 위해 속성이 똑같은 두개의 object가 필요할때 한 object의 properties를 다른 object로 복사할겁니다. 대신에 Javascript는 prototype chain이란 것을 제공합니다. prototype chain은 field lookup을 위임하여 한 object가 다른 object의 모든 property를 가지고 있는 것처럼 행동하도록 만듭니다.

```
let gold = {a:1};

let rose = Object.create(gold);
rose.b = 2;

console.log(rose.a);

gold.z = 3;

console.log(rose.z);
console.log(gold.b);
console.log(gold.toString());
```
이거 신기한 코드입니다.

분명 현재 상태는 이렇습니다.
```
let gold = {
  a : 1,
  z : 3,
}
let rose = {
  b : 2
}
```
```
1
3
undefined
[object Object]
```
결과는 이렇게 나왔습니다.

그런데 실행해보면 결과는 gold는 b에 접근 하지 못하지만 rose는 a,b,z모두 접근 가능합니다. `Object.create()`라는 함수를 이용해서 두 object 연결 고리가 생긴 것입니다. 다만 하위 object인 rose에서 만든 b는 상위 object인 gold에서 접근 하지는 못하네요 즉, Delegate는 위로만 올라가고 아래로는 안내려갑니다.

---

## Object prototyp

이런 식으로 모든 javascript의 object들은 최상위 object에 접근 할 수 있습니다. 대표적으로 `.toString()`이 있죠 toString() 말고 `constructor`라고 하는 최상위 method도 있습니다. constructor는 어떤 object가 만들어 질때 어떤 함수가 쓰였는지 알 수 있습니다.

```
function Tree(name) {
  this.name = name
}
let theTree = new Tree('Redwood')
console.log('theTree.constructor is ' + theTree.constructor)
```
```
theTree.constructor is function Tree(name) {
  this.name = name
}
```
진짜 함수 만들때 썼던 문자가 그대로 나옵니다.

---

## Array prototype

Array prototype에도 Array를 가리키는 constructor property가 있습니다. Object prototype에서 constructor는 Object를 가리켰죠, Delegate는 위로만 올라가니까 배열을 아무거나 만들고 constructor로 확인 하면 Array constructor를 얻습니다.

> 참고 : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor
>
> 참고 : https://classroom.udacity.com/courses/ud015/lessons/2794468536/concepts/27747885510923
