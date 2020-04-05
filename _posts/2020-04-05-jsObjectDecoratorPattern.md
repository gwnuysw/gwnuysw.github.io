---
layout: post
title:  "Code Reuse의 다양한 패턴들"
date:   2020-04-05 09:19:00 +0900
categories: jekyll update
comments : true
---

##  Decorator Function
```
var amy = {loc:1};
amy.loc++;
var ben = {loc:9};
ben.loc++;
```
위코드에서 반복되는 부분을 함수를 이용해서 줄일 수 있다.
```
var carlike = function(obj, loc){
  obj.loc = loc;
  obj.move = function(){
    obj.loc++;
  };
  return obj;
};
var amy = carlike({}, 1);
amy.move();
var ben = carlike({}, 9);
ben.move();
```

`move`메소드 에서 `this`를 쓰지 않고 `obj.loc++`한 이유는 carlike함수를 실행 할 때마다. 매번 유일한 closure 스코프가 생기기 때문입니다. 메모리는 많이 사용하지만 code 재사용 측면에서 약간의 장점이 있는 코드입니다.

## Functional Classes
```
var Car = function(loc){
  var obj = {loc: loc};
  obj.move = move;
  return obj;
};
var move = function(){
  this.loc++;
};
var amy = Car(1);
amy.move();
var ben = Car(9);
ben.move();
```
decorator 패턴과 큰 차이는 없지만 Car함수 내에서 Object를 만든다는 것과 이름이 대문자로 시작한다는 점이다. 똑같은 함수같지만 Class라고 하고 이때 Car()를 호출하면 인스턴스를 만드는거다.

### Functional shared pattern

move 함수를 밖으로 뺸거는 매번 move함수가 만들어 져서 메모리가 낭비되는 것을 막기 위함.(왜 이랬다 저랬다 하니?) 이를 functional shared pattern 이라고 한다.

그렇다. 이런식으로 전역 영역을 사용하는 것은 더럽기 때문에 또다른 테크닉을 사용한다.

```
var Car = function(loc){
  var obj = {loc: loc};
  extend(obj, Car.methods);
  return obj;
};
Car.methods = {
  move : function(){
    this.loc++;
  }
};
```
여기서 `extend`함수는 상상의 함수다. 그런데 다른 라이브러리에 이런 역할을 하는 것들이 많으니 찾아서 쓰면 된다.

## Prototypal Classes

이와 유사한데 move메소드를 프로토타입 체인에 넣는 방법도 있다. 이렇게 하면 extend로 모든 메소드를 복사하는 작업이 없어도 된다.
```
var Car = function(loc){
  var obj = Object.create(Car.methods);
  obj.loc = loc;
  return obj;
};
Car.methods = {
  move : function(){
    this.loc++;
  }
};
```
옛날에 이러한 방식이 너무 유용하고 많이 쓰여서 랭귀지에서 이런 기능을 아예 만들어 줬다.

```
var Car = function(loc){
  var obj = Object.create(Car.prototype);
  obj.loc = loc;
  return obj;
};
Car.prototype.move = function(){
  this.loc++;
};
var amy = Car(1);
amy.move();
var ben = Car(9);
ben.move();
```
이떄 Car의 prototype과 amy의 prototype은 같은 의미의 prototype이 아니다. amy의 프로토타입은 field lookup을 Car.prototype으로 하는 거고 Car함수의 prototype은 Car.prototype으로 가지고 있을 뿐이다.

신기하게도 prototype의 constructor를 확인해 보면 Car함수를 반환한다.
```
console.log('this : '+Car.prototype.constructor);
console.log('this : '+amy.constructor);
log(amy instanceof Car);
```
instanceof를 확인해 보면 true로 나오는데 이것은 prototypal class여야만 제대로 나온다.

## Pseudoclassical pattern

여기서 랭귀지는 다른 언어의 표현 형식을 본따서 Prototypal Classes pattern의 반복적인 코드를 더 줄였다.
```
var Car = function(loc){
  //this = Object.create(Car.prototype);
  this.loc = loc;
  //return this;
};
Car.prototype.move = function(){
  this.loc++;
};
var amy = new Car(1);
amy.move();
var ben = new Car(9);
ben.move();
```
`new` 키워드를 이용하면 오브젝트만드는 것과 리턴을 자동으로 해준다.

## Functional classes subclass

```
var Car = function(){
  var obj = {loc: loc};
  obj.move = function(){
    obj.loc++;
  };
  return obj;
};
var Van = function(loc){
  var obj = Car(loc);
  obj.grab = function(){/*...*/};
  return obj;
};
var Cop = function(loc){
  var obj = Car(obj);
  obj.call = function(){/*...*/};
  return obj;
};
var cal = Cop(2);
cal.move();
cal.call();
```

## Pseudoclassical subclass

```
var Car = function(loc){
  //this = Object.create(Car.prototype);
  this.loc = loc;
  //return this;
};
Car.prototype.move = function(){
  this.loc++;
};
var Van = function(loc){
  cal.call(this,loc);
}
var amy = new Car(1);
amy.move();
var ben = new Van(9);
ben.move();
```

이렇게 call 메소드를 이용하면 Car의 this는 Van이 this를 가리키게 된다. 그래서 Car함수를 호출 하더라도 Van함수와 같은 object 스코프를 가지게 된다. call메소드의 첫번째 매개변수가 호출된 함수의 this가 된다.
