---
layout: post
title:  "네트워크 유니캐스트 라우팅"
date:   2018-04-01 21:02:00 +0900
categories: jekyll update
---
## 거리벡터 라우팅

이웃한 노드하고만 정보를 공유(벨만 포드 방정식)하여 최소 비용 경로를 계산 한다. RIP 프로토콜이 사용한다.

[거리벡터 라우팅의 문제점](https://www.youtube.com/watch?v=_lAJyA70Z-o)
* 해결방안
  * 수평 분할

    상대 노드로 부터 받은 정보를 다시 상대 노드에게 알릴 필요가 없다.
  * 포이즌 리버스

    상대 노드로 부터 받은 정보를 광고하되 거리값을 무한대로 해서 "이 값을 사용하지 마시오, 내가 아는 것은 당신으로 부터 받았기 때문입니다."라고 경고한다.

---
## 링크 상태 라우팅

모든 경로의 정보를 알고 경로 계산을 한다. 각 라우터가 전체 인터넷에게 라우터가 자신의 이웃에 대해 무엇을 알고있는지 알리는 것을 **플러딩(flooding)** 이라고 한다. OSPF프로토콜이 사용한다.

---
## 경로 벡터 라우팀

AS간 사용되는 라우팅으로 최소 비용이 라우팅 선택 기준이 아니다. ISP가 주관 하는 네트워크가 AS다. 각 ISP마다 사용하는 라우팅 프로토콜이 다르다. AS는 ISP 내부 라우터를 관리 한다.

  * Intra domain routing

    하나의 AS내에서 사용하는 라우팅
  * inter domain routing

    AS간 사용하는 라우팅

  * 스터브 AS

    오직 다른 하나의 AS와만 연결할 수 있다. 데이터 트래픽은 스터브 AS에서 파괴되거나 초기화된다.
  * 다중홈 AS

    다른 AS들과 하나 이상의 연결 상태를 가질 수 있다. 마찬가지로 데이터 트래픽이 지나갈 수 없다.
  * Transit AS

    하나 이상의 AS와 연결되어 있고, 데이터가 통과 하도록 허락한다.

---
## 유니캐스트 라우팅 프로토콜
라우팅 프로토콜은 라우팅 테이블을 만들기 위해 존재하며 응용계층 프로토콜이다.

---
## RIP(Routing Information Protocol)

가장 널리 사용되는 **거리벡터 라우팅 기반 인트라 도메인 라우팅 프로토콜이다.**

* 홉 카운트

  거치는 라우터 갯수 RIP는 최소 비용을 홉 개수로 계산한다.

---
## OSPF(Open shortest Path First)
RIP와 같은 **인트라 도메인 라우팅 프로토콜이다. 링크상태 라우팅 프로토콜을 기반으로 한다.** 패킷양을 줄이기 위해 플러딩을 지역적으로 나눠서 한다.

* 메트릭(Metric)

  경로 선택 기준, 홈수, 왕복시간, 신뢰성등 기준이 다양할 수 있다. 홉수가 기준이 되면 포워딩 테이블이 RIP와 같게 된다.

---
## BGP4(Border Gateway Protocol Version 4)

  현재 인터넷에서 사용하고 있는 유일한 **인터도메인 라우팅 프로토콜이다. 경로 벡터 알고리즘을 기반으로 하고 있다.**

  * eBGP(external)

    다른 AS의 라우터에 접속하는 각 경계라우터에 설치한다.

  * iBGP(internal)

    AS내의 라우터 들은 인트라 도메인과 iBGP를 사용하여 동작한다.

    
  ![bgp](http://bgp.us/wp-content/uploads/2016/04/iBGP-and-eBGP.png)
