---
layout: post
title:  "uipath 프로젝트 구성하기"
date:   2020-05-18 11:27:00 +0900
categories: jekyll update
comments : true
---

모든 UiPath 구현은 명확한 원칙을 따라야합니다.

- 신뢰성(로봇이 예상대로 실행 되야 한다.에러 최소화)
- 효율성(실행 시간이 최소화 되야한다.)
- 유지보수성(워크플로우가 이해하기 쉬워야 하고 디버깅 하기 쉬워야 한다.)
- 확장성(새로운 기능을 추가하기 쉬워야 한다.)

## 프로젝트 레이아웃 선택하기

**Sequence**
  - 언제 사용 할까?
    - if문 사용 없이 논리적인 절차가 명확 할때(Ui automation)
    - 보통 flow chart와 state machine 내부 워크플로우로 사용합니다.
  - 장점
    - 순서대로 따라가면서 이해하기 쉽다. top to bottom 설계에 사용
    - 브라우저에서 버튼찾는 것과 같은 간단한 로직에 적합
  - 단점
    - 너무 많은 조건문을 사용하면 워크플로우를 이해하기 어렵다.
    - 반복적인 흐름울 설계할 때는 맞지 않다.

**FlowChart**
  - 언제 사용 할까?
    - 조건문을 여러개 사용하는 복잡한 워크플로우를 만들때 적어도 시각적으로 이해하기 쉬운 레이아웃입니다.
    - 워크플로우가 반복적이고 몇가지 조건에 의해서 종료할때
  - 장점
    - 이해하기 쉽다. 소프트웨어 개발할때 사용하는 선서도와 똑같다.
    - 반복적 워크플로우에 적용할 수 있다.
  - 단점
    - FlowChart는 반드시 내부에 Sequence를 이용하여 범용 워크플로우로 사용해야 합니다. FlowChart단독으로 프로젝트를 구성 하면 안됍니다.

**State Machine**

유한 상태 기계는 유한한 개수의 상태를 가질 수 있는 오토마타, 즉 추상 기계라고 할 수 있다. 이러한 기계는 한 번에 오로지 하나의 상태만을 가지게 되며, 현재 상태(Current State)란 임의의 주어진 시간의 상태를 칭한다. 이러한 기계는 어떠한 사건(Event)에 의해 한 상태에서 다른 상태로 변화할 수 있으며, 이를 전이(Transition)이라 한다. 특정한 유한 오토마톤은 현재 상태로부터 가능한 전이 상태와, 이러한 전이를 유발하는 조건들의 집합으로서 정의된다.

  - 장점
    - 더욱 복잡한 반복적인 워크플로우를 만들때 좋다.
    - 상태간 전이를 쉽게 정의할 수 있고, 유연성을 제공한다.
    - 한두개 이상의 조건문과 반복문을 사용하는 워크플로우 만들때 사용.
    - 프로그램의 거의 모든 분기/전환을 쉽게 처리할 수 있습니다.
  - 단점
    - 프로세스를 논리적 "상태"로 분할하고 전환 등을 파악하는 등의 복잡성으로 인해 개발 시간이 길어진다.

## 복잡한 프로젝트 잘게 부수기

https://docs.uipath.com/activities/docs/invoke-workflow-file

https://docs.microsoft.com/ko-kr/dotnet/framework/windows-workflow-foundation/using-workflowinvoker-and-workflowapplication

## 라이브러리 만들고 사용하기

https://docs.uipath.com/studio/docs/about-libraries

## 예외 처리

- Activity 수준에서 Try/Catch 블록, 또는 Retry Scope블록으로 해결
- Global Level에서 Global exception handler로 해결

자세한건 Error & Exception Handling 과정에서 배웁니다.

**참고**

Uipath Academy -> RPA Developer Foundation -> Project Organization

https://academy.uipath.com/learningpath-viewer/1986/1/151138/2

[위키백과 유한 상태 기계](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%95%9C_%EC%83%81%ED%83%9C_%EA%B8%B0%EA%B3%84)
