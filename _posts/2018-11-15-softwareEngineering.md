---
layout: post
title:  "소프트웨어공학 분석 설계 직접 해보기"
date:   2018-11-15 15:13:00 +0900
categories: jekyll update
comments : true
---

# 20141640 이석원
staruml에서 mdj확장자 파일 불러오기가 잘 안돼서 보고서도 같이 드립니다.
## 과제 1

- ATM에서 출금하는 scenario 3가지를 sequence diagram으로 표현하시오. 고객, ATM, 은행 시스템 사이의 interaction을 가능한 자세히 표현하여야 하며 ATM과 은행시스템 사이에 주고 받는 정보는 실제 상황과 일치할 필요는 없으나 합리적으로 표현되어야 한다.
- 정상적인 출금 scenario 1가지와 예외 scenario 2가지를 작성하되 각각 다른 sequence diagram으로(모두 3개의 diagram) 작성한다.

**정상출금 시나리오**

![정상출금](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/%EC%A0%95%EC%83%81%EC%B6%9C%EA%B8%88.png?raw=true)

**비밀번호 잘못 입력했을때 시나리오**

![비밀번호 오입력](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/%EA%B3%84%EC%A2%8C%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8%EB%B3%80%EA%B2%BD.png?raw=true)

**잔액보다 큰 금액 출금할때 시나리오**

![잔액보다 큰 금액 출금](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/%EC%9E%94%EC%95%A1%EB%B3%B4%EB%8B%A4%ED%81%B0%EA%B8%88%EC%95%A1%EC%B6%9C%EA%B8%88.png?raw=true)

## 과제 2

본교 학사관리 시스템의 use-case를 나열하시오. 단, 학사관리 시스템의 actor는 학생, 교수, 시 스템관리자(admin)만 고려한다.
참고: 각 use-case의 scenario(ATM 과제에서 sequence diagram으로 작성했던 내용)는 필요없 으며 해당 actor별로 use-case 이름만 나열하면 된다.

![학사관리 use-case](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/%ED%95%99%EC%82%AC%EA%B4%80%EB%A6%AC.png?raw=true)

## 과제 3

On/Clear, Off, +, =, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 버튼만 있는 계산기의 상태를 나타내는 state machine diagram을 작성하시오. 필요한 state와 transition을 나타내고 transition을 일으키 는 event와 transition 중에 수행하는 operation을 표시하여야 한다.
참고: 계산기를 사용하는 전형적인 시나리오는 다음과 같다. []안은 표시창 내용이다. 숫자를 입 력하는 도중의 표시창 내용은 나타내지 않았다.

1 Off 상태로 시작됨 []

2 On/Clear [0]

3-1. 1 0 1 [101]

3-2. 1 0 1 [101] + 2 0 [20] = [121]

3-3. 1 0 1 [101] + 2 0 [20] + [121] + 3 3 [33] = [154]

3-4. 1 0 1 [101] + 2 0 [20] + [121] + 3 3 [33] + [154]

3-5. 1 0 1 [101] + 2 0 [20] = [121] + 3 3 [33] = [154]

3-6. 1 0 1 [101] + 2 0 [20] + [121] + 3 3 [33] = [154] 7 0 [70] + 7 [7] = [77]

4-1. On/Clear를 눌러 2로 돌아감

4-2. Off를 눌러 1로 돌아감

자세한 내용은 첨부된 계산기 프로그램을 실행시켜서 확인하시오. (계산기 프로그램의 Off 상태 표시는 편의상 8888888888로 표시하였음)

![calculator](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/%EA%B3%84%EC%82%B0%EA%B8%B0.png?raw=true)
## 과제 4

1. MP3 player 구현을 위해 필요한 class를 나타낸 class diagram을 그리시오. Class는 적절한 attribute와 operation을 포함해야 한다.(attribute와 operation의 자세한 type은 명시할 필요 없 음)
2. MP3 player가 외부 입력 event(전원버튼, player 버튼, 일시정지 버튼, 다음곡 버튼, 이전곡 버튼, volumn up 버튼, volumn down 버튼, PC 연결 등)에 의해 어떻게 반응하는지 state machine diagram을 그리시오.

**MP3 class diagram**

![mp3 class diagram](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/mp3class.png?raw=true)

**MP3 satate diagram**

![mp3 state diagram](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/homeworks/mp3state.png?raw=true)
