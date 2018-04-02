---
layout: post
title:  "멀티미디어란 영상처리"
date:   2018-04-02 21:32:00 +0900
categories: jekyll update
---

# Computer Graphics

* 그림을 다루는 방법

  Scanner나 digitizer를 통해 입력된 그림을 디지털 형태의 데이터로 바꾸어 디스크에 자장한 후 필요시 프린터나 플로터를 통해 출력

* 그래픽 데이터의 표현 방법

  * Bitmap방식

    * 점(Pixel)의 조합을 그림을 표현하는 방식
    * 하나의 점을 표현하기 위해 최소 1bit이상의 데이터가 요구됨
    * 장점

      Display속도가 Vector방식에 비하여 빠르다.

    * 단점

      디스크 공간 소모, 확대, 축소시 그림의 모양이 변형 된다.

  * Vector방식

    그림을 나타내기 위한 간단한 명령령어와 파라메터 들만을 표현하는 방법

## 그래픽 파일 포맷

* GIF

  * Compuserve사에서 제안
  * 압축률이 뛰어나며 UNIX계열 워크스테이션에서도 운영가능
  * 단색부터 256color까지 지원

* JEPG

  * Joint Photographic Expert Group에서 개발
  * 24비트 화상에 대하여 무손실로 최대 1/25까지 압축가능
  * 약간의 손실을 감수한 경우 최대 1/100까지 압축 가능
  * 주로 멀티미디어, 인터넷에서 사진등을 압축할 때 사용
