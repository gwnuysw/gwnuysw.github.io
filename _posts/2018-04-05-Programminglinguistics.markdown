---
layout: post
title:  "프로그래밍 언어론 변수와 상수"
date:   2018-04-04 22:08:00 +0900
categories: jekyll update
---
### 블록 구조
```
{
  int a, x; //a, x 할당
  {
    int x;  
    char y;   //x, y할당
  }//x,y 해제
}//a,x 해제
```
```
void func(void)
{
  int a;
  float b;
}
```
실행 중에 이 함수선언을 만나면 블록func는 실행되지 않고 지역 변수 a와 b도 선언 시점에서 할당 되지 않는다. func가 호출 되었을 때에만 할당된다. func호출을 활성화(activation)이라 하고, 이에 대흥하는 할당된 기억 장소의 부분을 **활성 레코드** 라 한다.

---

### c와 동적할당

```
int *p; // 포인터 선언
p = (int *) malloc(sizeof(int)); //할당
*p = 10;  //역참조
free(p);  //객체 해제
```

malloc 함수는 **익명** 의 포인터를 리턴하기 때문에 배정될 변수의 데이터 타입에 결과를 일치 시키기 위해 캐스트 연산자를 적용한다. p가 가리키느 객체에 값을 배정할 때는 p를 **역참조** 하기 위해 내용 연산자를 이용한다.

---

### c++와 동적 할당

동적 할당과 해제가 new, delete연산자를 통해 이루어 진다.

---

### 정리
포인터를 가진 블록 구조 언어에서는 전역 변수를 위한 정적 할당, 지역 변수를 위한 자동 할당, 그리고 포인터 변수를 위한 동적 할당으로 구분할 수 있다. 이러한 구분을 변수의 **기억 장소 클래스** 라고 한다.

---
### 변수와 환경

문장의 **참조 환경** 은 문장 에서 가시적인 객체 이름의 모임이다. 정적 영역 규칙을 가지는 언어에서 참조 환경은 지역 영역내에서 선언된 모든 변수와 가시적으로 포함되는 영역 내에서 선언된 모든 변수를 포함한다.

---
### 변수 초기화

```
void test_static();
int main()
{
	test_static();
	test_static();
	test_static();
	test_static();
	exit(0);
}
void test_static()
{
	static int scnt = 0;
	auto int dcnt = 0;
	scnt += 1;
	dcnt += 1;

	printf("scnt = %d, dnt = %d\n",scnt, dcnt);

}
->
scnt = 1, dnt = 1
scnt = 2, dnt = 1
scnt = 3, dnt = 1
scnt = 4, dnt = 1
```

c에서 정적 변수는 오직 한번만 초기화 되고 동적 변수는 매번 호출될 때 마다 초기화 된다.

---
### 형식 추론 기능(type inference)

Swift에서는 타입 추론 기능을 이용하여 변수의 데이터 타입을 스스로 추론 한다.

```
var name = "Youngtae"
var year = 2009
```
### 상수와 변수

이름 상수는 배정문의 좌측에 올 수 없다. 상수값은 프로그램의 실행 중에 변경될 수 없다. 일반적으로 코딩 관례상 상수 이름은 일반 변수와 구별하기 위해서 대문자를 사용하고 단어들은 밑줄을 사용하여 구분한다.

```
const int MAX_ITEMS = 2000;
```

Pascal의 상수 표현
```
const size = 100; // :=대신 =이 사용되었다.
```
Pascal 컴파일러는 상수를 상수 선언에서 직접적으로 주어진 값의 기호 이름으로 보고 프로그램에서 나오는 모든 기호 이름을 그값으로 즉시 대치할 수 있따. 따라서 Pascal의 상수를 종종 명백한 상수(manifast constant)라고 하기도 한다.

```
if(CONST < 10){}
```
ada, c++, java에서는 상수에 값을 동적 바인딩 하는 것을 허용한다. 따라서 위의 내용은 기계어 코드로 번역 되지 않는다.
