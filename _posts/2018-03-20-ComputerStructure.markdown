---
layout: post
title:  "컴퓨터 구조 명령어"
date:   2018-03-20 23:16:00 +0900
categories: jekyll update
comments: true
---

### Instructions Language of the Computer

* ARM(Advanced Risc Machine)

  * ARMv7: 32-bit Instructions set - GalaxyS, iphone
  * ARMv8: 64-bit Instructions set - MIPS와 유사하다.


* MIPS(Microprocessor without Interlocked Pipeline Stages)

  * sony play station

---
### MIPS operand of the Computer hardware

* operand

  * register

    32개의 32 비트 레지스터가 있다.

    `$S0~$S7 : saved rigister`

    `$t0~$t9 : temporary register`

    ```
    e.g
    f = (g+h)-(i+j);
    // f:$S0, g:$S1, h:$S2, i:$S3, j:$S4
    add $t0, $S1, $S2
    add $t1, $S3, $S4
    sub $S0, $t0, $t1
    ```
    > C언어 에서 레지스터 선언 `register int j;` 은 j의 주소를 알 수 없다.
    
  * constants

    ```
    $3에 상수 4를 더하는 코드
    lw $t0, addrConst4($S1)
    add $S3, $S3, $t0
    lw를 사용하지 않는 코드
    addi $S3, $S3, 4
    ```
  * memory

    2^30 memory words `memory[0], memory[4],memory[8]...memory[2^30*4-4]`

    ```
    eg1
    g=h+a[8]
    //g:$S1, h:$S2, A:$S3(base)
    lw $t0, 32($S3)
    add $S1, $S2, $t0
    eg2
    a[12]=h+a[8]
    //a:$S3, h:$S2
    lw $t0, 32($S3)
    add $t0, $S2, $t0
    sw $t0, 48($S3)
    ```
    > 배열의 이름은 배열의 시작 주소이며, base address라고도 한다.

#### Byte address Endian

* Little Endian
  배열의 앞쪽 정보가 메모리의 앞에서부터 채워지는 방식
* Big Endian
  배열의 뒤쪽 정보가 메모리의 앞에서 부터 채워지는 방식
