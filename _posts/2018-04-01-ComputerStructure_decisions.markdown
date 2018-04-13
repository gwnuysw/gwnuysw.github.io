---
layout: post
title:  "컴퓨터구조 Instructions for making decisions"
date:   2018-04-01 14:12:00 +0900
categories: jekyll update
---
# Instructions for making decisions

## - Conditional branch

### BEQ -- Branch on equal

|Syntax|beq $s, $t, offset|
|----:|----|
|Description|Branches if the two registers are equal|
|Operation|`if $s == $t advance_pc (offset << 2)); else advance_pc (4);`|

### BNE -- Branch on not equal

|Syntax:|bne $s, $t, offset |
|------:|-------------------|
|Description:|Branches if the two registers are not equal|
|Operation:|`if $s != $t advance_pc (offset << 2)); else advance_pc (4);`|

---
## - Unconditional branch

### J -- Jump

|Syntax:|j target |
|------:|---------|
|Description:|Jumps to the calculated address|
|Operation:|`PC = nPC; nPC = (PC & 0xf0000000)(or op)(target << 2);`|

_출처 : [MIPS Instruction Reference](http://www.mrc.uidaho.edu/mrc/people/jff/digital/MIPSir.html)_

```
(f, g, h, i  : $S0~$S4)

if(i == j) f = g + h;
else f = g - h;
-> beq를 사용한 방법
    beq $S3, $S4, L1
    sub $S0, $S1, $S2
    j L2
L1  add $S0, $S1, $S2
L2  ~~~
-> bne를 사용한 방법
      bne $S3, $S4, else
      add $S0, $S1, $S2
      j NEXT
else  sub $S0, $S1, $S2
NEXT  ~~~  

(i: $S3, k: $S5, save:$S6)

while(save[i] == k) i += 1;
->
Loop    sll $t0, $S3, 2
        add $t1, $S3, $S6
        lw $t2, 0($t1)
        bne $t2, $S5, EXIT
        addi $S3, $S3, 1
        j Loop
EXIT    ~~~
```

---
## Comparison of two numbers

### SLT -- Set on less than (signed)

|Syntax:|slt $d, $s, $t |
|------:|---------------|
|Description:|If $s is less than $t, $d is set to one. It gets zero otherwise.|
|Operation:|`if $s < $t $d = 1; advance_pc (4); else $d = 0; advance_pc (4);`|

### SLTIU -- Set on less than immediate unsigned

|Syntax:|sltiu $t, $s, imm |
|------:|------------------|
|Description:|If $s is less than the unsigned immediate, $t is set to one. It gets zero otherwise.|
|Operation:|`if $s < imm $t = 1; advance_pc (4); else $t = 0; advance_pc (4);`|

### SLTI -- Set on less than immediate (signed)

|Syntax:|slti $t, $s, imm |
|------:|----------------|
|Description:|If $s is less than immediate, $t is set to one. It gets zero otherwise.|
|Operation:|`if $s < imm $t = 1; advance_pc (4); else $t = 0; advance_pc (4);`|

```
(i = $S0. j = $S1. N : $S2)

i = 0;
while(i < N)
{
  j += 1;
  i++;
}
//-------------혹은
for(i = 0; i < N; i++) j += 1;
->beq를 이용한 방법
      li $S0, 0
Loop  slt $t0, $S0, $S2
      beq $t0, $0, EXIT
      addi $S1, $S1, 1
      addi $S0, $S0, 1
      j Loop
EXIT  ~~~
->bne를 이용한 방법
      li $S3, 0
Loop  slt $t0, $S3, $S2
      bne $t0, $0, GO
      j EXIT
GO    addi $S4, $S4, 1
      addi $S3, $S3, 1
      J Loop
EXIT  ~~~      
```

---
1. $t1이 0x00101000이라고 할때 다음을 실행한 후 $t2는무엇인가?  $t2 = 3
```
      slt $t2, $0, $t0
      bne $t2, $0, ELSE
      j DONE
ELSE  addi, $t2, $t2, 2
DONE
```

2. $t1은 10으로 초기화 되어있다. $S0가 처음에 0이었다면 실행 후 $S2의 값은 무엇인가? $S2 = 20
```
      move $t1, $S1
LOOP  slt $t2, $0, $t1
      beq $t2, $0, DONE
      subi $t1, $t1, 1
      addi $S2, $S2, 2
      j LOOP
DONE
```
3. 문제 2번 순환문에 해당하는 C코드 루틴을 작성하라. $s1, $s2, $t1, $t2는 각각 정수 A, B, i, temp이다.
```
i = A;
while(0 < i)
{
  i -= 1;
  B += 2;
}
```
