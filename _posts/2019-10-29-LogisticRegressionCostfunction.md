---
layout: post
title:  "Logistic regression의 cost function"
date:   2019-10-29 14:30:00 +0900
categories: jekyll update
comments : true
---

 기존의 const function을 그대로 사용하면, Local Minimum 이란 것 때문에 꼭지점을 찾기가 어렵다. 그래서 cost function을 다시 설계 해야 한다.

![시그모이드의 한계](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%202.34.06.png?raw=true)

그래서 로그를 취한다.

![로그씌운그래프](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%202.58.40.png?raw=true)

![로그 수식](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%203.03.59.png?raw=true)

그래서 이 그래프에 Gradient descent 알고리즘을 적용하면 된다.

참조 : [sungkim유튜브](https://www.youtube.com/watch?v=6vzchGYEJBc&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm&index=12)
