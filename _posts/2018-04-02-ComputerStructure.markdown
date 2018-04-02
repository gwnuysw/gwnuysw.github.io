---
layout: post
title:  "컴퓨터구조 subroutine procedure"
date:   2018-04-02 21:50:00 +0900
categories: jekyll update
---
# 2.8 subroutine procedure

## Allocating space : stack (LIFO)
![heap](https://courses.engr.illinois.edu/cs232/fa2011/lectures/lecture6/memory.jpg)

* 최하위 주소 부분은 유보되어 있다.(reserved)
* 그 다음은 MIPS기계어 코드가 들어가는 부분이다. 이를 text segment라 부른다.
* 그 위는 상수와 정적 변수들이 들어가는 static data segment다. 배열은 크기가 고정되어 있어서 정적 데이터 세그먼트에 잘 맞는다.
* 링크 리스트는 크기가 유동적이다. 이러한 자료구조를 위한 세그먼트를 그 위에 heap이라 한다.

프로시저의 저장된 레지스터와 지역 변수를 가지고 있는 스택 영역을 activation record라 부른다.

## Execution of a procedure

1. put parameters($a0~$a3)
2. transfer control(caller -> callee)
3. acquire the storage on the stage
4. perform callee
5. put the result value
6. return control (callee -> caller)

```
int proc(int g, int h, int i, int j)
{
  int f;
  f = (g+h)-(i+j);
  return f;
}
->jr $ra # jmp back to the caller
addi $sp, $sp, -12
sw $t1, 8($sp)
sw $t0, 4($sp)
sw $t0, 0($sp)
add $t0, $a0, $a1 # $t0 = g+h
add $t1, $a2, $a3 # $t1 = i + j
sub $s0, $t0, $t1 # f = (g+h) - (i+j)
add $v0, $s0, $0
lw $s0, 0($sp)
lw $t0, 4($sp)
lw $t1, 8($sp)
addi $sp, $sp, 12
```

## parallelism : synchronisation

mutualexcusion - 상호배제 : lock & unlock(semaphore)신호등

## Translating and starting a program

1. C program
2. Compiler
3. machine language(object)
4. linker
5. executalble program
6. loader
7. memory

dll(dynamic linked library) 리눅스는 .so라고 한다.
the library rountines are not linked and loaded until the program is run

* 장점
  version update, small module
