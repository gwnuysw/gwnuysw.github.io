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

---
# closure

함수가 리턴된 뒤에도 스코프 접근이 가능한 모든 함수를 말한다.
```
var sagas = [];
var hero = aHero();
var newSaga = function(){
  var foil = aFoil();
  sagas.push(function(){
      var deed = aDead();
      log(hero+deed+foil);
    });
};
newSaga();
sagas[0]();
sagas[0]();
newSaga();
```
sagas[0]\();함수는 newSaga();가 종료되고 리턴됬는데도 해당 스코프에 접근하고 있다.

---
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

---

# Function Expression

자바스크립트의 변수는 뭐든지 들어갈수 있습니다. 심지어 함수까지도 변수에 assign될수 있습니다! 그런 경우 이 변수를 **Function Expression**이라고 하고 함수를 무명함수(anonymous function)라고 합니다.
```
var catSays = function(max) {
  // code here
};
```

Function Expression은 변수에 assign 연산자가 있기 때문에 호이스팅 되지 않습니다.

### callback

변수에 assign된 함수는 다른함수의 인자로 넘겨주기 쉽습니다. 이렇게 다른 함수에 인자로 넘겨진 함수를 callback이라고 합니다.

### 이름있는 function expression
function expression의 함수 이름을 지정할 수 있지만 변수이름으로만 호출가능하고 함수이름으로 호출할 수는 없습니다.
```
var FavoriteMovie = function movie(){
  return "the fountain"
};
```

### inline function expression

함수의 인자로 함수의 선언을 작성 할 수 있는데 이를 inline function expression 이라고 합니다. 그러면 함수의 매개변수가 바로 function expression이 되는 것입니다. 또한 inline function expression은 무명 함수 일 수도 있습니다.


참조(reference) : udacity
