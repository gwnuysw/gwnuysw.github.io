---
layout: post
title:  "uipath AI center"
date:   2021-03-31 14:47:00 +0900
categories: jekyll update
comments : true
---

# Document Understanding In AI center

AI Center는 Document Understanding Machine Learning 모델을 실행하는 infrastructure입니다.

Document Understanding Model과는 다음과 같은 단계가 연관 되어 있습니다.

- 문서 샘플과 추출해야하는 데이터 포인터의 요구 사항 수집
- Data Manager를 이용한 Data Labeling
- Labeling된 문서중 학습용을 골라내어 AI Center 저장소에 내보낸다.
- Labeling된 문서중 테스트용을 골라내어 AI Center 저장소에 내보낸다.
- AI Center에서 학습을 실행한다.
- 모델 성능을 평가 한다.
- 학습된 모델을 ML Skill로 AI center에 배포한다.
- RPA workflow에서 Document Understanding ML activity pack을 사용하여 ML skill을 이용한다.

## 구성품

용어

- Ports : 서버 포트번호
- 파란 화살표 : network통신, 화살표가 나온 곳이 요청 하는 곳
- 초록 화살표 : user action
- DU key : uipath.cloud.com 에서 받은 document understanding key
- DU creds : Account Executive 로 부터 받은 사용자 계정
- License File : 진짜 라이센스 파일, YAML형태인듯
- Domain Certificate : Ai Fabric 은 신뢰할 수 있는 인증기관에서 받은 인증서가 필요합니다.
- AIF installer : download.uipath.com에서 받은 tar file(air gapped에 필요)
- OOB bundles : ML 모델을 다운 받을 수 있는 URL? account Executive한테 연락해보란다.(air gapped에 필요)

![Non-Air-Gapped on-prem install](https://files.readme.io/1c09ed0-Sequence1.5x_1.png)
![Air-Gapped on-prem install](https://files.readme.io/f3042e1-Sequence1.5x_2.png)
![Non-Air-Gapped on-prem install DataManager Preview](https://files.readme.io/e09d1b4-Sequence1.5x_3.png)
![Air-Gapped on-prem install DataManager Preview](https://files.readme.io/2350a9c-Sequence1.5x_4.png)

# Data Manager
## Data Manager 설치

https://docs.uipath.com/document-understanding/docs/install-data-manager

궁금하면 찾아보자 일단 스킵!

### Configure Data Manager

https://docs.uipath.com/document-understanding/docs/configure-data-manager

궁금하면 찾아보자 일단 스킵!

## Configure OCR

문서 이미지를 DataManager에 가져오기 위해서는 OCR 설정은 필수입니다.

또한 배포할떄의 OCR과 학습할때의 OCR을 반드시 동일한 엔진으로 사용해야합니다.

## Configure Prelabeling

이미 라벨링 할 수 있도록 필드들을 뽑아놓은 모델이 있을때 씁니다. Prelabeling을 쓰면 시간이 대폭 줄어듭니다. 또한 URL을 제공하는 ML model이어야 합니다.

[prelabeling endpoints](https://docs.uipath.com/document-understanding/docs/public-endpoints)

## Create and Configure Feilds
Feild는 삭제나 수정이 불가능 하기 때문에 신중하게 설정해야 합니다. 다만 ML 모델에서 사용안할 feild는 숨김 처리 할 수 있습니다. 최대 40개 필드 까지 설정 가능합니다

### Colum Feild
예를 들자면 invoice에서 Description이나 Unit Price가 있습니다.

### Regular Feild
문서에서 단 한번만 등장하는 부분입니다. 예를 들자면 invoice에서 invoice number, Total amount와 같습니다.

### Classifier Feild
문서의 종류를 구분하는 필드입니다. 영수증에서 (음식, 호텔, 항공, 운수...) 옥은 invoice에서 화폐(USD, EUR, JPY)등이 있습니다.

## import Documents

**파일명에 특수문자 사용 불가 영문, 숫자, dash(-), under score(_)만 가능**

import는 다음과 같이 여러 방법이 있습니다.
- Schema import
- Raw documents import
- Data Manager Dataset import
- Machine Learning Extractor Trainer dataset import (preview 기능)

## Label Documents

라벨링 작업을 하는 training 데이터들은 균형이 있어야 합니다. 예를 들면 영수증 학습을 시키는데 어느 회사껀 1000장 어느회사껀 10장 이런 식의 데이터는 피해야 합니다. 보통 한 layout에 2-3종류의 문서면 충분합니다.

### Feilds that occur multiple times on the same document

문서에 같은 문자가 여러번 등장할떄 그것이 같은 것을 의미한면 모두 labeling을 해야합니다.

# AI center
## About Project

프로젝트는 비즈니스의 단위이다.

![AICenterProject](https://files.readme.io/d70aa3b-Screenshot_1.png)


## About Datasets

기계학습의 모델이 학습에 참고할 데이터 모음

## About ML Package

ML 패키지는 기계 학습 모델을 교육하고 제공하는 데 필요한 모든 코드와 메타 데이터가 포함 된 폴더입니다.

## building package
python등으로 작성한 ML model을 코드 조금 수정하여 ML Package로 사용할 수 있습니다.

## 즉시 사용 가능한 package (out of box package)

Uipath는 즉시 사용가능한 ai package를 많이 만들어 놨습니다. 또한 오픈소스로 개발중인 패키지도 사용 할 수있습니다.

### Uipath Document Understanding packages

Document understanding package도 종류가 있습니다.

- Document Understanding : 처음부터 학습 시켜야 하는 모델
- Out of the box Pre-trained models : 미리 학습 시켰지만 커스터마이징 해서 다시 학습시킬수 있는 모델
  - invoice : https://du.uipath.com/ie/invoices/info/model
  - Receipt : https://du.uipath.com/ie/receipts/info/model
  - Purchase Orders : https://du.uipath.com/ie/purchase_orders/info/model
  - Utility Bills : https://du.uipath.com/ie/utility_bills/info/model
  - invoice india : https://du.uipath.com/ie/invoices_india/info/model
  - invoice austrailia : https://du.uipath.com/ie/invoices_au/info/model

#### Models Details

- input type : file (pdf, png, bmp, jpeg, tiff)
- input description : 파일은 digitiezed가 완료되어야 하며, input 은 Data Extraction Scope 안에서 해야 합니다.
- output description : ML model에서 모든 feild가 추출된 json 파일, Data Extraction Scope에서 설정되며, ExtractionResult 변수에 담깁니다. Export Extraction Result activity에서 Dataset으로 변경 가능합니다.

#### pipeline

두종류의 학습 pieline이 있습니다.

1. Document Understanding
  - 다양성이 적은 문서는 30 ~ 50 개 문서만 학습 시켜도 좋은 결과를 얻습니다. 다양성이 큰 문서는 regular 필드당 20 ~ 50개의 문서를 학습시켜야 합니다.따라서 10개의 필드면 200~500개가 있어야 합니다. 컬럼 필드를 학습 시키려면 컬럼당 50 ~ 200개 데이터를 학습시켜야 합니다. 다양성이 적은 문서면 5개의 컬럼에 대해 300 ~ 400개의 데이터가 필요합니다. 하지만 다양성이 크다면 1000개는 필요합니다.
2. Invoices, Receipts, Purchase Orders, Utility Bills, Invoices India or Invoices Australia.
  - 이미 존재하는 필드를 정확도를 높이기 위해 학습 시킨다면 100 ~ 500개 문서가 필요합니다. 새로운 필드는 30 ~ 50 개면 됩니다. Classification 필드는 재학습이 불가능합니다.
> reference :  https://docs.uipath.com/document-understanding/docs/ai-center-relation-to-du
>
> reference : https://docs.uipath.com/ai-fabric/v0/docs/about-ai-center
