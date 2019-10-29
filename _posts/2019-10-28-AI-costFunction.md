---
layout: post
title:  "Linear Regression의 cost function"
date:   2019-10-28 21:00:00 +0900
categories: jekyll update
comments : true
---

### Gradient descent algorithm

![cost function제곱](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%208.49.28.png?raw=true)

사람눈으로 보기엔 딱봐서 꼭지점을 알 수 있지만 컴퓨터는 못한다.

컴퓨터는 임의의 지점 아무데나 시작해서 w,b를 조금씩 변경하며 꼭지점을 찾아야 한다.

 그때 **미분** 을 이용한다. 꼭지점 왼쪽에서 어딘가에서 시작한다면 미분값이 음수 이므로 오른쪽으로 찾아갈 것이고, 오른쪽에서 시작한다면 미분값이 양수 이므로 왼쪽으로 찾아갈 것이다 그래서 미분값이 0이 되는 점을 찾는것이 Gradient descent algorithm이다.

 ### convex function

 ![convex](https://d3ansictanv2wj.cloudfront.net/convex-non-convex-9c8cb9320d4b0392c5f67004e8832e85.jpg)

오른쪽 사진 같은 경우는 시작점이 잘 못 설정하면 최소점을 찾는데 실패할 수 있다.
그래서 선형회귀를 할때는 const function이 왼쪽 모양이 되도록 잘 설계 해야 하는데 왼쪽 모양을 convex function이라고 한다.


참고 : [sungkim유튜브](https://www.youtube.com/watch?v=TxIVr-nk1so&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm&index=6)
