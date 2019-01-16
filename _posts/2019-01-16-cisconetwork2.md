---
layout: post
title:  "라우터에 nat 설정하는법"
date:   2019-01-16 11:55:00 +0900
categories: jekyll update
comments : true
---

nat이 뭔지는 이전 포스트 찾아보면 있습니다. 오늘은 네트워크 구성하기도 귀찮고 그냥 명령어만 간단히 알아보고 넘어갈게요

```
ip nat pool ccie 201.98.100.2 210.98.100.254 netmast 255.255.255.0
ip nat inside source list 1 pool ccie
ip nat inside source static 10.1.1.100 210.98.100.100
access-list 1 permit 10.1.1.0 0.0.0.255
^Z
int e 0
ip address 10.1.1.1 255.255.255.0
ip nat inside
^Z
int s 0
ip address 210.98.100.1 255.255.255.0
ip nat outside
^Z
```

- `ip nat pool ccie 201.98.100.2 210.98.100.254 netmast 255.255.255.0` 는 공인 ip를 설정하는 부분이고 pool이름 ccie는 사용자가 마음대로 지정할 수 있습니다.
- `ip nat inside source list 1 pool ccie` 내부망 밖으로 나가는 패킷에서 출발지 사설 ip주소가 access-list에 있는 주소라면 pool ccie에 있는 공인 ip로 변환 하겠다는 뜻입니다.
- `ip nat inside source static 10.1.1.100 210.98.100.100` 이경우 공인 ip와 사설 ip의 짝을 맞춰줘야 하는 경우 이용하는 명령어입니다.
- `access-list 1 permit 10.1.1.0 0.0.0.255` 사설 ip를 access-list에 명시합니다. 그래야 사용할 수 있나봐요 nat도 access-list를 이용하는 거였다니 신기하네요
- 다음은 이더넷 인터페이스로 넘어가서 `ip nat inside`는 내부망 이라는 것을 명시합니다.
- 시리얼 인터페이스에서는 `ip nat outside`로 외부 망이라는 것을 알려줍니다.

참 쉽죠~?
