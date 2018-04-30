---
layout: post
title:  "ComputerStructure Accessing a Cache"
date:   2018-04-30 14:45:00 +0900
categories: jekyll update
---

---
### e.g1

22->26->22->26->16->3->16->18->16

|order|main memory address|cache address|valid|tag|data|hit or miss|
|:---:|-----:|:-----------:|:---:|---|----|:---------:|
|1|22(10110)|110|Y|10|mem(22)|miss|
|2|26(11010)|010|Y|11|mem(26)|miss|
|3|22(10110)|hit|hit|hit||hit|
|4|26(11010)|hit|hit|hit||hit|
|5|16(10000)|000|Y|10|mem(16)|miss|
|6|3(00011)|011|Y|00|mem(3)|miss|
|7|16(10000)|hit|hit|hit||hit|
|8|18(10010)|hit|hit|hit||hit|
|9|16(10000)|hit|hit|hit||hit|

---
### e.g2

32bit byte address

cache size
* (#cache 2^10 blocks)(10 bit index)
* block size 1word(4byte)

32bit byte address
* 31~11bit : tag
* 11~2bit : index
* 2~0bit : byte offset

```
total size
= (index) * (valid + tag + 1word) 
= 1024*(1+20+32)
= 53Kbit
```
