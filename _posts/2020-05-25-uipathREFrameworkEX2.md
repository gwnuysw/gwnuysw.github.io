---
layout: post
title:  "uipath REFramework 연습문제2"
date:   2020-05-25 10:55:00 +0900
categories: jekyll update
comments : true
---
## Generate Yearly Report 문제 설명

**1단계**

ACME System1 web application을 연다.

결과 : ACME System1 WebApp이 열려 있어야 한다.

**2단계**

로그인을 수행한다.

**3단계**

대시보드 화면이 떳나 확인

**4단계**

워크아이템에 들어간다. 데이터 테이블을 받는다.

**5단계**

for each activity로 WI4인 것을 필터링 한다.

**5-1단계**

5단계에서 받은 데이터로 각 details 페이지에 들어가서 vendor Tax ID를 뽑아온다.

**5-2단계**

go back 액티비티를 이용해서 대시보드 화면으로 갑니다. 그래서 mothly report를 다운로드 페이지로 갑니다.

**5-3단계**

vendor TaxID을 입력하고 2017년과 관련된 모든 monthly reports를 다운 받습니다.

**5-4단계**

월간 보고서를 모두 하나의 파일"Yearly-Report-2017-TAXID.xlsx"로 모아줍니다.

**5-5단계**

하나로 모은 파일을 올리기 위해 "Upload Yearly Report"에 들어갑니다.

**5-6단계**

Vendor TaxID, 2017년으로 설정하고 파일을 업로드 하는데 그 결과 uploadID가 나오는데 이거 나중에 쓰니까 저장해야합니다.

**5-7단계**

워크아이템 디테일즈에 가서 "Update work item"을 클릭합니다.

**5-8단계**

status를 "Completed",를 입력하고 코멘트를 다음과 같이 남깁니다. "UploadedwithID"

## 힌트

#### 디스패처

- REFramework를 만듭니다.
  - 우리는 트랜잭션 아이템으로 WIID를 업로드 할겁니다.
  - 데이터 스크래핑할때 > 화살표가 안먹히는 경우도 대비하고, 페이지 항해도중 문제가 발생헀을 때 다시 실행하도록 만들겁니다. 우리는 한 페이지를 워크아이템 리스트로 간주할 거고 page number를 transactionitem으로 간주할 겁니다.
  - 트랜잭션 아이템은 현재 진행중인 프로세스의 문자열 숫자를 의미합니다.
- Config 파일 설정
  - Settings sheet에서 inHouse_Process4 값을 QueueName파라미터에 넣습니다. 오케스트레이터에 같은 이름으로 큐를 만들겁니다.
  - Systeam1 URL과 System1 Credential parameters를 설정합니다.
  - Constants Sheet에서, MaxRetryNumber를 2로 설정해 주세요
- 프레임워크에 다음 내용을 변경해주세요
  - Mine파일 GetTransactionData, Process, SetTransactionStatus파일 TransactionItem 변수를 System.String으로 바꿔주세요
- 우리는 ACME System1사이트만 이용할거기 때문에 System1폴더를 루트 디렉토리에 만듭니다.
  - 그아래에 System1_Login.xaml, System1_Close.xaml, System1_NavigationTo_WorkItems.xaml에 넣으면 됩니다.
  - SendEmail.xaml도 복사하다가 Common폴더에 붙여넣습니다.
- InitAllApplications를 열고
  - System1 Login을 넣고다시 System1_NavigateTo_WorkItems.xaml을 넣습니다.
- CloseAllApplication을 열고
  - System1_Close.xaml파일을 넣습니다.
- KillAllProcesses.xaml파일에
  - add a Kill Process Activity를 넣고 "Kill process IE"라고 이름을 바꿈니다.
- GetTransactionData.xaml을 열고
  - Get Transaction Item 액티비티를 삭제합니다.
  - attach browser 액티비티를 맨위에 놓습니다.
  - Element Exists액티비티를 넣어서 next page가 가능한지 확인합니다. Element Exists selector도 수정하는데 in_TransactionNumber.ToString을 이용합니다.
  - if activity를 이용해서 버튼이 존재할 경우
    - then
      - out_TransactionItem = in_TransactionItem.ToString
      - else
        - out_TrasactionItem = Nothing
- Process.xaml파일을 엽니다.
  - 다음 페이지를 넘어가는데 click액티비티를 사용하고 마찬가지로 dynamic seletor를 이용합니다.
  - On Element Appear액티비티를 넣습니다. 그래서 진행중인 페이지가 열려있는지 확인합니다.
