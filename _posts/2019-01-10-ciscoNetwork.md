---
layout: post
title:  "패킷 트레이서로 RIP설정해 보기"
date:   2019-01-10 11:02:00 +0900
categories: jekyll update
comments : true
---

먼저 RIP(Routing Information Protocol)에 대해서 알아볼 필요가 있습니다.

RIP는 라우터가 디스턴스 벡터를 관리하며 목적지 까지 가장 적은 홉수를 거치는 경로를 찾아주는 프로토콜입니다. 사람이 일일이 경로를 설정해주는 정적라우팅이 아니라 알아서 경로를 찾는 동적 라우팅 프로토콜인 것이죠

장점
- 표준 라우팅 프로토콜이다.
- 메모리를 적게 사용한다.

단점
- 회선의 속도는 고려하지 않는다.
- 홉수가 15로 제한되어있다. 그 이상은 도달 불가능 하다고 판단한다.

따라서 RIP는 내부 네트워크를 구성하는데 적합합니다.

## 직접 해보기

![전체 구도](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/rip/%EC%A0%84%EC%B2%B4%EA%B5%AC%EB%8F%84.png?raw=true)

- 라우터 : 2901 3대
- 스위치 : 2960 3대
- pc 3대

ip 주소는 왼쪽부터 다음과 같습니다.

- 왼쪽 라우터
  - se0/0 : 203.240.250.2/24
  - Gig0/0 : 192.168.130.1/24
- 가운데 라우터
  - se0/0 : 203.240.150.1/24
  - se0/1 : 203.240.250.2/24
  - Gig0/0 : 203.240.100.1/24
- 오른쪽 라우터
  - se0/0 : 203.240.150.2/24
  - Gig0/0 : 203.240.200.1/24
- 왼쪽 pc
  - gate way : 192.168.130.1
  - ip : 192.168.130.2/24
- 가운데 pc
  - gate way : 203.240.100.1
  - ip : 203.240.100.2/24
- 오른쪽 pc
  - gate way : 203.240.200.1
  - ip : 203.240.200.2/24

## 라우터 설정하기

제일먼저 HWIC-2T, HWIC-4ESW 모듈을 각 라우터에 부착 합니다. 그러고 나서 각각 시리얼인터페이스와 이더넷 인터페이스에 ip설정을 해주면 되는데 이제 다들 잘하시리라고 생각해서 대표적으로 가운데 라우터 명령어만 적겠습니다.

```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#int se0/0/0
Router(config-if)#ip address 203.240.150.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int se0/0/1
Router(config-if)#ip address 203.240.250.2 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#int Gig0/0
Router(config-if)#ip address 203.240.100.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```
여기 까지 하면 라우터에 인터페이스 설정은 모두 끝났습니다. 나머지 오른쪽 왼쪽 라우터도 똑같은 방식으로 설정해 주면 됩니다. 그리고 중요한것이 rip설정을 해야합니다.

### RIP설정 하기

rip 는 꼭 config모드로 진입하고 나서 명령어를 입력해야 합니다. 어렵지 않으니 마찬가지로 가운데 라우터 설정만 보여드리겠습니다.
```
Router(config)#router rip
Router(config-router)#network 203.240.150.0
Router(config-router)#network 203.240.250.0
Router(config-router)#network 203.240.100.0
```
저렇게 `router rip`로 rip구성 모드로 들어가서 `network network-number`만 차례로 입력해 주면 끝입니다!! 주의할점은 서브네트워크 번호를 입력해야 한다는 것입니다.

이것이 rip구성 전부 입니다. 그리고 제가 잘 모르겠는점은 왼쪽 라우터의 Gig0/0 주소만 192.168.130.xxx 입니다. 처음엔 다른 주소들과 같은 형식으로 203.240.350.xxx를 입력하려고 했지만 불가능한 주소라고 자꾸떠서 다르게 설정 했습니다. 아시는분 댓글 남겨주세요~
