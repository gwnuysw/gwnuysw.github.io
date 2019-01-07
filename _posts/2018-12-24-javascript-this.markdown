---
layout: post
title:  "자바스크립트의 this"
date:   2018-12-24 15:37:00 +0900
categories: jekyll update
comments : true
---

### this 키워드

this는 다음과 같이 object 점연산자(.)의 왼쪽에 있는 object에 바운드 됩니다. 마치 매개변수와도 같다고 생각하면 쉽게 이해 됩니다. 매개변수이긴 한데 암묵적으로 인수가 넘겨지는 것이지요.

```
var fn = function(a, b){
  console.log(this);
};
var obj = {
  fn: function (a, b){
    console.log(this);
  }
};
var ob1 = {
  method : obj.fn
};
var ob2 = {
  method : fn
};

ob1.method(4,5);
ob1['method'](4,5);
ob2.method(1,2);
obj.fn(3, 4);
```

>{ method: [Function: fn] }
>
>{ method: [Function: fn] }
>
>{ method: [Function: fn] }
>
>{ fn: [Function: fn] }

`ob1['method'](4,5)`는 ob1.`method(4,5)`와 같은 표현입니다.

### 다양한 상황에서 this 키워드

```
var fn = function(one, two){
  console.log(this, one, two);
};

var r = {red : 'red'};
var g = {green : 'green'};
var b = {blue : 'blue'};
var y = {yellow : 'yellow'};
```
위와 같이 함수와 object가 있을때

**첫째**

```
r.method = fn;

r.method(g,b);
```
>{ red: 'red', method: [Function: fn] } { green: 'green' } { blue: 'blue' }

r.method의 점 왼쪽 robject가 this에 바운드 되었습니다.

**둘째**
```
fn(g, b);
```
이경우 점연산자도 없고 따라서 점왼쪽에 있는 object가 뭔지 알 수 없지만 자바스크립트는 default로 global object를 this에 바운드 시킵니다.

노드에서 실행하면
>{ console: [Getter], ...(중략)...
  setTimeout: { [Function: setTimeout] [Symbol(util.promisify.custom)]: [Function] } } { green: 'green' } { blue: 'blue' }

 이런 결과가 나오고 크롬 브라우저의 콘솔에서 실행하면

 >Window {postMessage: ƒ, blur: ƒ...(중략)... {green: "green"} {blue: "blue"}

 이런 결과가 나옵니다 두 환경에서 global object는 다른가 봅니다.

**셋째**

함수가 object의 property는 아니지만 (메소드는 아니지만) this에 특정 object를 바인드 시키고 싶을때가 있다고 합니다.

```
fn.call(r,g,b);
```
위와 같이 call메소드를 호출하여 첫번째 매개변수로 object를 주면 this는 그 object에 바운드 됩니다.
>{ red: 'red', method: [Function: fn] } { green: 'green' } { blue: 'blue' }

이미 어떤 object의 property라고 해도 call을 사용하면 this에 다른 object를 바운드 시킬 수 있습니다.
```
r.method.call(y, g, b);
```
>{ yellow: 'yellow' } { green: 'green' } { blue: 'blue' }

**넷째**

callback으로 넘겨진 fn과 r.method
```
setTimeout(fn, 1000);
setTimeout(r.method, 1000);
```
이경우도 node에서의 실행과 크롬브라우저에서의 실행 결과가 조금 다릅니다. 노드에서는 Timeout object가 this에 바운드 되고
> Timeout {
  _called: true,
  _idleTimeout: 1000,
  _idlePrev: null,
  _idleNext: null,
  _idleStart: 73,
  _onTimeout: [Function: fn],
  _timerArgs: undefined,
  _repeat: null,
  _destroyed: false,
  [Symbol(asyncId)]: 4,
  [Symbol(triggerAsyncId)]: 1 } undefined undefined


크롬 브라우저에서는 global object가 this에 바운드됩니다.
> Window {postMessage: ƒ, blur: ƒ...(중략)... undefined undefined


**다섯째**

inline function expression을 사용하여 선언된 함수 내부의 경우
```
setTimeout(function(){
  r.method(g, b)
}, 1000);
```
> { red: 'red', method: [Function: fn] } { green: 'green' } { blue: 'blue' }

명확하게 점 연산자 다음 r객체가 있습니다.

**여섯째**

```
new r.method(g,b);
```
new 키워드는 새로운 object를 만들기 때문에 this는 이 새로운 object에 바운드 된다고 합니다. 그런데 앞에 붙는 fn은 무슨 의미인지 모르겠네요
> fn {} { green: 'green' } { blue: 'blue' }


예전에 제가 작성했던 gist문서입니다. 열심히 써놓고 깜빡하고 있었네요
[https://gist.github.com/gwnuysw/4c1ea3e9be9adb11381c62ef3bdc6acc](https://gist.github.com/gwnuysw/4c1ea3e9be9adb11381c62ef3bdc6acc)
