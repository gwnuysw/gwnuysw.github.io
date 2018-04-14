---
layout: post
title:  "컴퓨터 구조 성능"
date:   2018-03-17 22:28:00 +0900
categories: jekyll update
comments: true
---
### Performance

speed - excution time (response time)
total time required for the computer to complete a task

throughput (= bandwith) : the number of tasks completed per unit time

`성능 = 1 / 실행 시간`

퍼포먼스는 실행 시간과 반비례합니다.

A는 10초 걸리고 B는 15초 걸린다 A는 몇배 빠른가?

`성능A/성능B = 실행시간B/실행시간A = 15/10 = 1.5`

---
### 성능의 측정

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

---
### CPU Performance and its factors
```
(CPU time for a program)
= (CPU clock cycle for a program) * (CPU clock time)
= (CPU clock cycle for a program) / (CPU clock rate)
```

```
(clock rate) = 3GHz
(clock cycle for a program) = 6*10^9

(CPU time) = 6 * 10^9 / 3GHz = 2sec
```

computer A(2GHz) runs a program in 10secs computer B runs 1.2times as the program in 6 secouns what is the clock rate of computer B?

```
10 = (cycle for a program) / 2GHz     -----computer A
6 = 1.2 * (cycle for a program)  / ?Hz  ---computer B

? = 4GHz
```
---
### instruction Performance
CPU:Clock cycle Per Instruction
```
(CPU clock cycle)
= (The number of Instruction for a program)
* (Average clock cycles per instruction)
```

|computer|clock cycle time|CPI|
|:------:|:--------------:|---|
|A|250ps|2.0|
|B|500ps|1.2|

How much is faster than B?
```
(2.0 * 250ps) / (1.2 * 500ps) = 1.2(times)
```

compairs code segment in a computer

|code segment|A|B|C|
|---|---|---|---|
|code 1|2|1|2|
|code 2|4|1|1|
|CPI|1|2|3|

```
CPI(code1) = (2*1+1*2+2*3) / (2+1+2) = 2
CPI(code2) = (4*1+1*2+1*3) / (4+1+1) = 1.5
```

---
**1.5** [4]<1.6> 동일한 명령어 집합을 가지고 있는 3개의 다른 프로세서 P1, p2, p3의 클럭 속도와 CPI가 다음과 같다.

| 프로세서| 클럭 속도 | CPI  |
|:-------------:|:-------------:|:-----:|
| P1   | 3GHz | 1.5 |
| P2  | 2.5GHz    |  1.0 |
| P3 | 4GHz   |  2.2 |

**a** 어느 프로세서의 성능이 가장 좋은가? 초당 명령어 수로 표현 하라.

```
초당 명령어 수 : 클럭속도 / CPI
P1 : 3G / 1.5 = 2G
P2 : 2.5G / 1.0 = 2.5G
P3 : 4G / 2.2 = 1.8181...G

P2의 성능이 가장 좋다.
```
**b** 각 프로세서가 어느 프로그램을 수행하는데 10초가 걸렸다고 할때, 사이클 수와 명령어 개수를 구하라.
```
10초 동안의 사이클 수 : 클럭속도 * 10초
P1 : 3G * 10 = 30G
P2 : 2.5G * 10 = 25G
P3 : 4G * 10 = 40G

그동안의 명령어 개수 : 10초 동안 사이클 수 / CPI
P1 : 30G / 1.5 = 20G
P2 : 25G / 1.0 = 25G
P3 : 40G / 2.2 = 18.1818....G
```
**c** 실행 시간을 30% 단축 시키려고 했더니, CPI가 20% 증가하게 되었다. 이만큼의 시간 단축을 위해서는 클럭 속도를 얼마로 해야 하는가?
```
0.7 (CPU 시간) = ((프로그램 명령어 개수) * 1.2 (CPI)) / (X * (클럭속도))
X = 12/7
클럭속도를 12/7배 빨리 해야 한다.
```
**1.6** [20] <1.6> 동일한 명령어 집합 구조의 두 가지 구현이 있다고 하자. A, B, C, D 네 가지 종류의 명령어가 있으며, 각 구현의 클럭속도와 CPI는 아래의 표와 같다.

| | 클럭 속도 | A의 CPI | B의 CPI | C의 CPI | D의 CPI |
|:--|:---------:|:-------:|:------:|:-------:|:------:|
| P1  | 2.5GHz | 1 | 2 | 3 | 3 |
| P2  | 3GHz    | 2 | 2 | 2 | 2 |

 1.0E6개의 명령어를 실행하느 프로그램이 있다. 그중 10%가 유형 A, 20%가 B, 50%가 C, 20%가 D라고 하면, P1과 P2중 어느 것이 더 빠른가?

**a** 각 구현의 전체적인 CPI는 얼마인가?
```
P1 CPI : 1 * 0.1 + 2 * 0.2 + 3 * 0.5 + 3 * 0.2 = 2.6
P2 CPI : 2 * 0.1 + 2 * 0.2 + 2 * 0.5 + 2 * 0.2 = 2.0
```
**b** 각 경우 실행에 필요한 클럭 사이클 수는 얼마 인가?
```
P1 : 2.6 * 10e6 = 2.6e6
P2 : 2.0 * 10e6 = 2.0e6
```
