---
layout: post
title:  "프로그래밍 언어론 타입"
date:   2018-04-20 19:45:00 +0900
categories: jekyll update
---

---
### 타입

타입은 변수들의 집합과 이 변수들에 대한 연산들의 집합이다.

---
### 기본 데이터 타입

프로그래밍 언어의 타입들은 크게 **기본 타입** 과 **응용타입** 으로 구분된다.
* 기본 타입

  기초 타입(primitive type)이라고 하기도 한다 java를 예로 기본 타입은 불리안 타입과 수치 타입으로 구분된다.

* 응용 타입

  구조화 타입 혹은 기본타입에서 유도된 타입 이라고 하기도 한다. java에서는 응용 타입을 참조 타입이라고 하고, 여기에는 클래스 타입, 인터페이스 타입, 배열 타입이 있다.

---
### 정수

* Ada

  프로그래머가 각 변수를 위한 범위를 정할수 있고, 그 범위가 컴파일러의 허용 범위를 초과할 경우 컴파일 오류를 발생한다.

* swift

  정수 데이터 타입은 Int와 Uint다

* swift ruby

  숫자의 가독성을 높이기 위해 밑줄 문자를 사용해서 숫자를 원하는 단위로 구분할 수 있다. 다음은 swift의 예이다.

  `var price = 1_000_000_000`

---
### 문자

C99,C++,Ada95, Java, C#, Fortran 2003 등과 같은 다수의 현대 언어에서는 문자 타입에 해당하는 표현은 ASCII 코드를 사용하지 않고 2바이트 형태의 코드 체계를 가진 유니코드와 ISO/IEC 10646 국제 표준에 기반을 둔 멀티바이트 문자 집합을 사용한다.

---
### [부동 소수점 수](https://gwnuysw.github.io/jekyll/update/2018/04/09/ComputerStructure.html)

---
### 불리안 타입

* C99, C++

  불리안 타입을 제공하지만, 이들 언어는 숫자 식이 불리안 식으로 사용되는 것을 허용한다.

* Java, C#

  숫자 식이 불리안 식으로 사용되지 않는다.

* Pascal, Ada

  true와 false 리터럴을 이용하여 표현한다.

* Python

  수를 bool로 변환하기도 하는데, 이는 세 가지 불리안 연산자를 수에도 직접 적용할 수 있다는 의미이다.

  * 0과 0.0은 false로, 다른 모든 수는 true로 취급한다.

  * 빈 문자열도 false로 대응되고 다른 모든 문자열은 ture에 대응된다.

* Swift는 Objective-c와 달리 true, false외의 다른 값을 혀용하지 않기 때문에 Yes, 1, No, 0 등은 더 이상 불리안 값으로 평가되지 않는다.

---
### 열거 타입

일반적으로 열거 타입의 객체는 프로그래머에 의해 정의되는 리터럴 스트링이다. 기본적인 연산은 상등 연산과 배정 연산이며 그 밖의 연산은 순서를 가정한 관계연산 (>, <, <>, =, <=, >=), 선행(pred), 후속(succ)연산이다. 일반적으로 열거 타입은 주어진 차례대로 정수 0,1,...으로 표현된다. **관련된 정수값 대신 이름을 프로그램에서 사용할 수 있으므로 프로그램 자체의 가독성이 향상된다.**

* C

  전통적인 C언어 스타일의 열거 타입은 포함되어 있지 않은 정수를 배정하더라도 컴파일러가 오류를 발견하지 못한다는 문제점이 있다.

* Swift

  C언어 유형의 문제점을 개선하고 클래스나 구조체가 가지고 있던 다양한 기능을 차용하였다. `today = 100`과 같은 코드는 컴파일 되지 않는다. 열거 타입의 멤버를 나열할 때에는 case 예약어를 사용한다. 하나의 case에서 여러 멤버를 동시에 나열할 때에는 콤마로 구분한다.
  ```
  enum Days{
    case Monday
    case Tuesday
    case Wednesday
    case Thursday
    case Friday
    case Saturday, Sunday
  }
  ```
  Swift의 열거 타입을 사용할 때에는 반드시 점연산자를 사용하여야 한다.

  `var today = Days.Sunday`

  ```
  var today : Days = Days.Sunday
  today = .Monday
  var today = .Monday // 오류
  var today : Days = .Monday //정상
  ```
* C#

  int 타입 이외의 정수 타입을 기본으로 하여 열거 타입을 만들기 위해서는 다믕과 같이 콜론(:)을 이용하여 타입을 지정한다.
  ```
  enum name : byte {Lee, Kim, Park, Kwon = 100, Choi, Moon};
  ```

* Ada

  동일한 리터럴이 하나 이상의 열거 타입에서 사용되는 것이 Pascal, C, C++에서는 불가능 하지만 Ada에서는 가능하다.

* Ada, C#, Java5.0

  세 언어 에서는 열거 타입은 다른 언어들에 비해 다음의 장점을 가진다.

  * 열거 타입에 대한 어떠한 산술 연산도 허용되지 않는다.
  * 열거 타입 변수에는 정의된 범위 밖에 속한 값을 배정받을 수 없다.
  * 열거 타입 변수가 정수 타입으로 강제 변환되지 않는다.

---
### 부분 범위 타입

미리 정의된 열거 타입 또는 정수 부분 집합을 값으로 하는 타입이다. 명시적은 부분 범위 타입은 몇 가지 장점을 가진다.
1. 프로그램 문서화를 지원한다.
2. 컴파일러가 부분범위 타입을 분석하기 때문에 유효하지 않은 값이 부분범위 타입의 변수에 대입되지 않는 것을 보장하는 동적인 의미검사 코드를 생성할 수 있다.
3. 컴파일러는 부분 범위 타입의 변수가 가질 수 있는 값의 개수를 알기 때문에 임의의 정수를 표현하기 위해 필요한 비트보다 더 적은 수의 비트를 사용할 수 있다.


* pascal

  ```
  type speed = 0..200;
  x: speed; i:integer;
  ```
  베이스 타입은 정수이다. 부분범위 타입인 speed의 값은 i와 같은 정수 변와 함께 섞을 수 있지만 다음과 같은 실행 시간 요류를 야기한다.
  ```
  X := 150 + i;
  ```

* Ada

  ```
  TYPE INDEX IS RANGE 0..50;
  TYPE CENTURY IS DELTA 0.1 RANGE 0.0..100.0;
  TYPE WORK_DAY IS ARRAY (DAYS-OF_WEEK RANGE MON..FRI) OF BOOLEAN;
  ```
---
### 배열

배열은 이름, 차원, 요소의 타입, 색인의 타입과 범위 등으로 특징지을 수 있다.

* Ada, FORTRAN, PL/I

  이름이 A이고, 크기가 4X5인 2차원 배열을 표시하기 위해서 A(4,5)라고 기술한다. 함수 표기와 같아서 혼동이 생길 수 있다.

* ALGOL 60, ALGOL 68, PASCAL

  A[4,5]

* C

  A[4][5]

* Pascal

  ```
  type size10 = array[1..10] of integer;
  type size20 = array[1..20] of integer;
  ```
  두 배열은 서로 다른 타입의 배열이 되서 하나의 매개변수로 상이한 길이의 배열을 받아들이는 서브프로그램은 작성되지 않는다. 또한 색인은 임의의 이산타입, 정수 타입, 문자 타입, 부분 범위 타입, 또는 열거 타입이 될수 있다.
  ```
  var temperature: array[month] of real;
  rainfall: array[months] of real;
  ```
  상수를 이용해서 배열의 크기를 정할 수 있다.
  ```
  const numberofdays = 30;
  var day: array[1..numberofdays] of real;
  ```
* FORTRAN

  100개의 요소를 가지는 배열 선언문
  ```
  DIMENSION A(100)
  ```

* Ada

  배열의 크기가 변하는 것을 허용하지만, 실제적으로는 상이한 크기의 새로운 배열을 배열 포인터에 배정한다.

* c, c++

  배열에 대한 경계 검사를 하지 않기 때문에, 배열의 본래 기억 장소 영역을 벗어나 다른 변수의 데이터나 프로그램 코드를 침범하는 경우가 발생할 수 있다.

* Ruby

  배열의 타입이 없고 가변적이다. 동적으로 배열의 크기가 변한다.
  ```
  a = [0,1,4,9,16]
  a[-1]   //16
  a[-2]   //9
  a[a.size-1]   //16
  a[-a.size]    //0
  a[8]        //nil
  a[-8]       //nil
  a[0] = "zero"   //["zero",1,4,9,16]
  a[-1] = 1..16    //"zero",1,4,9,1..16]
  a[8] = 64     //"zero",1,4,9,1..16,nil,nil,nil,64]
  a[-10] = 100  // 오류
  ```

* Python

  ```
  >>>fruits = [5,4,7,3,2,3,2,6,4,2,1,7,1,3]
  >>>fruits[-1]
  3
  >>>fruits[-2]
  1
  >>>fruits[-14]
  5
  ```

---
### 배열 초기화

* Ada

  ```
  type MAT is array(INTEGER range 1..2, INTEGER range 1..2) of REAL;
  A: MAT := ((10,20),(30,40));
  B: MAT := (1=>(1=>1, others=>0), 2=>(2=>1, others => 0));
  ```
* Swift

  ```
  var emptyArray: Array<String> = []
  var coupleNames: [String] = ["Youngtae", "Hyeja", "Bbangtae", "Hyeja"]
  ```

---
### 배열 연산

* c, FORTRAN, PASCAL

  배열 연산자를 제공하지 않음

* java, c++,c#

  메소드를 통한 배열 연산 제공

* Ada

  전체적인 배열의 배정과 &을 이용한 연결 허용

* Python

  배열 연결(+)과 멤버십(in) 연산을 제공하고, 두 개의 비교 연산자(is, ==)를 제공한다.

* Swift

  ```
  let coupleNames = ["Youngtae", "Hyeja", "Bbangtae", "Hyeja"]
  let residence = ["Wonju", "Seoul"]

  let combinedCouple = coupleNames + residence
  ```

---
### 배열 조각화

배열의 조각은 부분 요소이다. 단지 배열의 부분 요소를 참조하는 방법이다.

* PL/I

  ```
  declare w(1:3, 1:5), a(1:5), b(1:5)
  a = w(3, *);
  b = w(*, 5);
  ```

* Ada

  3x3 배열 A의 조각은 다음과 같은 방법들에 의해 배정될 수 있다.
  ```
  A: array(0..2, 0..2) of REAL := ((0.0, 0.0, 0.0)
                                    0.0, 0.0, 0.0)
                                    0.0, 0.0, 0.0));
  A: array(0..2, 0..2) of REAL := (0..2 => (0.0, 0.0, 0.0))
  A: array(0..2, 0..2) of Real := (0..2 => (0..2 => 0.0))
  ```
* Python

  배열 조각의 첫번째 색인은 시작 지점이고 두번째 색인은 포함하려는 항목의 색인보다 1이 큰 값이다.
  ```
  >>>love_makers = ['Youngtae', 'Hyeja', 'Bbangtae', 'Hyeja', 'YT']
  >>>trouble_makers = love_makers[0:4]
  >>>love_makers[:4]    //처음부터 4이전까지
  >>>love_makers[4:]    //4부터 끝까지
  ```

* Swift의 배열

	  ```
	  var phd = ["Boonhee", "Joonkil", "Hyeja", "Youngtae"]

	  for row in phd{
	    print("배열 원소는 \(row)입니다.")
	  }
	  ```

	swift에서 동적으로 배열을 정의하는 방법
	```
	//처음 발표 되었을 때 문법
	var phd = Array<String>() //선언과 초기화

	var phd : Array<String>	//선언후

	phd = Array()	//초기화

	//개선을 거친 후 문법

	var phd = [String]()	//선언과 초기화

	var phd : [String]	//선언

	phd = [String]()	//초기화 첫번째 방식

	phd = []	//두번째 방식

	```

---
### 색인의 바인딩과 배열 유형

* 정적 배열

	색인 범위가 정적으로 바인딩 되고, 기억 장소 할당이 실행 시간 전에 정적으로 이루어진다.
* 고정 스택 동적 배열

	색인 범위가 정적으로 바인딩 되지만, 기억 장소 할당이 선언문이 실행될 때에 이루어 지는 배열이다.
* 스택 동적 배열

	색인 범위와 기억 장소 할당이 모두 선언문이 실행될 때에 이루어지는 배열이다.
* 고정 힙 동적 배열

	색인 범위와 기억장소 바인딩이 모두 기억 장소가 할당된 이후에 고정된다는 점에서 스택 동적 배열과 유사하지만, 색인 범위와 기억 장소 바인딩이 실행 중에 프로그램이 요구할 때 이루어지고, 스택이 아닌 힙으로부터 할당된다는 차이점을 가진다. Java의 모든 배열은 고정 힙 동적 배열이다. C#도 이러한 배열을 가진다.

* 힙 동적 배열

	색인 범위의 바인딩과 기억 장소 할당이 동적으로 이루어지고, 바인딩이 배열의 수명 동안 여러 차례 변경이 가능한 배열을 말한다.

---
### 문자열

* c

	c는 문자열을 널 문자로 끝나는 일련의 문자들로 표현한다. C에서는 문자열을 위한 배정문과 문자열을 위한 기억 장소를 자동적으로 할당하는 문자열 함수는 존재하지 않는다.

* java


	문자열이 널문자로 끝나지 않는다. 별도로 문자열을 다루는 클래스인 String 클래스와 Stringbuffer클래스가 존재한다. 
* Ada
	
	문자열 타입에 관한 부분 문자열의 참조 연산자, 연결 연산자, 관계 연산자, 배정문이 제공된다. Name(2:5)는 Name의 두번째, 세번째, 네 번째, 다섯 번째 문자를 참조 한다.

* Fortran77

	문자열을 도입하였다 
	```
	CHARACTER L*15, M*20 //각각 길이 15, 20의 문자열
	CHARACTER(LEN=9) N	//N은 fortran90에서 길이가 9인 문자열로 사용
	```
* Python

	```
	>>>'12'+str(34)+'56'
	'123456'
	>>>'XO'*5
	'XOXOXOXOXO'
	>>>5*'-'
	'-----'
	>>>'XO'*0
	''
	>>>'XOXOXOXOXO'*-5
	''
	```
* Ruby

	```
	x = %q{this is a first string
	second string
	third string}
	//{}, <>, ()으로 대치 하거나 !!등 임의의 한쌍의 기호를 사용해도 된다.
	x=<< END_STRING
	this is a first string
	second string
	third string!
	END_STRING
	//perl과 shell 스크립트 언어에서 종종 사용되는 here document기능을 쓰는 방법이 있다.
	```
