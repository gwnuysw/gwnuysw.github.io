---
layout: post
title:  "패킷 트레이서로 OSPF 설정해보기"
date:   2019-01-14 10:48:00 +0900
categories: jekyll update
comments : true
---

우선 OSPF(Open Shortest Path First)를 RIP와 비교하며 알아 보겠습니다.

## 정보 교환 주기
RIP의 경우 라우터간 30초 마다 정보를 교환하여 업데이트 하지만 OSPF는 네트워크 변화가 생길때 바로 업데이트를 합니다. 따라서 대역폭을 낭비 하지않으면서도 최신 정보를 DB에 보관 하고 있는 겁니다.

## VLSM(Virable Length Subnet Mask)
RIP v1이나 IGRP의 경우 라우터의 인터페이스마다 서브넷 마스크가 모두 같아야 합니다. 그러나 OSPF는 다른 서브넷 마스크를 부여 할 수있어서 ip주소를 효율적으로 사용 할 수 있습니다.

## 네트워크 크기 제한
RIP의 경우 최대 홉은 15이고 IGRP는 255입니다. 그러나 OSPF는 이런 제한이 없습니다.

## 경로 결정
IGRP와 마찬가지로 다양한 요소를 종합적으로 판단하여 경로를 결정합니다. 홉수만 따지는것은 RIP밖에 없네요

## 적용할 수 있는 네트워크 토폴로지(Topology)
OSPF가 적용되는 토폴로지는 따로 있습니다. OSPF는 토폴로지가 바꿔는 것에 따라 약간씩 동작이 바뀐다고 하네요
![topology](http://img1.daumcdn.net/thumb/R1920x0/?fname=http%3A%2F%2Fcfile1.uf.tistory.com%2Fimage%2F990813335A28F68C213FBB)

(출처 : http://jdh5202.tistory.com/272)

## DR과 BDR

DR(Designated Router)는 Link state링크 상태를 관리하는 녀석입니다. 모든 라우터들이 각자 link state 를 관리 할때 보다 트래픽이 적고 라우터들이 한번에 같은 정보를 갖기 때문에 Sync가 맞게 된다는 장점이 있습니다. BDR은(Backup Designated Router)는 DR이 잘 동작하는지 관리하고 DR이 다운 되었을때 DR로 승격하고 다른 BDR을 뽑게 됩니다.

### DR, BDR선정 방식

1. Priority 가 높은 순
2. 라우터 ID가 높은 순

priority나 라우터 ID모두 OSPF패킷에 들어 있는 정보들입니다. priority는 디폴트로 1이고 따로 설정해 줄 수 있습니다. priority 가 같게 되면 라우터 ID를 보고 DR, BDR을 고르게 되는데 라우터 ID는 해당 라우터 인터페이스들 중에서 가장 높은 IP주소가 라우터 ID가 됩니다. 하지만 인터페이스는 상황에 따라 다운될 수도 있고 다운 됐던게 다시 살아날 수 도 있는 것이기 때문에 Loopback IP 로 라우터 ID를 설정 한다고 하네요 loopback ip가 뭐하는 건지는 모르겠네요 가상의 ip주소인가?

그리고 네트워크에 또다른 라우터가 추가 된다고 해도 바로 DR, BDR이 재선정 되는 것이 아니라 DR이 죽게되면 BDR이 자리를 물려받고 그 BDR 자리를 재선정 한다고 합니다.

### DR, BDR 선출된 후 새로운 라우터의 OSPF 작동 과정

1. OSPF가 시작 되면 이웃 라우터를 찾기 위해 그 라우터는 멀티캐스트 주소(224.0.0.5)를 이용하여 모든 OSPF라우터 들에게 hello 패킷을 전송 합니다.
2. 라우터는 이 hello 패킷을 이용하여 DR, BDR주소를 알게 됩니다.
3. 새 라우터는 자신이 가지고 있는 Link state 정보를 DR에게 전송합니다. 이때도 멀티캐스트(224.0.0.6)를 이용합니다.
4. DR이 이 정보를 받으면 BDR은 타이머를 키고 타이머가 다 될때 까지 DR이 반응이 없으면 고장났다고 판단하고 DR로 승격합니다. 그리고 BDR을 다시 선출 합니다.
5. DR은 이 정보를 멀티캐스트(224.0.0.5)를 이용하여 다른 OSPF라우터 들에게 전송하고 ack를 받습니다.
6. 새 라우터의 다른 인터페이스가 끊어 지면 즉시 DR에게 알리게 됩니다.

## 직접 해보기

![전체 구도]()

명령어가 너무 간단 합니다. 똑똑한 프로토콜일수록 구성도 간단해 집니다.

왼쪽 라우터
```
Router(config)#int Gig0/0
Router(config-if)#ip address 172.16.10.1 255.255.255.0
Router(config-if)#int se0/0/0
Router(config-if)#ip address 192.168.12.1 255.255.255.240
Router(config-if)#exit
Router(config-router)#int Gig0/0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int se0/0/0
Router(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
Router(config-if)#exit
Router(config)#router ospf 100
Router(config-router)#network 172.16.10.0 0.0.0.255 area 0
Router(config-router)#network 192.168.12.0 0.0.0.15 area 0
Router(config-router)#exit
```
가운데 라우터
```
Router(config)#int se0/0/0
Router(config-if)#ip address 192.168.12.2 255.255.255.240
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int se0/0/1
Router(config-if)#ip address 192.168.23.2 255.255.255.240
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#router ospf 100
Router(config-router)#network 192.168.12.0 0.0.0.15 area 0
Router(config-router)#network 192.168.23.0 0.0.0.15 area 0
```
오른쪽 라우터
```
Router(config)#int se0/0/0
Router(config-if)#ip address 192.168.23.3 255.255.255.240
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int Gig0/0
Router(config-if)#ip address 172.16.30.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#router ospf 100
Router(config-router)#network 172.16.30.0 0.0.0.255 area 0
Router(config-router)#network 192.168.23.0 0.0.0.15 area 0
```
`router ospf 100`할때 100 과 같이 숫자가 뒤에 따라오는 것은 한 라우터에서 여러개의 ospf를 톨릴때 사용합니다. `area0`은 이 세개의 라우터를 연결한 네트워크 영역이 모두 area0에 있다는 뜻입니다. 그리고 `network 172.16.30.0 0.0.0.255 area 0`에서 서브넷 마스크가 아닌 것이 뒤에 따라 오는데 이는 **와일드 마스크**라고 합니다. 서브넷 마스크에서 0, 1을 뒤집으면 와일드 마스크가 되는데 왜 쓰는지는 모르겠네요.

```
Router#sh ip ospf interface

Serial0/0/0 is up, line protocol is up
  Internet address is 192.168.23.3/28, Area 0
  Process ID 100, Router ID 192.168.23.3, Network Type POINT-TO-POINT, Cost: 64
  Transmit Delay is 1 sec, State POINT-TO-POINT, Priority 0
  No designated router on this network
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:06
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 192.168.23.2
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0 is up, line protocol is up
  Internet address is 172.16.30.1/24, Area 0
  Process ID 100, Router ID 192.168.23.3, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 192.168.23.3, Interface address 172.16.30.1
  No backup designated router on this network
```
DR을 확인 하고 싶으면 `sh ip ospf interface`를 입력하면 됩니다. 그리고 우리가 만든 네트워크는 point to point 라는 것도 알 수있네요

```
Router#show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.23.2      0   FULL/  -        00:00:38    192.168.23.2    Serial0/0/0
```
위 명령어는 이웃 라우터를 검색하는 명령어 입니다.
