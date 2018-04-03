---
layout: post
title:  "컴퓨터 구조 컴퓨터 연산"
date:   2018-04-03 15:14:00 +0900
categories: jekyll update
---

# 3.2 Arithmatic for computers

- addition, add
- subtraction, subtract
- multiplication, multiply
- division, divide

* subtraction

  addition 2's complement numbers

  * decimal

    -3 -> 7(borrow 10)

  * binary

    -0011 -> 1101 (borrow 2^4)


```
4-3

    0100
+   1101
  -------
   10001  // -3 빌린 숫자이기 때문에 carry는 정상적이다.

-3-4

    1101
+   1100
  -------  
   11001  // -3, -4에서 borrow를 두번 했지만 1개밖에 못값는다.

4+5

    0100
+   0101
  -------  
    1001 // -7 wrong overflow      
```

# 3.3 multiplication

```
8*9

    1000  //multiplicand(shift left)
*   1001  //multiplier(shift right)
-----------
    1000
   0000
  1000
 1000
-------
 1001000 : 72(product)
```
