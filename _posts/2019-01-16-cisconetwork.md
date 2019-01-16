---
layout: post
title:  "패킷 트레이서로 HSRP 설정해보기"
date:   2019-01-16 10:51:00 +0900
categories: jekyll update
comments : true
---

HSRP(Hot Standby Routing Protocol)은 외부로 나가는 라우터가 고장나는 것에 대비해서 라우터 한대를 더 구성에 포함한 후, 메인 라우터가 고장 나면 두번쨰 라우터가 그 역할을 대신하는 기능입니다.

HSRP를 이용하기 위해서는 가상의 ip를 사용합니다.

## 직접 해보기

![hsrp전체 구도](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/hsrp%EC%A0%84%EC%B2%B4%EA%B5%AC%EB%8F%84.png?raw=true)

이 네트워크는 지난번 rip네트워크를 조금 변경했습니다. 따라서 라우터 마다 rip설정은 그대로 있어야 합니다.

**ip 주소**

맨위 pc
- gateway 203.240.100.1
- ip 203.240.100.2/24

가운데 라우터
- gig0/0 203.240.100.1/24
- se0/0/0 203.240.150.1/24
- se0/0/1 203.240.250.1/24

왼쪽 라우터
- se0/0/0 203.240.250.2/24
- gig0/0 172.70.100.2/24

오른쪽 라우터
- se0/0/0 203.240.150.2/24
- gig0/0 172.70.100.3

맨 밑 pc
- ip 172.70.100.4/24
- gateway 172.70.100.1

**hsrp 설정**

라우터설정은 왼쪽, 오른쪽 두개 라우터에서만 해주면 됩니다. 우선 왼쪽라우터 부터 보면,
```
Router(conf)#int gig0/0
Router(config-if)#ip address 172.70.100.2 255.255.255.0
Router(config-if)#standby 1 timers 3 10
Router(config-if)#standby 1 priority 105
Router(config-if)#standby 1 preempt
Router(config-if)#standby 1 ip 172.70.100.1
Router(config-if)#standby track se0/0/0
```

1. 첫번째 줄 부터 보면 hsrp 1 그룹에 속한 라우터 끼리 매 3초마다 서로를 확인하는 패킷을 보내며, 주 라우터(액티브 라우터) 에서 10초동안 반응이 없으면 부 라우터(스탠바이 라우터)가 액티브 역할을 수행합니다.
2. 두번째 줄은 이 라우터의 priority를 설정해 주는 명령어 입니다. 이 값이 높은 라우터가 액티브 라우터가 됩니다.
3. 세번째 줄은 주 라우터가 죽었다가 살아나는경우 다시 주라우터로 복귀하는 명령어 입니다. _후니의 시스코 네트워킹_ 책에는 딜레이 시간이 있는데 딜레이 쓰면 오류가 납니다. 명령어가 바뀌었나봐요
4. 네번째 줄은 가상 ip를 의미합니다. 이것은 hsrp그룹에서 동일한 ip를 써야 합니다.
5. 마지막 트랙은 라우터가 고장나진 않았는데 외부로 나가는 시리얼 포트만 망가지는 경우를 대비한 명령어 입니다. 이경우 se0/0/0이 됩니다.


다음은 오른쪽 라우터 입니다.
```
Router(config)#int gig0/0
Router(config-if)#ip address 172.70.100.3 255.255.255.0
Router(config-if)#standby 1 timers 3 10
Router(config-if)#standby 1 priority 100
Router(config-if)#standby 1 preempt
Router(config-if)#standby 1 ip 172.70.100.1
Router(config-if)#standby 1 track se00/0/0
```
유일한 차이점은 priority값입니다. standby 라우터의 값이 더 작은 것을 알수 있는데요 active라우터 가 망가지거나 시리얼이 끊겼을 경우 priority값을 떨어뜨려서 standby 라우터의 값이 더 커지면 그때 standby라우터가 active역할을 맡게됩니다.

**rip수정**

rip도 새로 할당한 주소에 맡게 고쳐줘야합니다. 왼쪽 오른쪽 라우터에서만 수행합니다.

왼쪽의 경우
```
Router(config)#router rip
Router(config-router)#network 172.70.0.0
Router(config-router)#
Router(config-router)#end
```

오른쪽의 경우
```
Router(config)#router rip
Router(config-router)#network 172.70.0.0
Router(config-router)#
Router(config-router)#end
```
분명 서브넷 마스크를 /24로 줬는데 이렇게 해도 잘 되더라구요

![실행화면](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/hsrp1.png?raw=true)

시리얼 한쪽을 잘라서 해봤는데 처음엔 됐다가 지금 안돼네요...뭐 시뮬레이터를 100% 신뢰할 수는 없으니까 한번 됐으면 된거겠죠?
