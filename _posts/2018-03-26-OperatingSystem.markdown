---
layout: post
title:  "운영체제 입출력 관리"
date:   2018-03-26 14:23:00 +0900
categories: jekyll update
---
# 입출력 개관

모든 입출력은 하드웨어와의 인터페이스를 통해서 이루어지고, 하드웨어는 운영체제에 의해서 통제되므로, 응용 츠로그램들의 입출력은 반드시 운영체제를 통해야만 가능하다.

## 직접 입출력과 간접 입출력

* 직접 입출력

  응용프로그램과 하드웨어 장치 사이에 입출력되는 데이터가 가공 없이 전달된다. 예를 들어 하드 디스크의 경우 응용 프로그램이 "ABCD"라는 데이터를 직접 데이터 입출력 통로로 출력하면, 해당 디스크 첫 섹터의 앞 네 바이트에 "ABCD"가 기록된다.

* 간접 입출력

  가공된 데이터가 전달된다. 파일 시스템으로 출력하면 해당파일의 inode에 새인된 첫 데이터 섹터의 앞 네 바이트에  그 내용이 기록된다.

## 문자 입출력 장치와 블록 입출력 장치

키보드를 연결하는 UART장치나 LAN어댑터 등은 입출력이 바이트 단위로 이루어지므로 **문자 입출력 장치** 라 부른다. 입출력 단위가 일정한 크기로 고정된 경우를 **블록 입출력 장치** 라 한다.

# 파일개방과 파일 기술자(파일 디스크립터)

파일 입출력은 운영체제가 지원하는 서비스에 따라 "개방"->"입출력"->"폐쇄"의 절차를 따라야 한다. 이 때, 응용 프로그램의 지속적인 입출력을 서비스하기 위해 운영체제가 내부적으로 관리하는 파일의 상세 정보를 파일 기술자(File Descripter)라 한다. 유닉스/리눅스 파일 시스템에서의 1차 파일 디스크립터는 inode라고 말할 수 있다.

## 파일 입출력 절차

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

## 파일 입출력 서비스(유닉스/리눅스)

![inode](http://www.science.unitn.it/~fiorella/guidelinux/tlk/img84.gif)

_출처[<a href="http://www.science.unitn.it/~fiorella/guidelinux/tlk/node96.html">www.science.unitn.it/</a>]_

## 대기 입출력(Blocking IO)과 비대기 입출력(Non-blocking IO)

입출력을 요청한 응용프로그램은 그 입출력이 진행되는 동안 대기상태로 전환돼 기다려야 한다.

* 입력 대기

  `n = read(fd, buf, size);`를 예로

  1. 최대 size 바이트 1바이트라도 입력이 이루어질 때까지 대기 시켰다가 데이터 입력이 이루어지면 그 만큼만 리턴해주고 실행을 재개 하돍 한다.

  2. size바이트가 충족될 때까지 대기시켰다가. 데이터 입력이 충족되면 데이터를 리턴해주고 실행을 재개하도록 한다.

  3. 요청 시점 현재 입력된 데이터가 전혀 없으면 데이터 없음 표시와 함께 그대로 리턴하여 프롯세스가 계속 진행하면서 대처하도록 한다. **(비대기 입력)**

비대기 입력은 여러 장치로부터의 입력 데이터를 최대한 빨리 얻고자 할 때 활용된다.

* 출력 대기
