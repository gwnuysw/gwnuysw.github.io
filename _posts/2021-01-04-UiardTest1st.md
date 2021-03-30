---
layout: post
title:  "uipath uiard 시험공부"
date:   2021-01-04 09:45:00 +0900
categories: jekyll update
comments : true
---

**GenericValue**

uipath 만든 DataType, 데이터 스크랩 관련 액티비티 실행 후 스크랩 데이터가 어떤 형태인지 모를때 임시로 사용한다. GenericValue로 연산을 할때는 알아서 형변환을 해주지만 어떻게 형변환 해주는지 그때그떄 다르기 때문에 명시적으로 형변환을 해주어야 한다.
```
GenericValue var1 = "123";
GenericValue var2 = 123;
GenericValue var3 = var1 + var2; //var1, var2 연산 순서 주 목

LogMessage(var3);

> "123123"

GenericValue var1 = "123";
GenericValue var2 = 123;
GenericValue var3 = var2 + var1; //var1, var2 연산 순서 주목

LogMessage(var3);

> 246
```
