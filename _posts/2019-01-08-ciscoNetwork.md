---
layout: post
title:  "패킷 트레이서로 VLAN(Virtual LAN) 설정해보기"
date:   2019-01-08 10:39:00 +0900
categories: jekyll update
comments : true
---

![vlan그림](https://blog.boxcorea.com/wp/wp-content/uploads/2017/12/vlan-router-273x300.png)

(출처 : https://blog.boxcorea.com/wp/archives/2104)


![내 캡처](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/%EC%A0%84%EC%B2%B4%EA%B5%AC%EB%8F%84.png?raw=true)

이번 시간은 위와같은 vlan을 구성해 볼겁니다. 스위치, 라우터, PC구성을 차례로 해주어야 합니다.

## 스위치 구성

**vlan만들기**

2960스위치를 중앙에 가져다 놓구요 fa0/0, fa0/1의 vlan이름은 fox이고, fa0/2, fa0/3의 vlan이름은 cat 입니다.

```
SWITCH>enable
SWITCH#conf t
SWITCH(config)#vlan 100
SWITCH(config-vlan)#name FOX
SWITCH(config-vlan)#exit
SWITCH(config)#vlan 200
SWITCH(config-vlan)#name CAT
SWITCH(config-vlan)#end
```
`sh vlan`명령어로 설정한 vlan을 확인해 볼수 있습니다. 원래 vlan 서버, 클라이언트 설정이 다른데 디폴트로 서버니까 건드리는거 없이 다음으로 넘어갑니다.

**스위치 포트에 vlan할당**

vlan fox

```
Switch(config)#int fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 100
Switch(config-if)#int fa0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 100
Switch(config-if)#end
```

vlan cat

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int range fa0/3-4
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 200
Switch(config-if-range)#end
```

`show interface status`명령어로 할당된 내용을 확인 할 수 있으며, `range` 명령어를 통해 여러 포트를 한번에 할당 할 수도 있습니다.

**트렁크 포트 할당하기**

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int fa0/24
Switch(config-if)#switchport mode trunk
```

맨 마지막 포트를 라우터와 연결할 트렁크 포트로 설정합니다. 클라이언트 스위치를 더 연결해야 한다면 다른 포트도 트렁크 포트로 설정해야 합니다. 스위치 2960의 경우 IEEE801.1Q 트렁킹 밖에 없으므로 다른 설정 없이 넘어갑니다.


**트렁크 포트의 native vlan 설정**

이게 뭐하는 건진 모르겠습니다. 그냥 따라합니다.
```
Switch(config)#int fa0/24
Switch(config-if)#switchport trunk native vlan 100
Switch(config-if)#exit
```

**vlan ip설정**

얘도 왜하는지는 모릅니다. 스위치의 인터페이스에는 ip주소를 할당 안하는데 vlan에는 하네요? 왜그럴까요
```
Switch(config)#int vlan 100
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan100, changed state to up

Switch(config-if)#ip address 192.168.10.2 255.255.255.0
Switch(config-if)#int vlan 200
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan200, changed state to up

Switch(config-if)#ip address 192.168.20.2 255.255.255.0
Switch(config-if)#end
```

디폴트 게이트 웨이도 설정해 줍니다.
```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip default-gateway 192.168.10.1
Switch(config)#
Switch(config)#end
```

## 라우터 설정

라우터는 2901라우터를 가져다 놓습니다.

fox vlan 과 cat vlan사이에 통신이 가능하도록 설정합니다. 스위치에서 게이트웨이로 설정했던 주소를 할당해줍니다. 그리고 vlan 100은 서브인터페이스로 잡지 않도록 하라는군요 이것도 왜그런진 모르겠네요
```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip routing
Router(config)#int Gig0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#int Gig0/0.1
Router(config-subif)#encapsulation dot1q 200
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
Router(config-subif)#end
```

## PC설정

- fox (gateway 192.168.10.1)
  - 192.168.10.11/24
  - 192.168.11.12/24
- cat (gateway 192.168.20.1)
  - 192.168.20.11/24
  - 192.168.20.12/24

## 패킷 보내 보기

**브로드 캐스팅**

### fox
![boradcasting1](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/broadcast1.png?raw=true)
![boradcasting2](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/broadcast2.png?raw=true)

### cat
![boradcasting3](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/broadcast3.png?raw=true)
![boradcasting4](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/broadcast4.png?raw=true)

스위치에서 해당하는 vlan으로만 브로드캐스틍을 해줍니다.
**그냥 한놈한테 보내기**

![pc2topc0-1](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/Screenshot%20from%202019-01-08%2011-55-01.png?raw=true)
![pc2topc0-2](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/Screenshot%20from%202019-01-08%2011-55-07.png?raw=true)
![pc2topc0-3](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/vlan/Screenshot%20from%202019-01-08%2011-55-19.png?raw=true)

직접 해보시면 알곗지만 브로드캐스팅과 다르게 패킷이 라우터까지 도달한후 라우터에서 목적지로 갑니다.


참고 : https://blog.boxcorea.com/wp/archives/2104 여기 보고 따라했습니다. 다른 포스팅도 해봤는데 브로드캐스팅은 잘되고 서로다른 vlan간의 통신은 안돼는 경우가 많더라구요 근데 이건 브로드캐스팅과 vlan간 통신이 잘 이루어 졌습니다.
