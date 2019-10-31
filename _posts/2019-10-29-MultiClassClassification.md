---
layout: post
title:  "MultiClass Classification"
date:   2019-10-29 15:00:00 +0900
categories: jekyll update
comments : true
---

공부시간에 따라 성적이 A,B,C,D,E,F로 나오는 것과 같은 분류를 MultiClass Classification이라고 한다.  

MultiClass Classification을 하는 두가지 방법이 있다.

![바이너리와 비교](http://www.holehouse.org/mlclass/06_Logistic_Regression_files/Image%20[23].png)

## One vs All

첫 번째는 각각의 클래스에 대하여 이진 분류를 하는 것이다. 그래서 클래스의 갯수 만큼 이진분류를 하면된다.

![oneVsAll](http://www.holehouse.org/mlclass/06_Logistic_Regression_files/Image%20[24].png)

## Softmax regression

각 클래스가 될 확률을 구하는 방법이다.

추측인데 one vs all 을 사용하면 코딩량이 많아져서 조금 복잡한 개념이지만 간단한 코드를 사용하는 softmax를 이용하는것 같다...(정확히는 모름)
근데 softmax regression이 one vs all의 연장선에 있는것 처럼 말하는 것도 있는데 아니다. 내가 보기엔 서로 별개의 방법론인것 같다. [이진 분류도  softmax regression](https://dataaspirant.com/2017/03/02/how-logistic-regression-model-works/)을 이용할 수 있다.
이진 분류는 여러번 해야하지만 softmax regression은 각 클래스가 나올 확률을 한번에 구해준다.

참조

[외국강의](http://www.holehouse.org/mlclass/06_Logistic_Regression.html)

[스탠포드자료](http://ufldl.stanford.edu/tutorial/supervised/SoftmaxRegression/)
