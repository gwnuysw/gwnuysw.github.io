---
layout: post
title:  "네트워킹 VLAN(Virtual LAN) 이란?"
date:   2019-01-07 09:58:00 +0900
categories: jekyll update
comments : true
---

vlan 이란 한대의 스위치를 마치 여러 대의 분리된 스위치 처럼 사용할 수 있는 기술을 말합니다. 즉 하나의 스위치에 연결된 장비들의 브로드 캐스트 도메인이 다를 수 있습니다.

![vlan sample](https://www.thomas-krenn.com/de/wikiDE/images/4/4f/VLAN-Grundlagen-Beispiel-3.png)

(그림 1)
> image source : https://www.thomas-krenn.com/en/wiki/VLAN_Basics

위 그림에 하나의 스위치에 두개의 다른 색갈로 포트가 구분 되는데 이는 브로드 캐스트 도메인이 두가지 라는 뜻입니다. vlan은 네트워크 구성 변경을 쉽게 하기 위해서 등장한 기술입니다.


![no vlan](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/packetTracer/pt2/VLAN-Grundlagen-Beispiel-2.png?raw=true)

(그림 2)

그림에서 스위치 A와 B가 다른 사무실이고 스위치 A의 포트 4번을 쓰던 사람이 네트워크는 그대로 유지하면서 스위치 B가 있는 사무실로 자리를 옮겨야 한다면 vlan이 없을때는 사무실 A 에서 부터 선을 길게 뽑아 와서 써야 할 것입니다. 하지만 vlan이 가능 하다면 그냥 스위치 B번에 초록색 포트 아무데나 꽂으면 네트워크 A를 그대로 쓸 수 있는 것이죠

### 트렁크 포트 Trunk port

라우터와 스위치, 스위치와 스위치 간에 정보교환은 그림 1상에서 포트 8번을 통해 이루어 집니다. 이를 트렁크 포트(Trunk port)라고 하는데 이 포트를 통해 각각의 VLAN정보가 한꺼번에 전송됩니다. 그래서 8번 포트 색깔이 초록색과 주황색이 섞여있는 모습입니다. **단, 같은 스위치 라도 서로 다른 vlan과의 통신은 라우터를 만드시 거쳐야 합니다.** 트렁킹 할때 각 데이터에 해당하는 VLAN 이름표를 붙이는데 CISCO에서 사용하는 방식인 ISL과 산업표준인 IEEE802.1Q 방식이 있습니다.

### Native VLAN
IEEE802.1Q 방식에서 native VLAN이란 것을 사용 합니다. 패킷에 VLAN정보를 붙이지 않고 보내는 VLAN으로 모든 스위치 네트워크에서 유일하게 한개의 VLAN만 native VLAN으로 세팅 할수 있습니다.

### VTP (VLAN Trunking Protocol)

사진처럼 스위치를 2개만 쓰지 않고 여러 개의 스위치를 연달아 연결하여 네트워크를 구성 하는 것이 일반 적입니다.
스위치의 구성 변경이 있을 때마다 여러개의 스위치의 설정을 직접 변경해 주어야 하는데 VTP는 각 스위치 구성 변경을 자동으로 해주는 프로토콜 입니다. VTP는 산업 표준이 아니라 스시코 회사 제품에만 제공 되는 거라고 하네요. 자동 구성을 위해 스위치 별로 역할 분담을 해야 합니다.

1. VTP서버 : VLAN을 생성, 삭제, 변경하며 VLAN정보를 저장하여 어쩌다 전원이 꺼지더라도 VLAN정보를 복구 할 수 있습니다.
2. VTP 클라이언트 : VLAN 생성, 삭제, 변경을 할 수 없으며 단지 VLAN 서버가 전달 해준 정보를 다른 스위치에 전달하는 것만 합니다. 또한 VLAN정보도 따로 저장하지 않습니다.
3. VTP Transparent : VTP 도메인안에 존재 하는 독립 적인 VTP 모드 라고 생각 하면 될거 같습니다. VTP 서버가 준 업데이트 정보를 반영하지 않고 자기만의 VLAN 생성, 삭제, 변경이 가능 하고 또 자기만의 정보를 다른 스위치에게 알리지 않습니다. 그러나 서버로 부터온 메시지는 다른 스위치 들에게 전달해줍니다. (근데 왜 Transparent를 쓰는지는 모르겠네요 서버, 클라이언트로 부족한가?)

각각의 구성요소는 다음과 같은 메시지를 주고 받습니다.

1. Summary Advertisement : 서버가 자신과 연결되어있는 스위치들에게 매 5분 마다 한번씩 Revision number를 전달합니다. 클라이언트 들은 이 숫자를 보고 자신의 VLAN정보가 최신 상태인지 판단합니다. 만약 클라이언트 VLAN이 최신이 아니라면 Advertisement Request 를 보내는데 이떄 서버는 즉시 Summary Advertisement를 보냅니다.
2. Subset Advertisement : VLAN정보가 들어있는 메시지입니다. 당연히 Advertisement Request를 받았을때 전송하며 Summary Advertisement와 함께 전송 합니다.
3. Advertisement Requset : 클라이언트가 자신의 revision 넘버보다 큰 revision 넘버를 받았을 때 즉, 자기의 VLAN정보가 최신이 아닐때 서버에게 보냅니다. 또한 VTP도메인 이름이 바뀌거나 스위치가 리셋 됐을 때도 VTP서버에게 보냅니다.
