---
layout: post
title:  "컴퓨터 구조 부호 있는 수와 없는 수"
date:   2018-03-21 21:59:00 +0900
categories: jekyll update
comments: true
---
### Signed and unsigned numbers
#### Unsigned
* unsigned numbers : 0 ~ 2^32-1

#### Signed
* 양수
  ```
  00000000000000000000000000000000(2) = 0(10)
  ...
  01111111111111111111111111111111(2) = 2^31-1(10)
  ```

* 음수
  ```
  1000000000000000000000000000000000(2) = -2^31
  ...
  1111111111111111111111111111111111(2) = -1
  ```

#### convert binary into decimal
