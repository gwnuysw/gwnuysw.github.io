---
layout: post
title:  "자바스크립트의 함수"
date:   2018-09-29 15:56:00 +0900
categories: jekyll update
comments : true
---

# scope

* 오직 함수 선언의 중괄호만이 새로운 scope를 만들어 낸다.
* 반복문이나 조건문 등의 중괄호는 새로운 scope를 만들어 낼수 없다.
* 여러개의 javascript file에서 global scope 영역은 공유됩니다.
* 내부 스코프가 외부 스코프를 참조 할수 있지만 외부 스코프가 내부 스코프를 참조 할 수는 없다.

```
if(window){
    var x = 213;
}
alert(x);
```

C언어나 Java에서 변수 x는 if문 외부에서 참조 할수 없지만 javascript는 가능합니다. if문 뿐만 아니라 for문도 외부 영역에서 내부 변수를 참조 할 수 있습니다.

# 함수 선언

```
function isNimble(){return true;}; //이름을 가진 함수
console.log(isNimble());
var canFly = function(){ return true;}; //익명 함수
console.log(canfly());
window.isDeadly = function(){ return true;}; //프로퍼트에 익명 함수를 선언
console.log(window.isDeadly());
function outer(){
  console.log(inner());
  function inner(){return true;};
  console.log(window.inner());
}
outer();
console.log(window.inner());
window.wieldsSword = function swingSword() { return true;}; //인라인 함수
console.log(window.wieldsSword.name);
```

### Function Expression, 익명함수

자바스크립트의 변수는 뭐든지 들어갈수 있습니다. 심지어 함수까지도 변수에 assign될수 있습니다! 그런 경우 이 변수를 **Function Expression** 이라고 하고 **이름없는 함수를 익명함수(anonymous function)** 라고 합니다.

```
var catSays = function(max) {
  // code here
};
```

Function Expression은 변수에 assign 연산자가 있기 때문에 호이스팅 되지 않습니다.

### inline function

익명 함수의 함수 이름을 지정할 수 있지만 변수이름으로만 호출가능하고 함수이름으로 호출할 수는 없습니다.
```
var FavoriteMovie = function movie(){
  return "the fountain"
};
```

# 호이스팅(hoisting)

함수 정의나 변수의 선언은 코드가 해석될때 자동으로 스코프의 상위로 재배치된다. 이를 호이스팅이라고 한다. 변수의 assign은 호이스팅 되지 않기때문에 조심해야한다.
```
sayHi("Julia");

function sayHi(name) {
  console.log(greeting + " " + name);
  var greeting;
}
```
위 코드는 아래와 같이 재배치 된다.
```
function sayHi(name) {
  var greeting;
  console.log(greeting + " " + name);
}

sayHi("Julia");
```
>Returns: undefined julia

```
sayHi("Julia");

function sayHi(name) {
  console.log(greeting + " " + name);
  var greeting = "Hello";
}
```
위 코드는 아래와 같이 재배치 된다.
```
function sayHi(name) {
  console.log(greeting + " " + name);
  var greeting = "Hello";
}

sayHi("Julia");
```
>Returns: undefined julia

다음은 의도대로 정상 실행 되는 경우다.
```
function sayHi(name) {
  var greeting = "Hello";
  console.log(greeting + " " + name);
}

sayHi("Julia");
```
>Returns: Hello julia

# closure

내부 함수가 외부 변수에 저장 되어서 외부 함수가 리턴된 뒤에도 접근 할 수 있는 외부 함수 스코프.
이런 경우는
1. passing to setTimeout
2. 함수를 return할때
3. 외부 변수에 함수를 저장할때

가 있다.
```
var outerValue = 'ninja';
var later;
function outerFunction() {
  var innerValue = 'samurai';
  function innerFunction() {
    console.log(outerValue);
    console.log(innerValue);
  }
  later = innerFunction;
}
outerFunction();
later();
```
later함수는 outerFunction이 종료되고 리턴됬는데도 해당 스코프에 접근하고 있다. 자바스크립트에서 정보은닉은 closure를 이용합니다.




참조(reference) : udacity
