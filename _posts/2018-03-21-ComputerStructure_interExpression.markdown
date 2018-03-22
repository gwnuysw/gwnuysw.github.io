---
layout: post
title:  "컴퓨터 구조 명령어의 컴퓨터 내부 표현"
date:   2018-03-21 22:59:00 +0900
categories: jekyll update
---

### 명령어의 컴퓨터 내부 표현
register - number Mapping rule

|$S0|$S1|$S2|$S3|$S4|$S5|$S6|$S7|
|:---:|:----:|:----:|:---:|:---:|:---:|:----:|
|16|17|18|19|20|21|22|23|

|$t0|$t1|$t2|$t3|$t4|$t5|$t6|$t7|
|:---:|:----:|:----:|:---:|:---:|:---:|:----:|
|8|9|10|11|12|13|14|15|

operation table

|add|sub|addi|lw|sw|
|:---:|:----:|:----:|:---:|:---:|
|0|0|8|35|43|

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
