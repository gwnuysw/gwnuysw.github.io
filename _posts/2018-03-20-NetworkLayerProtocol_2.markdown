---
layout: post
title:  "네트워크층 프로토콜(ICMPv4)"
date:   2018-03-20 21:42:00 +0900
categories: jekyll update
---
### 인터넷 제어 메시지 프로토콜 ICMPv4

IP데이터그램의 전달과정에서 **오류를 소스호스트에 알리고, 호스트의 상태 진단 및 관리를 위한 프로토콜** 이다.

* 메시지

  메시지는 크게 오류보고메시지와 질의 메시지로 나눌 수 있다.

  * 오류보고 메시지 타입과 코드값


  |Type|Name|Code|comment|
  |:----:|:-----:|:-----:|:------:|
  |03|Destination unreachable|0~15|목적지 찾을 수 없음|
  |04|Source quench|only 0|사용 되지 않음(역압과 비슷)|
  |05|Redirection|0~3||
  |11|Time exceeded|0 and 1|time to live 필드|
  |12|parameter problem|0 and 1| |

  * 질의 메시지 타입과 코드값


  |Type|Name|Code|comment|
  |:----:|:-----:|:-----:|:------:|
  |08 and 00|Echo request and reply|only 0|ping test에서 사용|
  |13 and 14|timestamp request and reply|only 0||
