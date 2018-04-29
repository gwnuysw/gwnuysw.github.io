---
layout: post
title:  "컴퓨터 구조 The proccessor"
date:   2018-04-21 10:44:00 +0900
categories: jekyll update
---

---
### 4.1 Introduction

핵심적인 MIPS 명령어는 다음과 같이 3종류가 있다.
* 메모리 참조 명령어인 워드 적재(lw) 워드 저장(sw)
* 산술/논리 명령어인 add, sub, AND, OR, slt
* 같을 시 분기 명령어인 beq와 점프 명령어 j


위 명령어들은 모두 아래의 처음 두 단계는 동일하다.

1. Send PC and fetch
2. Read one or two registers


---
### Data path
![simpledatapath](https://i.imgur.com/nM8IuyM.png)

_Source:[StackOverflow](https://stackoverflow.com/questions/33334521/extending-mips-datapath-to-implement-sll-and-srl)_

#### Fetching instruction
![fetching](https://www.cise.ufl.edu/~mssz/CompOrg/Figure4.5-MIPSdatapath1.gif)

_Source:[University of Florida](https://www.cise.ufl.edu/~mssz/CompOrg/CDA-proc.html)_

#### R formed instruction
instruction : op(6)rs(5)rt(5)rd(5)shamt(5)function(6)
![Rformed](https://www.cise.ufl.edu/~mssz/CompOrg/Figure4.7-MIPSdatapathRfmt.gif)

_Source:[University of Florida](https://www.cise.ufl.edu/~mssz/CompOrg/CDA-proc.html)_

#### Load and store instruction

instruction : op(6)rs(5)rt(5)addr(16)
![lwsw](https://www.cise.ufl.edu/~mssz/CompOrg/Figure4.8-MIPSdatapathLodStr.gif)

_Source:[University of Florida](https://www.cise.ufl.edu/~mssz/CompOrg/CDA-proc.html)_

#### Jump, Branch

![jump](https://www.cise.ufl.edu/~mssz/CompOrg/Figure4.9-MIPSdatapathBranch.gif)


_Source:[University of Florida](https://www.cise.ufl.edu/~mssz/CompOrg/CDA-proc.html)_

---
### 4.5 An overview of Pipelining

- multiple instructions are overlapped in excution

MIPS instruction steps

1. Fetch
2. Decode and read registers
3. Excute the operation
4. Access an operand in data memory
5. Write the result into a register

![Pipelining](https://cs.nyu.edu/courses/fall10/V22.0436-001/Figure_4.27.jpg)

_Source:[NewYorkUniversity](https://cs.nyu.edu/courses/fall10/V22.0436-001/lecture15.html)_

![Pipelining2](https://cs.nyu.edu/courses/fall10/V22.0436-001/Figure_4.35.jpg)

_Source:[NewYorkUniversity](https://cs.nyu.edu/courses/fall10/V22.0436-001/lecture15.html)_

```
(time Pipelining) : (thime pipelined) / (#pipeline stages)
```
