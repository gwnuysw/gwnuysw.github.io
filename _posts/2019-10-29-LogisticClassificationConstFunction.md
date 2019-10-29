---
layout: post
title:  "Logistic Classification의 cost function"
date:   2019-10-29 14:00:00 +0900
categories: jekyll update
comments : true
---

 분류 알고리즘 중에서 **가장 강려크한 알고리즘** 입니다.

활용 예
 - 스팸/햄 필터링
 - facebook feedback: show or hide
 - 신용카드 도난 당했을때 카드 사용패턴이 다르다면 분실 했다고 판단한다.
 - 주식을 팔까? 말까?
 - 종양이 암일까? 아닐까?

 ![선형회귀한계]()

 기존의 선형회귀로는 한계가 있다.

그래서 수학자들이 x값에 어떤 값을 넣어도 값이 0~1사이가 나오는 함수를 찾았다.

그것이 바로 **Sigmoid 함수** 다.

![시그모이드]()

그래서 우리의 가설은 이런 형태다

![가설]()
참조 : [sungkim유튜브](https://www.youtube.com/watch?v=PIjno6paszY&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm&index=11)
