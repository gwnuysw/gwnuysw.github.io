---
layout: post
title:  "운영체제 스레드란?"
date:   2018-04-02 20:54:00 +0900
categories: jekyll update
---

# 스레드란?

## 다중 스레드 프로세스(Multi-thread Process)

다중 스레드 프로세스란 메모리에 적재된 프로그램의 시작 점이 여러 곳 있어서, 해당 프로세스를 바탕으로 진행하는 실행 줄기가 여러 개 존재한다는 의미이다. 이때 프로세스는 스레드를 담는 Resource Container역할을 한다.

```
int S;
void main1()
{
  int x, y, z;
  x = 1, y = 2;
  z = add(x, y);
  S = S + z;
}
void main2()
{
  int a, b, c;
  a = 3, b = 4;
  c = add(a, b);
  S = S + c;
}
int add(int p, int q)
{
  int s;
  s = p + q;
  return(s);
}
```
main1과 main2 스레드가 동시에 독립적으로 실행할 때 서로 다른 기계어 코드를 실행 하겠지만, add 함수 부분을 실행할 때에는 완전히 동일한 기계어 코드를 실행하게 된다. 그래도 스레드는 연산 과정이 고유하게 유지되는데, add함수와 그 안의 지역(자동)변수가 스택 메모리에 각각 저장되기 때문이다. 이때 기계 명령어는 SP(Stack Pointer) 상대주소를 이용한다. 전역 변수 S에는 원하고자 하는 값이 들어 가지 않는다.
