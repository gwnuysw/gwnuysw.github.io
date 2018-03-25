---
layout: post
title:  "운영체제 인터럽트"
date:   2018-03-25 20:08:00 +0900
categories: jekyll update
---
# 인터럽트
CPU에 전달되는 사건 신호(Event Signal)로서 주로 전용 회선으로 전달된다. 인터럽트를 인지한 CPU는 현재 처리 중인 일을 중지하고, 그인터럽트가 어떤 사건인지를 식별하여 처리한 후, 다시 원래이 처리하던 일을 계속 한다.

## 인터럽트 서비스 루틴
어떤 사건인지를 식별하여 처리한다는 것은 인터럽트 선으로 전달된 인터럽트 번호에 대응되는 처리루틴을 찾아 실행시켜줌을 의미하는데, 대응 처리 루틴을 보통 **인터럽트 서비스 루틴** 혹은 **인터럽트 핸들러** 라고 부른다.

## Vectored Interrupt, Non-Vectored Interrupt

>A **vectored interrupt** is where the CPU actually knows the address of the Interrupt Service Routine in advance. So the vectored interrupt allows the CPU to be able to know what ISR to carry out in software (memory).
>
>A **non-vectored interrupt** is where the interrupting device never sends an interrupt vector. An interrupt is received by the CPU, and it jumps the program counter to a fixed address in hardware. The CPU crucially does not know which device caused the interrupt without polling each I/O interface in a loop and checking the status register of each I/O interface to find the one with status “interrupt created”. __출처: [<a href="https://www.quora.com/What-is-the-difference-between-a-vectored-and-a-non-vectored-interrupt" target="_blank">Quora</a>]__


## 인터럽트 우선순위(Interrupt Priority)
인터럽트들은 상대적 우선순위를 가지고 있어서 높은 우선순위의 인터럽트 서비스가 먼저 처리된다.

## 인터럽트 사이클(Interrupt Cycle)
일반적으로 인터럽트 사이클은 기계 사이클의 맨 마지막 단계로 추가된다.

## 인터럽트 유형

* 디바이스 인터럽트(Device Interrupt)

  디바이스 인터럽트는 입출력 장치 등과 같이 CPU 외부 하드웨어로부터 발생하기 때문에 하드웨어 인터랍타 부르기도 한다. 대부분의 인터럽트가 여기에 해당한다.

* 오류 인터럽트(Error Interrupt)

  실행 중인 프로그램에 해당 CPU의 기계 명령어 포맷에 어긋나는 명령어가 포함되어 있을경우 CPU는 더 이상 진행할 수 없으므로 스스로 인터럽트를 발생 시켜 이에대한 오류 메시지를 출력하고, 다른 프로그램의 실행을 계속 하도록 한다. 오류 상황은 예상 못한 예외 사건이 발생한 경우이므로 예외라고 부르기도한다.

* 소프트웨어 인터럽트(Software Interrupt)

  인터럽트를 기계 명령어로도 발생시킬 수 있는 CPU들이 있다.(인터럽트를 테스트할때 사용한다.) 기계 명령어에 의해 프로그램에서 인위적으로 발생되는 인터럽트를 소프트웨어 인터럽트라 한다.트랩(Trap)이라 부르기도 한다.
