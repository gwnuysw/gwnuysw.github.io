---
layout: post
title:  "프로그래밍 언어론 프로그래밍 언어의 분류"
date:   2018-03-23 22:13:00 +0900
categories: jekyll update
---

### 프로그래밍 언어의 분류

#### 명령형 언어

변수와 배정문과 같이 기억장소를 참조하는 명령으로 기억장소의 내용을 조작하여 계산 과정을 수행하는 언어로 프로그램의 상태를 어떻게 변화 시키는가를 지시하기 위한 명령을 주제로 한 언어라고 할 수 있다.

>거의 대부분의 컴퓨터 하드웨어는 명령형으로 구현된다. 거의 모든 컴퓨터 하드웨어들이 컴퓨터의 고유 언어인 기계어를 실행하도록 설계되어 있는데, 이것이 명령형으로 씌어 있다. _-출처:[위키백과]_

#### 함수 언어

프로그램에 의한 계산 과정은 컴퓨터 구조와 독립적으로 생각할 수 있다. **명령형 언어와 달리 함수 언어에서는 기억 장소의 상태에 대한 개념이 존재 하지 않는다.**
>수학의 함수는 정의상 입력이 같으면 출력도 같다. 그러니까 f(x)=x+1 인 함수를 정의했다면 f(1)=2다. 다른 값은 절대 나오지 않는다. 함수형 언어의 함수도 마찬가지다.

```
int inc(int a) { static int c=0; c=c+a; return c; }
```
>이 함수에 1을 넣어 여러 번 호출하면 1, 2, 3, 4, 5, ...가 나온다. 순수 함수형 언어는 이게 안된다는 얘기다.
>_출처:[나무위키]_

#### 논리 언어

논리 언어는 모델의 계산을 위해 기호 논리와 집합론을 이용한다. 컴파일-링크-실행의 사이클을 갖지 않는 대화형 언어에 해당한다.

#### 객체 지향 언어

>프로그램을 단순히 데이터와 처리 방법으로 나누는 것이 아니라, 프로그램을 수많은 '객체'라는 기본 단위로 나누고 이 객체들의 상호작용으로 서술하는 방식이다.
>_출처:[나무위키]_
