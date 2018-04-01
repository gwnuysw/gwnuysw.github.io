---
layout: post
title:  "컴퓨터 구조 pseudo instructions"
date:   2018-04-01 13:54:00 +0900
categories: jekyll update
---
# MIPS pseudo instructions
MIPS assembler recognizes, but not real instructions

|Name|Assemly Syntax|Expantion|
|:---|:-------------|:--------|
|Load immediate|`li reg, value`|`lui reg, upper_hi`<br> `ori reg, reg, valure_lo`</br>|
|move|`move $t, $s`|`add $t, $zero, $s`|

```
li $t, 0x1234abcd
->
lui $t, 0x1234
ori $t, 0xabcd

move $t, $s
->
add $t, $0, $s
```
