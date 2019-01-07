---
layout: post
title:  "자바스크립트의 scope"
date:   2019-01-05 14:49:00 +0900
categories: jekyll update
comments : true
---

예전에 gist에 올렸던건데 까먹고 있었네요 다시 포스팅합니다.

**lexical Scope**

* 오직 함수 선언의 중괄호만이 새로운 scope를 만들어 낸다.
* 반복문이나 조건문 등의 중괄호는 새로운 scope를 만들어 낼수 없다.
* 여러개의 javascript file에서 global scope 영역은 공유됩니다.
* 내부 스코프가 외부 스코프를 참조 할수 있지만 외부 스코프가 내부 스코프를 참조 할 수는 없다.

```
var a = 1;
var b = function(){ //내부에서 외부 스코프의 변수 a참조 가능
  var c = d();  //var 키워드는 생략 하면 외부 스코프에서 자동으로 선언문이 생성되지만 좋은 습관이 아니다.
};
log(c); //그러나 이 문장은 실행 될 수 없다.
```

**in-memory data stores**
* memory scope와 lexical scope는 다르다.

```
var a = 1;
var b = function(){
  var c = d();
  var e = function(){
    var f = g();
    log(a+f+c);
  };
  e();
  e();
};
b();
b();
>>>>>>>>in-memory scope<<<<<<<<
--------------------------------------global scope
a = 1;
b = {function};
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!b scope
c = d();
e = {function};
**************************************e scope
f = g();
log(a+f+c)//log message : (1g()d())
**************************************e scope
**************************************e scope
f = g();
log(a+f+c)//log message : (1g()d())
**************************************e scope
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!b scope
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!b scope
c = d();
e = {function};
**************************************e scope
f = g();
log(a+f+c)//log message : (1g()d())
**************************************e scope
**************************************e scope
f = g();
log(a+f+c)//log message : (1g()d())
**************************************e scope
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!b scope
--------------------------------------global scope ^
```
>udacity object oriented javascript ud 117 강의를 참조 했습니다.
