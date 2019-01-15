---
layout: post
title:  "패킷 트레이서로 스탠다드 액세스 리스트 설정해보기"
date:   2019-01-15 10:40:00 +0900
categories: jekyll update
comments : true
---
액세스 리스트는 라우터에서 한 인터페이스에 대해 접근 불허가/허가 목록을 말합니다.

그 중에서도 스탠다드 액세스 리스트는 패킷의 출발지 주소만 보고 이 주소에 대해 해당 인터페이스에 접근을 결정하는 것입니다.

라우터 설정은 지난번에 RIP설정 했던것을 그대로 사용해서 맨 오른쪽 라우터에 pc 하나를 더 가져다 놨습니다.

![전체 구도](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/%EC%8A%A4%ED%83%A0%EB%8B%A4%EB%93%9C%EC%95%A1%EC%84%B8%EC%8A%A4%EB%A6%AC%EC%8A%A4%ED%8A%B8.png?raw=true)

왼쪽 라우터
- se0/0 : 203.240.250.2/24
- Gig0/0 : 192.168.130.1/24

가운데 라우터
- se0/0 : 203.240.150.1/24
- se0/1 : 203.240.250.2/24
- Gig0/0 : 203.240.100.1/24

오른쪽 라우터
- se0/0 : 203.240.150.2/24
- Gig0/0 : 203.240.200.1/24

왼쪽 pc
- gate way : 192.168.130.1
- ip : 192.168.130.2/24

가운데 pc
- gate way : 203.240.100.1
- ip : 203.240.100.2/24

오른쪽 pc1
- gate way : 203.240.200.1
- ip : 203.240.200.2/24

pc2
- gate way : 203.240.200.1
- ip : 203.240.200.3/24

이 네트워크 구성을 가지고 가운데 라우터의 gig이더넷에 대하여 접근설정을 하려고 합니다. 다른 ip는 모두 접근 허가 하는 대신 새로추가한 pc, 203.240.200.3만 접근 불허를 하려고 합니다.

가운데 라우터의 인터페이스에 대한 접근권한 명령을 아래와 같이 입력합니다.

```
Router(config)#access-list 2 deny 203.240.200.3
Router(config)#access-list 2 permit 192.168.130.0 0.0.0.255
Router(config)#access-list 2 permit 203.240.200.0 0.0.0.255
Router(config)#int Gig0/0
Router(config-if)#ip access-group 2 out
Router(config-if)#^Z
Router#
%SYS-5-CONFIG_I: Configured from console by console
```
여기 까지가 설정 명령어 입니다. 혹시 실수 하면 앞에 `no access-list 2 permit ...(생략)` 처럼 앞에 no만 붙이면 되긴 하는데 모든 액세스 리스트가 한번에 지워지므로 실수 하지 않도록 합니다. 또 인터페이스 설정 할때 `ip access-group 2 out`에서 out은 gig0/0인터페이스에서 나오는 설정을 하기 때문입니다. se0/0/0과 se0/0/1인터페이스 에서 설정한다고 하면 in으로 바꿔줘야 합니다.

```
Router#sh ip access-list
Standard IP access list 2
    10 deny host 203.240.200.3
    20 permit 192.168.130.0 0.0.0.255
    30 permit 203.240.200.0 0.0.0.255
```
위 명령어는 설정한 액세스 리스트를 확인 할 수 있습니다. **여기서 주의할 점은 라우터에 들어온 패킷에 대하여 액세스 리스트의 맨 위부터 차례로 확인한다는 것입니다. 만약 `deny 203.240.200.3`이 `permit 203.240.200.0`보다 뒤에있다 된다면 deny명령은 아무 효과가 없게 되고** 203.240.200.3도 그냥 통과 시켜 버립니다. 또한가지 주의 할 점은 액세스 리스트의 마지막은 `access-list 2 deny all`이 생략되어 있습니다. 즉, access-list에 명시돼지 않은 주소는 모두 차단하는 것입니다.
