---
layout: post
title:  "기본적인 머신용어 정리와 개념"
date:   2019-10-28 14:00:00 +0900
categories: jekyll update
comments : true
---

**explicit program의 한계**

제어문(if, while )으로 프로그램이 어떻게 동작할지 기술하는 것이 너무 많은 경우가 있다.

- Spam filter : 규칙이 많다
- 자율 주행 : 규칙이 너무나 많다

**Machin Learning**

그럼 프로그래머가 일일이 프로그래밍 하지말고 프로그램이 어떤 데이터를 가지고 스스로 학습하도록 하자!

> "Field of study that gives computer the ability to learn without being explicitly programed" Arthur Samuel (1959)

**머신러닝의 종류**

- Supervised Learning
  - learning with labeled examples - training set

예를 들면

![고양이 사진](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%207.27.42.png?raw=true)

각 사진들에 '고양이' 라고 하는 이름표를 붙이는 것이다.

![강아지 사진](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%207.30.31.png?raw=true)

강아지 사진도 마찬가지

- Unsupervised Learning
  - learning labeled data

예를 들면 구글 뉴스 grouping, 비슷한 단어 grouping

**Supervised Learning의 종류**

- 공부한 시간에 따라 시험성적이 어떻게 나올 것인가?
  - regression
- 공부한 시간에 따라 Pass할 것이가? NonPass할 것인가?
  - binary classification 두개중에 하나 고르는것
- 공부한 시간에 따라 A,B,C,D,E,F 중 뭘 받을 것인가?
  - multi-labeled classification 여러 레이블중에 뭘 받을지 분류
