---
layout: post
title:  "프로그래밍 언어론 문법"
date:   2018-03-24 14:22:00 +0900
categories: jekyll update
comments: true
---

### 예약어
식별자로 사용될 수 없는, 특별한 의미를 가지는 단어, 키워드는 식별자로 사용될 수 있다.

#### 장점

* 프로그램의 가독성을 향상 시킨다.
* 심벌 테이블을 짧은 시간에 탐색할 수 있으므로 파싱 능력이 향상 된다.
* 오류 복구를 더 잘할 수 있다.

#### 단점

* 예약어 수가 늘어나면 많은 예약어들을 기억하기 어려우므로, 변수 이름으로 예약어를 사용하는 오류를 범할 수 있음
* 기존의 언어를 확장하면, 확장 이전에 사용했던 식별자와 새로운 예약어가 중복가능

### 문법 구조의 표현

### 용어

* 파스 트리

  ![parseTree](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/parseTree.png)

* 터미널 노드, 리프, 단말 기호

  'The', 'MAN', 'EATS', 'THE', 'APPLE'

* 그 외 노드는 비단말 기호

* 문법

  각 비단말 기호가 어떠한 서브 노드를 어떤 순서로 가지는가를 규정하는 규칙의
  집합
  ![grammar](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/grammar.png)

* 생성 규칙

  앞의 문법으로 그릴 수 있는 파스 트리, 다음 문장들과 일치하는 네 가지 파스트리가 존재

  ![Strings](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/fourString.png)

   문장을 생성하기 위해 문법을 이용하기 때문에, 문법 규칙을 생성 혹은 **생성 규칙** 이라고 한다.

* 파싱

  생성과 반대로 주어진 문장이 문법적으로 옳은 문장인가를 분석하는 것이다.

### BNF
Backus-Naur form 베커스 나우르 표기법은 **문맥 자유 문법(Context-free grammar)** 을 나타내기 위해 만들어진 표기법이다.

>배커스-나우르 표기법(Backus–Naur form), 약칭 BNF는 문맥 무관 문법을 나타내기 위해 만들어진 표기법이다. 존 배커스와 페테르 나우르의 이름을 따서 부른다.
>
>BNF는 기본적으로 다음의 문법을 사용한다.
>
> `<기호> ::= <표현식>`
>
>여기에서 기호는 말단 기호가 될 수 없고, 표현식은 다른 기호의 조합, 또는 여러 가지의 표현식 중 하나를 사용한다는 의미로 \|를 사용한다. 다른 표현식으로 정의되지 않은 기호는 자동적으로 말단 기호가 된다. 또한, 기호가 아닌 상수에는 따옴표를 붙여서 구별한다. _출처: 위키백과_

### 문맥 자유 문법

문맥을 고려하지 않고 파싱에서의 치환이 이루어지는 문법을 문맥 자유 문법이라고 한다.

### 문맥 자유 문법의 구성

* 단말 기호의 집합
* 비단말 기호의 집합
* 생성 규칙의 집합
* 시작 비단말 기호로 선정된 비단말 기호

### 모호한 문법

생성 규칙에 따라 두 개의 파스 트리를 생성하는 문법

* 생성 규칙

  ```
  <expr> := <expr> + <expr> | <expr> * <expr> | A | B | C
  ```

* 두개의 파스 트리가 존재

  **모호한 문법을 증명할 때는 반드시 파스 트리를 이용한다.**
  ![ambiguous](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/amniguousGrammar.png)

* 모호한 문법의 해결

  연산자 우선순위와 연산자 결합의 개념으로 해결, 연산자들을 우선순위별로 나누어서 같은 우선순위를 가진 연산자들끼리 동일 레벨에서 처리, 새로운 비단말 기호 추가

  ![fixambig](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/fixAmbig.png)

### if-else dangle ambiguous

`if expr1 then if expr2 then stmt1 else stmt2`

만일 문법이 다음과 같이 주어진다면 위 문장은 두개의 파스 트리를 갖는다.
```
<if-stmt> ::= if<cond_expr> then <stmt> |
              if<cond_expr> then <stmt> else <stmt>
<stmt> ::= <if-stmt>
```
![ifambiguous](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/ifambiguous.png)

* 문법 규칙의 수정
  * then 다음의 <stmt>와 else 다음의 <stmt>에 대한 구분이 없기 때문
  * else를 가장 최근의 if, 즉 가장 내부의 if와 대응되는 파싱만을 허용
  * <stmt>를 <matched>와 <unmatched>로 구분하고 then 다음에는 <matched>만이 나타나도록 정의

![fixif](https://raw.githubusercontent.com/gwnuysw/gwnuysw.github.io/master/_images/fixif.png)


_모든 이미지는 동아 인쇄 출판사 '프로그래밍 언어론'에서 가져왔습니다._
