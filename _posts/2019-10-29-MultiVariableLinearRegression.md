---
layout: post
title:  "Multi-variable Linear Regression"
date:   2019-10-29 13:00:00 +0900
categories: jekyll update
comments : true
---

### Hypothesis에 변수가 여러개면?

`H(x) = W1x1 + W2x2 + W3x3 ... + b`

그냥 쭉 나열하면 되는데 수천, 수만개가 되면 한줄로 표현하기 어렵다.

그래서 **행렬** 로 표현한다.

![행렬표현](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%201.32.56.png?raw=true)

행렬이란것을 알리기 위해 X W자리를 바꿔준다.

![다수의 인스턴스](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%201.47.56.png?raw=true)

이렇게 변수에 값을 많이 넣어야 할때는 그냥 X행렬에 행에 값을 다 넣어주면 된다.

![커진 매트릭스](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_29/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-29%20%EC%98%A4%ED%9B%84%201.48.58.png?raw=true)

우리가 찾으려고 하는 W는 그대로이기 때문에 상관없다.

참조 : [sungkim유튜브](https://www.youtube.com/watch?v=o2q4QNnoShY&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm&index=10)
