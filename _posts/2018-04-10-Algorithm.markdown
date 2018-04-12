---
layout: post
title:  "알고리즘 divide and conquer"
date:   2018-04-09 14:16:00 +0900
categories: jekyll update
---

---
### 되풀이

* 반복

  * for나 while과 같은 키워드를 사용하여, 반복 실행할 코드 부분을 명시적으로 나타냄
  * 일정 횟수 동안 혹은 어떤 조건이 만족될 때까지 반복하도록 지시

* 순환

  * 어떤 알고리즘이나 함수가 자기 자신을 호출하여 문제를 해결하는 기법
  * 본질적으로 순환적 특징을 갖는 문제나 순환적 데이터 구조를 다루는 프로그램에 적합
  * 수학의 귀납적 정의와 점화식과 밀접한 연관
  * 동일한 기능의 반복 구조 프로그램으로 변환 가능
  * 반복에 비해 수행속도 면에서는 손해

---
### 팩토리얼 Factorial


```
int iterative_factorial(int n)
{
  int i, factorial = 1;
  for(i = n; i > 0; i--) factorial *= i;
  return factorial;
}
```

반복 알고리즘의 시간 복잡도는 Θ(n)이다.

```
int recursive_factorial(int n)
{
  if(n <= 1) return 1;
  else return n*recursive_factorial(n-1)
}
```
순환 알고리즘의 시간 복잡도
```
T(n) = T(n-1) + C
     = T(n-2) + 2C
     = T(n-k) + kC
     = T(1) + (n-1)C
     = C + (n-1)C
     = Θ(n)
```

---
### 피보나치 수열

```
int iterative_fib(int n)
{
  int fib[MAX_SIZE], i;
  fib[0] = 0;
  fib[1] = 1;
  for(i = 2; i <= n; i++)fib[i] = fib[i+1] + fib[i-2];
  return fib[n];
}
```
반복 구조 피보나치 수열계산의 시간 복잡도는 Θ(n)
```
int recursive_fib(int n)
{
  if(n == 0) return 0;
  if(n == 1) return 1;
  else return recursive_fib(n-1) + recursive_fib(n-2);
}
```
순환 구조 피보나치 수열 계산의 시간 복잡도는
```
T(n) = T(n-1) + T(n-2) + C, T(n-1) > T(n-2)
     < 2T(n-1) + C = 2(T(n-2) + T(n-3)) + 3C, T(n-2) > T(n-3)
     < 4T(n-2) + 3C = 4(T(n-3) + T(n-4)) + 7C, T(n-3) > T(n-4)
     < 2^(n-2)*T(2) + (2^(n-2)-1)*C = 2^(n-2)(T(1)+T(0)) + (2^(n-2)-1)*C
     = O(2^(n-2))
```

```
T(n) = T(n-1) + T(n-2) + C, T(n-1) > T(n-2)
     > 2T(n-2) + C = 2(T(n-3) + T(n-4)) + 3C, T(n-2) > T(n-3)
     > 4T(n-4) + 3C = 4(T(n-5) + T(n-6)) + 7C, T(n-3) > T(n-4)
     > 2^((n-2)/2)*T(2) + (2^((n-2)/2)-1)*C = 2^((n-2)/2)(T(1)+T(0)) + (2^(n-2)/2-1)*C
     = Ω(2^(n-2)/2)
```

---
### 분할 정복의 기초

* 주어진 문제를 부문제들로 쪼개는 과정을 반복하면서 문제를 계속적으로 축소하는 방법
  * 부문제는 원래의 문제와 똑같은 성질을 가지면서 단지 크기만 작은 문제
    * 순환 호출 때마다 비용이 발생  
    * 부문제들은 완전히 독립적으로 해결하여야함
  * 축소 과정은 쪼개진 문제들을 쉽게 직접 풀 수 있을 때까지 진행

1. 작은 부문제로 나눈후(divide)
2. 각 부문제를 해결(conquer)
3. 부 문제의 해를 결합(combine)

```
divide_and_conquer(P)
{
  if(size(P) <= THRESHHOLD) return  solve_direct(P);
  else
  {
    divie(P, SUB1, SUB2, SUB3, ... ,SUBk);
    for(i = 1; i <= k; i++)
    Si = divide_and_conquer(SUBi);
    return combine(S1, S2, S3,... , Sk)
  }
}
```

---
### 마스터 정리

```
T(n) = aT(n/b) + O(n^c)

a = b^c경우 : T(n) = O(n^c*logn)
a > b^c경우 : T(n) = O(n^d), 여기서 d = logb(a)
a < b^c경우 : T(n) = O(n^c)
```

---
### 이진 탐색

* 복잡도 분석

  `T(n) = T(n/2) + O(1)` 이진 탐색의 부문제는 한 개이다. 마스터 정리 a = 1, b = 2, c = 0이므로 a = b^c 인 경우이며 복잡도는 O(logn)이다.

---
### 최대값 찾기

주어진 배열에서 최대값을 찾는 문제이다. 여기서 사용할 기본 아이디어는 이진탐색과 유사하다. 반으로 나눈 두 배열의 최대값은 다시 각각을 반으로 나눠 각각의 최대값 중 더 큰 값으로 하면 된다. 언제 까지 분할 할지는 설계자 몫이다.

```
int maximum_DC(int list[], int low, int high){
  int middle, lmax, hmax;
  if(low == high) return list[low]; //원소가 하나일 때
  else if(low == high-1){ //원소가 두 개일 때
    if(list[low] >= list[high]) return list[low];
    else return list[high];
  }

  else{   //탈출 조건이 아닌 경우 분할, 결합을 수행한다.
    middle = (low + high) / 2;  //분할
    lmax = maximum_DC(list, low, middle);//부할
    hmax = maximum_DC(list, middle+1, high);//분할
    if(lmax >= hmax) return lmax;
    else return hmax;
  }
}
```

시작하는 순서는 트리의 전위 순회를 따르고 종료하는 순서는 후위순회를 따른다.

* 복잡도 분석

  `T(n) = 2T(n/2) + O(1)` a = 2, b = 2, c = 0이므로 a > b^c 따라서 복잡도는 Θ(n)이다.

---
### 거듭제곱
순환 알고리즘

```
              |---      1               (n = 0)
power(x, n) = |
              |---    x*power(x, n-1)   (n > 1)
```

```
double iterative_power(double x, int n)
{
  int i;
  double power = 1.0;
  for(i = n; i > 0; i--) power *= x;
  return power;
}

double recursive_power(double x, int n)
{
  if(n == 0) return 1;
  else return x * recursive_power(x, n-1);
}
```
분할 정복 알고리즘

```
              |----     1                       (n = 0)
power(x, n) = |----     power(x^2, n/2)         (n is even)
              |----     x*power(x^2,(n-1)/2)    (n is odd)
```

```
double recursive_power_DC(double x, int n)
{
  if(n == 0) return 1;
  else if((n%2) == 0) return recursive_power_DC(x*x, n/2);
  else return x * recursive_power_DC(x * x, (n-1)/2);
}
```
분할 정복 알고리즘의 부문제는 한 개이고 부문제의 크기는 n에서 n/2또는 (n-1)/2으로 대략 절반으로 줄어듦

`T(n) = T(n/2) + O(1)` 마스터 정리 a = 1, b = 2, c = 0이므로 a = b^c T(n) = Θ(logn)
