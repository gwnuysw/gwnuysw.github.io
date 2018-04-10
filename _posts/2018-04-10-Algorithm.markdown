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
