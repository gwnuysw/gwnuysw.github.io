---
layout: post
title:  "중급자용 Javascript"
date:   2019-03-11 19:56:00 +0900
categories: jekyll update
comments : true
---

# 소개

[자바스크립트의 역사](https://wit.nts-corp.com/2014/08/13/1925)

제 생각에 자바스크립트의 가장 큰 특징은 정체성이 없다는 것입니다. C언어는 유닉스 시스템을 위해 만들어 졌고, Python은 [분명한 철학](https://gist.github.com/Nesffer/30651e6197f03eb029720a0e5b1e0c22)이 있습니다. 그러나 자바스크립트는 인간이 일상 생활에서 사용하는 진짜 언어 처럼 시대에 따라 변화 하는 것 같습니다.

**자바스크립트의 구성**
(참고 : 자바스크립트 닌자 비급)

- 객체
- 함수
- 클로져

## 강의 순서

1. [자바스크립트의 데이터 타입](https://gwnuysw.github.io/jekyll/update/2018/09/29/javascript-datatype.html)
2. [자바스크립트의  Conditionals](https://gwnuysw.github.io/jekyll/update/2018/09/29/javascript_condition.html)
3. [자바스크립트의 함수](https://gwnuysw.github.io/jekyll/update/2018/09/29/javascript_function.html)
4. [자바스크립트의 this, arguments](https://gwnuysw.github.io/jekyll/update/2018/12/24/javascript-this.html)
5. 클로저
## 함수가 핵심이다.

자바스크립트는 **1종 객체(first-class Object)** 이다.

- 리터럴로 생성될 수 있습니다.
- 변수, 배열 엘리먼트, 다른 객체의 프로퍼티에 할당될 수 있습니다.
- 함수의 인자로 전달될 수 있습니다.
- 함수의 결과값으로 반환될 수 있습니다.
- 동적으로 생성된 프로퍼티를 가질 수 있습니다.

자바스크립트의 함수 작동 원리와 **[이벤트 루프](https://meetup.toast.com/posts/89)**

- 함수가 실행의 기본 모듈 단위 입니다.
- 함수는 비동기 상태로 호출됩니다.

  프로그램 실행 도중 함수를 호출하면 프로그램 문맥은 해당 함수가 반환될때 까지 기다리지 않고 다음 작업으로 넘어갑니다. 호출 스택에 있는 함수는 순서대로 백그라운드로 넘어가고 백그라운드 작업이 끝나면 태스크 큐로 옮겨집니다. 이벤트 루프는 현재 실행중인 태스크가 없는지, 태스크 큐에 태스크가 있는지 반복적으로 확인하는 것입니다.

자바스크립트는 **단일 Thread** 다?

  자바스크립트가 단일 스레드 라는 말은 자바스크립트 엔진이 단일 호출 스택을 사용한다는 관점에서만 사실입니다. 그러나 실제 자바스크립트 구동환경은 여러 쓰레드가 사용되며, 이때 자바스크립트의 단일 호출 스택과 연동하기 위해 사용하는 장치가 이벤트 루프입니다.

**논블로킹 I/O**

이벤트 루프를 이용하면 오래 걸리는 작업을 효율적으로 처리할 수 있습니다. 오래 걸리는 함수를 백그라운드로 보내 버리면 되죠! 논블로킹이란 이전 작업이 완료될 때까지 멈추지 않고 다음 작업을 수행함을 뜻합니다.

**Call back** 함수와 **Promise, async await**

Array.sort()와 같이 자바스크립트의 call back 함수는 코드를 간결하게 만들어 줍니다. 자바스크립트의 비동기 함수 호출은 정말 놀랍지만 함수를 동기적으로 호출하고 싶을 때가 있습니다. 그럴때 call back을 사용하는데, call back을 남발하면 코드의 가독성이 급격하게 떨어집니다. 이를 call back hell 이라고 합니다. 최근 ECMAscript는 call back hell을 해결하기 위해 Promise와 async await 문법을 추가했습니다.

## 클로저

전통적으로 클로저는 순수 함수형 프로그래밍 언어가 지닌 특징중 하나였다. 자바스크립트는 정보 은닉을 클로저를 이용해서 구현합니다.
