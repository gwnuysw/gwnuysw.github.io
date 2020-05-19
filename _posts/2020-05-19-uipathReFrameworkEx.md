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

출처 : https://academy.uipath.com/learningpath-viewer/1992/1/150802/2
