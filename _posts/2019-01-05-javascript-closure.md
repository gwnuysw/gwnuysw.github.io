---
layout: post
title:  "자바스크립트의 closure"
date:   2019-01-05 14:52:00 +0900
categories: jekyll update
comments : true
---

이것 역시 gist에 올렸던건데 보충 설명해서 다시 포스팅 합니다.

**closure**

> Every function should have access to all the variables from all the scopes that surraound it.
> a closure is just any function that somehow remains available after those outer scopes have returned

함수가 또다른 함수를 리턴하는 순간 closure가 만들어 집니다.

* 함수 내부의 lexical-socpe는 외부 lexical-scope의 변수 참조 가능
* 함수 내부의 lexical-scope를 외부 lexical-scope에서 참조 불가능
* 리턴된 내부 lexical-scope는 외부에서 참조 가능
* 리턴된 내부 lexical-scope는 in-memory-scope변수 규칙을 따른다.

스토얀 스테파노프의 'javascrpt patterns'라는 책에 보면 closure는 반환 되는 함수에서는 접근 할수 있지만 코드 외부에서는 접근할 수 없기 때문에, 비공개 데이터 저장을 위해 사용할수 있다고 하네요.

예를 들면 이런겁니다.
```
var serup = function () {
  var count = 0;
  return function () {
    return (count += 1);
  };
};

var next = setup();
next();   //1
next();   //2
next();   //3
```

다른건 알겠는데 왜 비공개 데이터 저장을 위해 사용한다는 건지는 아직 이해가 안돼네요. 외부에서 스코프 내부 변수 접근 못하는 건 똑같지 않은가라는 의문이 듭니다.
