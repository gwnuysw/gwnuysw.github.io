---
layout: post
title:  "운영체제 인터럽트, 이중 모드 연산"
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

# 이중 모드 연산(Dual mode Operation)

사용자 모드와 시스템 모드가 식별되는 경우를 이중 모드 연산 이라 하고, 시스템 모드에서만 실행 가능한 기계 명령어를 특권 명령어라 한다.

# 입출력 장치

모든 입출력 장치는 상태(Status), 명령(Command), 데이터(Data)등 세개의 레지스터로 구성된 인터페이스 즉, 접근 창구를 제공 한다.

* 상태 레지스터

  데이터가 입력 되있는가 혹은 데이터의 출력이 완료 되었는가를 표시한다.

* 명령 레지스터

  '입력' 혹은 '출력'의 기본 명령과 기타 해당 장치 고유의 명령을 저장 함으로써 명령을 전달한다.

* 데이터 레지스터

  입력된 데이터나 출력될 데이터를 보관한다.

## 동기적 입력, 비동기적 입력

* 동기적

  하드 디스크 장치는 명령 레지스터에 '입력'이 주어졌을 때만 입력 작업을 진행한다. 출력은 언제나 동기적이다.
* 비동기적

  키보드 장치는 어느 때라도 키가 눌리면 해당 데이터를 입력 시킨다. 입력된 데이터가 컴퓨터 내에서 처리되기 전에 또 다른 데이터가 입력되면 데이터의 손실이 불가피하므로, 인터럽트에 의해 입력 사실을 CPU등 처리기에게 전달하여 신속하게 읽어가도록 해야한다.

## 입출력 장치 제어기, 입출력 장치 구동기

* 입출력 장치 제어기(Device Controller)

  상태, 명령, 데이터 레지스터등 인터페이스를 제공 하는 입출력 장치를 장치 제어기 라고 한다.

* 입출력 장치 구동기(Device Driver)

  입출력을 처리하는 소프트웨어를 장치 구동기라 부른다.

## 입출력 장치의 식별

  입출력은 입출력 장치 구동기로 장치 인터페이스인 상태, 명령, 데이터 레지스터에 대한 읽기 쓰기 작업으로 입출력을 실현한다. 이 때, 각 장치별로 고유 식별 위치 혹은 번호의 부여가 필요한데 이를 입출력 포트라 부른다. 메모리의 위치가 주소에 의해 식별되는 것처럼 입출력 포트에도 식별 메커니즘이 존재 한다.

### 메모리 대응 입출력
입출력 레지스터의 위치를 메모리 주소 영역의 일부에 대응시켜, 처리기가 메모리 접근 방법과 동일한 방법으로 입출력 레지스터에 접근 하도록 한다.

* 장점

  메모리 대응 입출력 방식은 메모리 접근이나 입출력을 위한 기계 명령어가 동일하기 때문에 개발자 입장에서 편리성이 있다.

* 단점

  메모리 주소 영역의 일부르 메모리 용으로 사용할 수 없다.

### 격리된 입출력

입출력 포트의 식별 영역이 메모리 주소 영역과 분리되어 있는 경우를 말한다.

* 장점

  메모리 공간을 충분히 사용할 수 있다.

* 단점

  메모리 접근 기계 명령어와 입출력 포트 접근 기계 명령어가 따로 있어야 하고 시스템 버스 메커니즘이 복잡해진다.

## 저장 장치 계층

저장 장치들을 접근 속도, 용량, 가격 관점에서 살펴보면 일련의 계층을 이룬다.

![storage hirerachy](http://cfile24.uf.tistory.com/image/0329B639510AFA3B2252F4)

cache는 메인 메모리의 1% 요량만 놔도 90 이상의 적중률을 보이는 것으로 알려져 있다.
