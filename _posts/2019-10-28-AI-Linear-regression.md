---
layout: post
title:  "선형회귀"
date:   2019-10-28 20:00:00 +0900
categories: jekyll update
comments : true
---

### 선형회귀란

![데이터사진](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%208.16.36.png?raw=true)

(출처 : [웹페이지](https://minsgodrema.ga/5387))

Hypothesis `H(x) = Wx + b`

데이터를 가장 잘 설명하는 선형 상관관계를 찾는 것

선형회귀에서 선형이란 변수 x가 아니라 변수 x를 이용해 찾고자 하는 기울기나 절편 같은 것이 선형이란 것을 의미 합니다. 아래식은 모두 선형 회귀식입니다.

![비선형 처럼 보이는것](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%209.55.19.png?raw=true)

![선형으로 보이게 바꾼것](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%209.55.24.png?raw=true)


## 어떤 가설, 함수가 좋은지 어떻게 찾는가?

가설과 데이터의 오차가 가장 적은것을 고르면 된다.

![가설탐색](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%208.32.34.png?raw=true)

이때 사용하는 함수를 **cost(Loss) function** 이라고 한다.

**const function은 가설과 데이터 차이의 평균을 구하면 되는데** cost function을 단순히 `H(x) - y`를 사용하면 `H(1) - 1 = 0.5, H(3) - 3 = -1`인데 이를 더하면 0.5로 오차가 작아져 버린다.

이런 손실을 없애기 위해 가설과 데이터의 차를 제곱해서 음수를 없앤다.

`(H(x) - y)^2`

![cost function제곱](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%208.49.28.png?raw=true)
절대값을 써도 되지 않을까?

`|H(x) - y|`

![cost function절대](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/2019_10_28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-10-28%20%EC%98%A4%ED%9B%84%208.48.33.png?raw=true)
그래도 되는데 제곱했을 떄의 이점이 몇가지 있다.

1. '제곱'이기 떄문에 오차가 클수록 패널티가 크다
2. 미분했을 때 값이 다양해서 꼭지점을 찾기 쉽다.

참고
- [sungkim유튜브](https://www.youtube.com/watch?v=Hax03rCn3UI&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm&index=4)
- https://brunch.co.kr/@gimmesilver/18
