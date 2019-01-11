---
layout: post
title:  "패킷 트레이서로 IGRP 설정해보기"
date:   2019-01-11 11:35:00 +0900
categories: jekyll update
comments : true
---
IGRP(Interior Gateway Routing Protocol)은 RIP의 후속작으로 표준 프로톹콜이아니라 시스코에서 만들어낸 프로토콜입니다. 따라서 시스코 라우터에서만 가능합니다.

RIP의 여러 단점들이 있었습니다.
- 최대 15홉
- 홉수만 기준 삼아서 목적지 경로 결정

반면 IGRP는
- 최대 255홉
- 대여폭, 지연시간, 회선 신뢰성, 부하, MTU(최대 전송 유닛 크기)를 기준으로 목적지 경로를 결정합니다.

## 직접 해보기

![전체 구도]()

네트워크 모양은 지난 시간 RIP와 같습니다. 또한 설정 명령어 역시 RIP와 상당히 비슷합니다.

- 라우터 : 2901 3대
- 스위치 : 2960 3대
- pc 3대

ip 주소는 왼쪽부터 다음과 같습니다.

- 왼쪽 라우터
  - se0/0 : 203.210.200.2/24
  - Gig0/0 : 150.150.1.1/16
- 가운데 라우터
  - se0/0 : 203.210.200.1/24
  - se0/1 : 203.210.100.1/24
  - Gig0/0 : 203.210.10.1/24
- 오른쪽 라우터
  - se0/0 : 203.210.100.2/24
  - Gig0/0 : 172.70.100.1/24
- 왼쪽 pc
  - gate way : 150.150.1.1
  - ip : 150.150.1.10/16
- 가운데 pc
  - gate way : 210.240.10.1
  - ip : 210.240.10.2/24
- 오른쪽 pc
  - gate way : 172.70.100.1
  - ip : 172.70.100.10/24

### 라우터 ip설정 하기

이번에도 가운데 라우터만 다루겠습니다. 양쪽 라우터는 적절하게 ip주소 변경해서 입력해주세요
```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#int se0/0/0
Router(config-if)#ip address 203.210.200.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#exit
Router(config)#int se0/0/1

Router(config-if)#ip address 203.210.100.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#exit
Router(config)#int Gig0/0
Router(config-if)#ip address 210.240.10.1 255.255.255.0
Router(config-if)#no shutdown
```

### igrp설정하기

```
Router(config)#router eigrp 100
Router(config-router)#network 203.210.100.0
Router(config-router)#network 203.210.200.0
Router(config-router)#network 210.240.10.1
Router(config-router)#
```

RIP랑 정말 유사하죠? 라우터에 붙어있는 네트워크 주소만 적어주면 됩니다.
