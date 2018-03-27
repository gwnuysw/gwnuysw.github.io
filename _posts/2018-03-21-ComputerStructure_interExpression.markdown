---
layout: post
title:  "컴퓨터 구조 명령어의 컴퓨터 내부 표현"
date:   2018-03-21 22:59:00 +0900
categories: jekyll update
comments: true
---

### 명령어의 컴퓨터 내부 표현
register - number Mapping rule

|$S0|$S1|$S2|$S3|$S4|$S5|$S6|$S7|
|:---:|:----:|:----:|:---:|:---:|:---:|:----:|
|16|17|18|19|20|21|22|23|

|$t0|$t1|$t2|$t3|$t4|$t5|$t6|$t7|
|:---:|:----:|:----:|:---:|:---:|:---:|:----:|
|8|9|10|11|12|13|14|15|

MIPS 명령어 인코딩


|instruction|format|op|rs|rt|rd|shamt|funct|address|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|add|R|0|reg|reg|reg|0|32|NA|
|sub|R|0|reg|reg|reg|0|34|NA|
|addi|I|8|reg|reg|NA|NA|NA|constant|
|lw(load word)|I|35|reg|reg|NA|NA|NA|address|
|sw(store word)|I|43|reg|reg|NA|NA|NA|address|
|sll(shift left logic)|R|0|0|reg|reg|amount|0|NA|
|srl(shift right logic)|R|0|0|reg|reg|amount|2|NA|
|And|R|0|reg|reg|reg|0|36|NA|
|or|R|0|reg|reg|reg|0|37|NA|
|nor|R|0|reg|reg|reg|0|39|NA|

#### register type

|op|rs|rt|rd|shamt|funct|
|:---:|:---:|:---:|:---:|:---:|:---:|
|6bits|5bits|5bits|5bits|5bits|6bits|
|연산자|source|source|목적지|shift|function|

#### Immedate type

|op|rs|rt|constant or address|
|:---:|:---:|:---:|:---:|
|6bits|5bits|5bits|16bits|
|연산자|source|목적지|상수or주소|

연습 문제

1. `add $t1, $S2, $S3` -> 0(6), 18(5), 19(5), 9(5), 0(5), 32(6)
2. `sub $S1, $S2, $S3` -> 0(6), 18(5), 19(5), 17(5), 0(5), 34(6)
3. `lw $S1, 8($S3)` ->  35(6), 19(5), 17(5), 8(16)


1. 첫번째 문제

  * `addi $t1, $t2, 4`

    `8(6), 10(5), 9(5), 4(16)`

  * `sw $t1, 16($t2)`

    `43(6), 10(5), 9(5), 16(16)`

2. 두번째 문제

```
A[10] = f + g - B[5]

A = $S0, f = $t0, g = $t1, B = $S1

add $t0, $t1, $t0
lw $t1, 20($S1)
sub $t0, $t1, $t0
sw $t0, 40($S0)
//convert
0(6), 8(5), 9(5), 8(5), 0(5), 32(6)
35(6), 17(5), 9(5), 20(16)
0(6), 8(5), 9(5), 8(5), 0(5), 34(6)
43(6), 16(5), 8(5), 40(16)
```

```
B[8] = A[i - j];
B = $S4, A = $S3, i = $S0, j = $ S1

sub $S0, $S0, $S1
sll $S0, $S0, 2
add $S0, $S3, $S0
sw $S0, 32($S4)

0(6)16(5)17(5)16(5)0(5)34(6)
0(6)0(5)16(5)16(5)2(5)0(6)
0(6)19(5)16(5)16(5)0(6)32(6)
43(6)20(5)16(5)32(16)
```
