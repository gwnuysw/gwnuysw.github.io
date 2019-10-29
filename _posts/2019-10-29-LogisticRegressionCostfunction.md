---
layout: post
title:  "Logistic regression의 const function"
date:   2019-10-29 14:30:00 +0900
categories: jekyll update
comments : true
---

 기존의 const function을 그대로 사용하면, Local Minum 이란 것 때문에 꼭지점을 찾기가 어렵다. 그래서 cost function을 다시 설계 해야 한다.

![시그모이드의 한계]()

그래서 로그를 취한다.

![로그씌운그래프]()
![로그 수식]()

그래서 이 그래프에 Gradient descent 알고리즘을 적용하면 된다.
