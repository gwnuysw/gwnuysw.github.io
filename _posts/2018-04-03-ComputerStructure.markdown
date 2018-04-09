---
layout: post
title:  "컴퓨터 구조 컴퓨터 연산"
date:   2018-04-03 15:14:00 +0900
categories: jekyll update
---

###  Arithmatic for computers

- addition, add
- subtraction, subtract
- multiplication, multiply
- division, divide

---

### subtraction

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

---
### multiplication


![multiplication](http://s3.amazonaws.com/answer-board-image/9003dea7-8194-4b0a-b559-223471873883.png)
_IMGsource:[CheggStudy](http://www.chegg.com/homework-help/questions-and-answers/1using-design-multiply-0101-multiplier-0111-multiplicand-write-steps-value-register-stepsi-q1733999)_

```
8*9

    1000  //multiplicand(shift left)
*   1001  //multiplier(shift right)
-----------
    1000
   0000
  0000
 1000
-------
 1001000 : 72(product)
```

|n|multiplicand|multiplier|product|
|:---:|:------:|:--------:|:-----:|
|0|0000 1000|1001|0000 0000|
|1|0001 0000|0100|0000 1000|
|2|0010 0000|0010|0000 1000|
|3|0100 0000|0001|0000 1000|
|4|||0100 1000|

---
### division

![division](https://cdn.hackaday.io/images/9038841462229407929.png)
_IMGsource:[hackaday](https://hackaday.io/project/10889-spdt16-16-bits-arithmetic-unit-with-relays/log/37371-multiplication-and-division)_
```
74/8 = 9...2

(74 : dividend, 8 : divisor, 9 : quotient, 2 : reamainder)

       0000 1001
     _____________
1000 | 0100 1010    ----->  74/8
   - | 0000 0000
     -------------
     | 0100 1010
   - | 0100 0000
     -------------
     | 0000 1010
   - | 0000 0000
     -------------
     | 0000 1010
   - | 0000 0000
     -------------
     | 0000 1010
   - | 0000 1000
     -------------
       0000 0010
```

|n|remainder|divisor|quotient|
|:---:|:---:|:-----:|:------:|
|0|0100 1010|1000 0000|0000|
|1|0100 1010|0100 0000|0000|
|2|0000 1010|0010 0000|0001|
|3|0000 1010|0001 0000|0010|
|4|0000 1010|0000 1000|0100|
|5|0000 0010|0000 0100|1001|
