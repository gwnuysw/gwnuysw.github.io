---
layout: post
title:  "컴퓨터 구조 floating point"
date:   2018-04-09 14:16:00 +0900
categories: jekyll update
---
### Floating Point

real numbers( float in C)

`1.0 * 2^-1 = 10.0 * 2^-2 = 0.1 * 2^0`

binary point is not fixed (floating)

normalized number : `1.xxx * 2^yyy`

---
### Float point representation


```
(-1)^S * (1 + fraction) * 2^(exponent - bias)
```
![IEEE 754](http://cssimplified.com/wp-content/uploads/2014/09/precision.jpg)

* single precision

  `bias = 127`
  ```
  -0.75----dec = -11 * 2^(-2)----binary

  -1.1 * 2^(-1)

  S : 1, fraction : 10000...0(23bits), exponent : -1 + 127 = 0111 1110

  1 | 0111 1110 | 1000...00 (23bits)|
  ```
* double precision

  `bias = 1024`
  ```
  -0.75

  S : 1, fraction : 0111...1110 (52bits), exponent : 011 1111 1110
  ```

```
1|1000 0001|0100...0| ---- single precision

-1.01 * 2^2
```
