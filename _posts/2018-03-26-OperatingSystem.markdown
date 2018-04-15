---
layout: post
title:  "운영체제 입출력 관리"
date:   2018-03-26 14:23:00 +0900
categories: jekyll update
---

# 입출력 개관

모든 입출력은 하드웨어와의 인터페이스를 통해서 이루어지고, 하드웨어는 운영체제에 의해서 통제되므로, 응용 프로그램들의 입출력은 반드시 운영체제를 통해야만 가능하다.

---
### 직접 입출력과 간접 입출력

* 직접 입출력

  응용프로그램과 하드웨어 장치 사이에 입출력되는 데이터가 가공 없이 전달된다. 예를 들어 하드 디스크의 경우 응용 프로그램이 "ABCD"라는 데이터를 직접 데이터 입출력 통로로 출력하면, 해당 디스크 첫 섹터의 앞 네 바이트에 "ABCD"가 기록된다.

* 간접 입출력

  가공된 데이터가 전달된다. 파일 시스템으로 출력하면 해당파일의 inode에 새인된 첫 데이터 섹터의 앞 네 바이트에  그 내용이 기록된다.

---
### 문자 입출력 장치와 블록 입출력 장치

키보드를 연결하는 UART장치나 LAN어댑터 등은 입출력이 바이트 단위로 이루어지므로 **문자 입출력 장치** 라 부른다. 입출력 단위가 일정한 크기로 고정된 경우를 **블록 입출력 장치** 라 한다.

---
### 파일개방과 파일 기술자(파일 디스크립터)

파일 입출력은 운영체제가 지원하는 서비스에 따라 "개방"->"입출력"->"폐쇄"의 절차를 따라야 한다. 이 때, 응용 프로그램의 지속적인 입출력을 서비스하기 위해 운영체제가 내부적으로 관리하는 파일의 상세 정보를 파일 기술자(File Descripter)라 한다. 유닉스/리눅스 파일 시스템에서의 1차 파일 디스크립터는 inode라고 말할 수 있다.

---
### 파일 입출력 절차

* 입력

```
# include <stdio.h>
# include <fcntl.h>

struct student{
  char name[10];
  int age;
}Std;

main()
{
  int fd, i;

  fd = open("/data/test.dat", O_WRONLY | O_CREAT, 0755);

  if(fd < 0){
    perror("open()");
    return (-1);
  }
  for (i = 0;i<3;i++){
    scanf("%s %d", Std.name, &Std.age);
    write(fd, &Std, sizeof(Std));
  }
  close(fd);
}
```
* 출력

```
# include <stdio.h>
# include <fcntl.h>

struct student{
  char name[10];
  int age;
}Std;

main()
{
  int fd, i;

  fd = open("/data/test.dat", O_RDONLY);

  if(fd < 0){
    perror("open()");
    return (-1);
  }
  for (i = 0;i<3;i++){
    read("%s %d", Std.name, &Std.age);
    printf(fd, &Std, sizeof(Std));
  }
  close(fd);
}
```
위 에서 open(), read(), write(), close()는 유닉스/리눅스 운영체제가 제공하는 **파일 입출력 서비스(시스템 콜)** 이다. 이들 시스템 서비스는 함수와 동일한 방식으로 호출되지만 _함수 내부에서 소프트웨어 인터럽트를 발생 시켜 운영체제 내부의 인터럽트 서비스 루틴으로 진입한다._

해당 인터럽트 핸들러에서는 응용프로그램이 넘긴 매개 변수 값들을 운영체제 내부 메모리로 복사한 후, 이들을 사용하여 운영체제 내에 구현된 open(), write(), close()등의 프로시저로 점프한다. 이와 같이 사용자 응용 프로그램 영역의 데이터를 운영체제 내부 영역으로 복사하는 일을 copy-in 이라 부른다.

---
### 파일 입출력 서비스(유닉스/리눅스)

![inode](http://www.science.unitn.it/~fiorella/guidelinux/tlk/img84.gif)

_출처[<a href="http://www.science.unitn.it/~fiorella/guidelinux/tlk/node96.html">www.science.unitn.it/</a>]_

---
### 대기 입출력(Blocking IO)과 비대기 입출력(Non-blocking IO)

입출력을 요청한 응용프로그램은 그 입출력이 진행되는 동안 대기상태로 전환되 기다려야 한다.

* 입력

  `n = read(fd, buf, size);`를 예로

  1. 최대 size 바이트 1바이트라도 입력이 이루어질 때까지 대기 시켰다가 데이터 입력이 이루어지면 그 만큼만 리턴해주고 실행을 재개 하돍 한다.

  2. size바이트가 충족될 때까지 대기시켰다가. 데이터 입력이 충족되면 데이터를 리턴해주고 실행을 재개하도록 한다.

  3. 요청 시점 현재 입력된 데이터가 전혀 없으면 데이터 없음 표시와 함께 그대로 리턴하여 프롯세스가 계속 진행하면서 대처하도록 한다. **(비대기 입력)**

대부분의 운영체제는 기본으로 1.의 형태로 처리하고, 응용프로그램의 선택에 따라 3.의 동작을 취할 수 있도록 설계된다. 비대기 입력은 여러 장치로부터의 입력 데이터를 최대한 빨리 얻고자 할 때 활용된다.

* 출력

  `n = write(fd, buf, size)`

  1. 최대 size 바이트이내에서 1바이트라도 출력이 이루어질 때까지 대기 시켰다가, 데이터 출력이 이루어지면 그 크기를 알려주고 실행을 재개하도록 한다.
  2. size바이트가 출력될 때까지 대기시켰다가. 그 결과를 알려주고 실행을 재개하도록 한다.
  3. 요청 시점 현재 출력이 불가능하면 출력 불가 표시와 함께 그대로 리턴하여 프로세스가 계속 진행하면서 대처하도록 한다.

출력도 대부분의 운영체제는 1.형대의 대기 출력을 기본을 한다. 만약 3.형태의 비대기 출력을 원한다면 fcntl(fd, ...)이나 ioctl(fd,...)등의 시스템 서비스를 통해서 운영체제에게 설정 변경을 요청해야 한다. 다만, 데이터 저장이 컴퓨터 내부에 이루어지는 경우에는 비대기 출력이 불가능 하다.

---
### 입출력 장치 구동기(디바이스 드라이버)
드바이스 드라이버란 입출력 장치의 인터페이스에 직접 접근하여 입출력을 진행하는 운영체제 코드의 한 부분이다. 입출력 장치의 인터페이스는 장치 제어기 회로와 연결되어 있는 **상태 레지스터(Status Register), 명령 레지스터(Command Register), 데이터 레지스터(Data Register)** 를 의미한다.

주기억 장치와 입출력 장치 인터페이스에 접근하여 입출력을 진행하는 장치 구동기의 기본적인 동작 절차는 다음과 같다.

* 동기 입력(디스크 등과 같이 데이터가 컴퓨터 내부에 저장되어 있는 경우)
  * 상태 레지스터를 읽어서 'Idle'상태 인지 확인
  * 명령 레지스터에 '읽기' 명령을 표시한 후, 상태 레지스터가 '입력 중(Busy)'으로 전환 되는지 확인
  * 상태 레지스터에 입력이 완료되었음을 의미하는 '데이터 대기(Ready)'가 표시될 때까지 대기
  * 데이터 레지스터 값을 읽어서 주기억 장치에 저장
  * 원하는 크기만큼의 데이터를 읽을 때까지 위의 과정을 반복

* 비동기 입력(키보드, LAN과 같이 데이터 입력을 예측할 수 없는 경우)
  * 상태 레지스터를 읽어서 'Idle'이거나 '입력중(Busy)'상태이면 '데이터 대기(Ready)'가 표시될 때까지 대기
  * 데이터 레지스터 값을 읽어서 주기억장치에 저장
  * 원하는 만큼의 데이터를 읽을 때까지 위의 과정을 반복

* 출력(출력은 항상 동기적)
  * 상태 레지스터를 읽어서 Idle이 아니고 Busy이면 Idle이 표시 될 때까지 대기
  * 주기억장치에서 데이터를 읽어서 데이터 레지스터에 저장
  * 명령 레지스터에 '쓰기'명령을 표시한 후, 상태 레지스터가 '출력중(Busy)'으로 전환 되는지 확인
  * 원하는 만큼의 데이터를 출력할 때까지 위의 과정을 반복

---
### 프로그램 입출력(바쁜 대기 입출력 Busy waiting I/O)

* 입력

```
# define STR (*(char *)0x80)//상태 레지스터
# define DTR (*(char *)0x82)//데이터 레지스터
# define CDR (*(char *)0x84)//명령 레지스터

/*busy waiting synchronous INPUT*/
read_char_bw(char *bp, int count) //bp는 버퍼주소 count는 읽을 개수
{
  int i;

  for(i = 0; i < count; i++, bp++)
  {
    while(STR != IDLE)  //첫 번째로 IDLE상태 인지 확인한다.
    ;
    CDR = 1; //두 번째로 input Command
    while(STR != READY) //세번 째로 데이터가 데이터 레지스터에 채워지고
    ;       //데이터 레지스터가 채워 질때 까지 계속 상태를 확인한다. 이떄문에 바쁜 대기 입출력 이라고 한다.
    *bp = DTR;//네 번째로 데이터 레지스터에 있는 정보를 가져간다.
  }
  return (i);
}
```
* 출력

```
# define STR (*(char *)0x80)
# define DTR (*(char *)0x82)
# define CDR (*(char *)0x84)

/*busy waiting synchronous OUTPUT*/
write_char_bw(char *bp, int count)
{
  int i;

  for(i = 0; i < count; i++, bp++)
  {
    while(STR != IDLE)
    ;
    DTR = *bp, bp++;
    CDR = 2;  //output Command
  }
  return (i);
}
```

---
### 인터럽트 기반 입출력(Interrupt-driven I/O)
```
# define STR (*(char *)0x80)
# define DTR (*(char *)0x82)
# define CDR (*(char *)0x84)

static char *Bp;
static int Nb Cb;
/*Interrupt driven INPUT*/
read_char_id(char *bp, int count)
{
  Bp = bp, Nb = count, Cb = 0;
  while(STR != IDLE)// 상태를 계속 확인 한다.
  ;
  CDR = 1;//입력 명령을 내린다.
  return(1);//그리고 함수를 반환해버린다!! 이부분이 busy waiting과 중요한 차이점 이다.
}//busy waiting 은 데이터레지스터가 채워질때까지 상태레지스터를 계속 확인 한다.
ISR(CHAR_RX_vect)//반대로 데이터 레지스터가 채워지면 인터럽트를 걸어서 데이터가 채워진것을 알려준다.
{
  *Bp = DTR, Bp++, Cb++;
  if(Cb < Nb)
  {
    CDR = 1;//input Command
  }
  else
  {
    ...//schedule the process
  }
}
```
