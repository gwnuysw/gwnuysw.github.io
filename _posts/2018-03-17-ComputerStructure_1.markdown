---
layout: post
title:  "컴퓨터 구조 1"
date:   2018-03-17 21:44:00 +0900
categories: jekyll update
---

### computer abstractions & technology

컴퓨터의 기능은 세가지로 나눌수 있습니다. 저 수준 언어에서 고 수준 언어 순으로 나열하면 다음과 같습니다.

1. 하드웨어
2. 운영체제 OS
3. 응용 소프트 웨어

#### 저 수준 언어
하드웨어를 통제하기위해 고안된 언어는 2진수로 이루어진 기계어가 있습니다. 기계어 명령어를 instruction 이라고 합니다. 그러나 이러한 2진수 표현은 사고의 한계가 있기 때문에 사람들은 어셈블리어를 만들었습니다. 단순한 기호로된 명령어 입니다. 어셈블러는 어셈블리어를 번역하여 기계어로 변환 합니다.

#### 고 수준 언어
어셈블리어 마저 사고의 한계가 있어서 사람들은 고 수준 언어를 만들었습니다. 고 수준 언어는 컴파일러가 어셈블리어로 번역 합니다. 고 수준 언어는 몇 가지 중요한 장점이 있습니다.

1. 자연어와 유사합니다.
2. 프로그래머의 생산성을 높여 줍니다.
3. 프로그램을 개발한 기종과 상관없이 어느 컴퓨터에서든 실행이 가능합니다.

### Performance

speed - excution time (response time)
total time required for the computer to complete a task

> throughput (= bandwith) : the number of tasks completed per unit time

```
성능 = 1 / 실행 시간
```
퍼포먼스는 실행 시간과 반비례합니다.
#### 성능의 측정

1. cpu time
```
cpu time = user time + system time
```
2. wall-clock time
벽시계 시간, 응답시간, 경과시간 이라고 부르며 이것은 한 작업을 끝내는데 필요한 전체시간을 뜻하는 것으로 디스크 접근, 메모리 접근, 입출력 작업, 운영체제 오버헤드 등 모든 시간을 다 더한 것이다. cpu time 보다 정확 하다
```
start = timestamp();
end = timestamp();
wall-clock time = end - start;
```
