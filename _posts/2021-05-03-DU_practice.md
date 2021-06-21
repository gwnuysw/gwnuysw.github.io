---
layout: post
title:  "Document Understanding 연습문제 만들기"
date:   2021-05-03 14:03:00 +0900
categories: jekyll update
comments : true
---

# 연습용 데이터 만들기

https://expressexpense.com/make-receipt-online.php?style=Itemized%20Receipt

https://receiptwriter.com/fast-food-restaurant-template-with-itemized-food-and-tax.html

https://www.zoho.com/invoice/free-invoice-generator.html

https://create.onlineinvoices.com/templates

https://invoice-generator.com/#/8

https://products.aspose.app/words/ko/invoice

# Document Understanding 시중 제품

- abby
- naver clova ocr
- uipath document understanding
- 인지소프트

# 목차

- OCR 대해
  - OCR 소개
    - OCR 역사
  - uipath 기본 ocr 액티비티들 설명
    - 연습데이터
      - Receipt 3종류 총 60장의 이미지
      - invoice 3종류 총 60장의 이미지
      - 이용한 사이트 소개
    - tesseract
    - microsoft
    - google
    - omni
- receipt 유형 1개 구조(structured) OCR 연습
  - tesseract와 image magick으로 연습
- receipt, invoice 유형 다구조 문서 DU
  - Document Understanding 대해
    - Document Uunderstanding 설명
    - 시중에서 서비스 중인 document ocr 기술
      - abby
      - 네이버 클로바 도큐먼트 ocr
    - ~~약점~~
      - ~~out of box schema 부재~~
      - ~~validation의 한계~~
        - ~~사실은 OCR의 성능 이상의 기능을 할 수 없다.~~
      - ~~Extractino Retraining은 아직 preview 단계다~~
  - Document Understanding 진행 절차
  - Document Understanding 인프라
    - DU 프로세스 실행중 ai fabric을 사용하는 시점, Data Manager를 사용하는 시점
    - Extraction 학습시키기
  - Document Understanding 절차별 설명
    - Taxonomy
    - Digitization
    - Classify
    - Human Validation
      - Attended
      - Unattended
    - Retraining
    - Extraction
    - Validation
      - Attended
      - Unattended
    - Export
    - 더 알아보기(custom model)

# 참조

https://tedium.co/2017/03/22/ocr-typography-optical-character-recognition-history/

https://en.wikipedia.org/wiki/Timeline_of_optical_character_recognition

https://docs.uipath.com/document-understanding/docs/public-endpoints

https://imagemagick.org/script/command-line-options.php#deskew
