---
layout: post
title:  "uipath REFramework 연습문제"
date:   2020-05-19 08:49:00 +0900
categories: jekyll update
comments : true
---

### 비즈니스 시나리오

UiDemo라고 하는 출납원 신청이 있습니다. 트랜잭션 삽입 처리를 자동화를 해야합니다. 트랜잭션 관련 데이터는 엑셀 파일에 저장되어있습니다. Read Range Activity와 반복문을 사용하여 DataTable의 한 행씩 UiDemo에 트랜잭션을 등록합니다. 하지만 엄청난 양의 트랜잭션 때문에 여러대의 로봇을 동시에 실행해야 합니다. 그래서 우리는 5대의 로봇을 할당 해야 합니다.

### 비즈니스 분석

1. 입력 데이터는 숫자가 아닙니다.
2. 1000$ 이상 금액은 처리할 수 없습니다.

### 설계

실패한 트랜잭션은 두번 더 시도하게 설계해야 합니다. 예외가 발생 하더라도 로봇은 스스로 복구해서 작업을 계속 진행 해야 합니다. 마지막 시도가 실패하면 에러를 Thtrow 하고, 재시도하는 트랜잭션은 warning 로그를 남겨야 합니다.

### Task

자동화 프로젝트는 Orchestrator Queue와 REFramework를 사용합니다.

[트랜잭션 엑셀파일](https://html.cdn.contentraven.com/crcloud/crscorm/uploads/uipath_lms_11218/encryptedfile/150802/v3.0/scormcontent/assets/DpIeB7v5hVbx4NUi_APkLmFv3flTJmd4_-re-framework-20-20-transactions-20-excel.zip)

[REFramework Uidemo Application](https://html.cdn.contentraven.com/crcloud/crscorm/uploads/uipath_lms_11218/encryptedfile/150802/v3.0/scormcontent/assets/tIykUBCz8z5eeZi4_RNYgWaSZ-eDphT1v-re-framework-20-20-ui-demo-20-application.zip)

### Credentials for UiDemo

Username : admin

Password : password

## 답 해설

제일 먼저 크게 두가지 워크플로로 나누어서 진행 합니다.

- Dispatcher
  - Dispatcher - UploadQueue.xaml 시퀀스 사용
  - Orchestrator item에 트랜잭션 데이터를 전송하는 역할을 합니다.
  - read range로 엑셀을 읽어서 for each row 안에서 add queue item을 사용합니다.
- Performer
  - Main.xaml State machine 사용
  - REFramework를 사용하는 부분입니다.
  - State들
    - Initialization
      - Entry의 Try Catch를 먼저 실행합니다.
        - Try
          - `if (Config is Nothing)`
            - Framework\InitAllSettings.xaml를 호출합니다.
              - 매개변수
                - String in_ConfigFile = "Data\Config.xlsx"
                - ArrayString in_ConfigSheets = {"Settings", "Contains"}
              - 결과
                - Dictionary Config = out_Config
              - For each를 이용하여 in_ConfigSheets의 sheet들을 하나씩 엽니다.
                - sheet하나씩 열어서 받은 데이터테이블을 For each row를 이용하여 out_config에 넣습니다.
                - `out_Config(Row("Name").ToString.Trim) = Row("Value")`
              - Try Catch를 이용해서 Orchestrator Asset에 있는 데이터를 가져옵니다.
                - 엑셀 Asset sheet에 있는 Asset Name을 이용합니다.
                - Read Range로 엑셀을 읽고
                - For each row
                  - 또 Try Catch를 이용합니다.
                    - Try
                      - Get Orchestrator asset
                      - `out_Config(row("Name").ToString) = AssetValue`
            - `if (Not String.IsNullOrWhiteSpace(in_OrchestratorQueueName))`
              - Orchestrator Queue Name이 엑셀이 있으면 가져옵니다.
              - `Config("OrchestratorQueueName") = in_OrchestratorQueueName`
            - Framework\KillAllProcesses.xaml를 호출합니다.
              - Kill Process 액티비티로 이미 켜져있는 UiDemo앱을 끕니다.
            - Add Log Fields
              - 나중에 실행중 에러 팝업창이 떳을때 여기에 지정한 내용을 추가로 보여줍니다.
              - `logF_BusinessProcessName = Config("logF_BusinessProcessName").ToString`
              - Config엑셀 Settings Sheet에 logF_BusinessProcessName이 있어야 합니다.
          - Framework\InitAllApplications.xaml를 호출합니다.
            - 매개변수
              - Dictionary in_Config = Config
            - Open Application을 이용해서 UiDemo를 실행합니다.
              - UiDemoSubfolder\UiDemo_Login.xaml를 호출합니다.
                - 매개변수
                  - `String in_Credential = in_Config("UiDemoCredential").ToString`
                - UiDemoSubfolder\GetAppCredentials.xaml를 호출합니다.
                  - 출력
                    - String Username = out_Username
                    - SecueString Password = out_Password
                  - 입력
                    - String in_Credential = in_Credential
                - Attatch Window를 이용해서 로그인 레코딩을 합니다.
                  - Type into : ID
                  - Type Secure Text : password
                  - Click : login button
        - Catch
          - System Exception = Exception
      - 전환
        - SystemException이 비어있는 경우
          - Get Transaction Data로 전환
        - SystemException에 뭐가 있는 경우
          - End Process로 전환
    - Get transaction Data
      - Entry를 실행 합니다.
        - shouldstop 액티비티로 Orchestrator에서 누가 Stop을 했는지 체크합니다.
        - if (shouldstop)
          - then
            - `TransactionItem = Nothing`
          - else
            - Try Catch
              - try
                - 입력
                  - Int in_TransactionNumber = TransactionNumber
                  - Dictionary in_Config = Config
                  - DataTable io_TransactionData = TransactionData
                - 출력
                  - QueueItem out_TransactionItem = TransactionItem
                  - String out_TransactionField1 = TransactionField1
                  - String out_TransactionField2 = TransactionField2
                  - String out_TransactionID = TransactionID
                - Get Transaction Item
                  - 결과물 : out_TransactionItem
              -  `if (out_TransactionItem isNot Nothing)`
                - then
                  - out_TransactionID = now.ToString
                  - out_TransactionField1 = string.Empty
                  - out_TransactionField2 = string.Empty
              - Catch
                - TransactionItem = Nothing
              - 전환
                - TransactionItem isNot Nothing인 경우
                  - Process Transaction으로 전환
                - TransactionItem Is Nothing 인 경우
                  - End Process로 간다.
    - Process Transaction
      - Entry
        - Try Catch
          - Try
            - BusinessException = Nothing
            - Process.xaml 호출
              - 입력
                - QueueItem in_TransactionItem = TransactionItem
                - Dictionary in_Config = Config
              - 이 입력들을 flow차트를 이용해서 잘 사용합니다.
          - Catch
            - BusinessRuleException
              - BusinessException = Exception
            - Exception
              - SystemExceptino = Exception
          - 전환
            - SystemException이 있는 경우 Initialization으로
            - BusinessException이 있는 경우 Get Transaction Data로
            - 둘다 없는 경우 Get Transaction Data
    - End Process
      - UiDemo를 종료합니다.

출처 : https://academy.uipath.com/learningpath-viewer/1992/1/150802/2
