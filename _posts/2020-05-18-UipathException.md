---
layout: post
title:  "uipath 에러처리"
date:   2020-05-18 14:11:00 +0900
categories: jekyll update
comments : true
---
## 일반적인 예외들

모든 예외들은 **System.Exception**에서 파생 되었습니다. 따라서 Try/Catch 액티비티에서 이 예외를 사용하면 모든 예외들을 catch할 수 있습니다.

- NullReferenceException : 일반적으로 초기화하지 않은 변수를 사용할때 발생 합니다.
- IndexOutOfRangeException : 개체의 인덱스가 컬렉션의 크기를 벗어난 경우에 발생합니다.
- ArgumentException : 메소드가 호출되고 전달 된 인수 중 하나 이상이 호출 된 메소드의 매개 변수 정의와 다를 때 발생합니다.
- SelectorNotFoundException : 로봇이 타임 아웃 기간 내에 대상 앱에서 activity에 대해 지정된 selector를 찾을 수 없을 때 발생합니다.
- ImageOperationException : 시간 초과 기간 내에 이미지를 찾을 수 없을 때 발생합니다.
- TextNotFoundException : 시간 초과 기간 내에 표시된 텍스트를 찾을 수 없을 때 발생합니다.
- ApplicationException : 응답하지 않는 응용 프로그램과 같은 기술적 인 문제에 기인 한 오류를 설명합니다.

#### Business Rule Exceptions
비즈니스 규칙 예외는 위에 나열된 모든 시스템 예외와 별개입니다. 이것은 자동화 프로젝트가 의존하는 특정 데이터가 불완전하고 누락되었거나 설정된 경계를 벗어난 사실에 근거한 오류를 설명합니다 (예 : 일일 한도보다 ATM에서 더 많은 데이터를 추출하려고하는 경우)

Try Catch activity에서 일반 System.Exception을 사용하여 비즈니스 규칙 예외가 발생하지 않습니다. 이 예외를 처리하는 메커니즘은 프로세스 소유자가 설정 한 규칙에 따라 개발자가 별도로 정의하거나 간단한 Throw activity를 사용하여 프로세스 실행 중지로 줄일 수 있습니다.


## Try Catch

- try : 예외가 발생할 가능서이 있는 워크플로우 혹은 activity를 실행 합니다.
- catch : try에서 예외가 발생했을 때 수행할 워크플로우 혹은 activity를 실행 합니다. 여러 예외 및 그에 해당하는 활동 추가 가능
- finally : try만 정상적으로 실행하던지 catch까지 실행 하던지 항상 실행합니다.

## Retry Scope

Retry Scope activity는 조건이 충족되지 않거나 오류가 발생하는 한 포함 된 활동을 재 시도합니다.

 Retry Scope activity는 오류를 포착하고 처리하는 데 사용되므로 Catch 시도와 유사합니다. 차이점은 복잡한 처리 메커니즘을 제공하는 대신 실행을 재 시도한다는 것입니다. 또한 종료 조건없이 사용할 수 있습니다.이 경우 예외가 발생하지 않거나 제공된 시도 횟수를 초과 할 때까지 활동을 재 시도합니다.

 **추가 속성**

 - NumberOfRetries : 시퀀스가 재 시도되는 횟수입니다.
 - RetryInterval : 각 재시도 사이의 시간 (초)을 지정합니다.

## ContinueOnError Property

오류가 발생하더라도 실행을 계속 해야하는지 여부를 지정하는 특성.

범위가있는 activity (예 : attach browser 또는 open browser)에서 ContinueOnError가 True로 설정되어 있으면 해당 범위 내의 다른 activity에서 발생하는 모든 오류도 무시됩니다.

이 속성을 true로 설정하는 것이 모든 상황에서 권장되는 것은 아니지만 다음과 같은 상황이 의미가 있습니다.

- 데이터 스크래핑을 사용하는 동안-마지막 페이지에서 활동이 오류를 발생시키지 않습니다.( '다음'버튼의 선택기를 더 이상 찾을 수 없을 때)
- 우리는 오류를 포착하는 데 관심이 없지만 단순히 활동의 실행에 관심이 있습니다.

이 필드는 부울 값 (True, False) 만 지원합니다. 기본값은 False이므로 필드가 비어 있고 오류가 발생하면 프로젝트 실행이 중지됩니다. 값을 True로 설정하면 오류에 관계없이 프로젝트 실행이 계속됩니다.

## Global Exception Handler

Global Exception Handler는 프로젝트 수준에서 실행 오류가 발생할 때 동작을 결정하도록 설계된 워크 플로 유형입니다. 자동화 프로젝트당 하나의 Global Exception Handler만 설정할 수 있습니다.

**어떻게 작동하나?**

Global Exception Handler에는 2 개의 사전 정의 된 인수가 있으므로 제거하면 안됩니다.

- In 방향의 errorInfo : 실패한 워크 플로우에서 발생 된 오류 포함합니다.
- Out 방향의 result : 오류가 발생하면 프로세스의 다음 동작을 저장합니다.

또한 2개의 activity도 있는데 이건 지우면 안됍니다.

- Log Error : 이 부분은 단순히 오류를 기록합니다. 개발자는 치명적, 오류, 경고, 정보 등 로깅 수준을 선택할 수 있습니다.
- Choose Next Behavior : 여기서 개발자는 실행 중에 오류가 발생했을 때 수행 할 작업을 선택할 수 있습니다.
  - Continue : 예외를 다시 던집니다.
  - Ignore : 예외를 무시하고 다음 액티비티로 넘어갑니다.
  - Retry : 예외를 발생한 액티비티를 다시 실행 합니다.
  - Abort : 실행을 정지합니다.
  - 위 네가지를 `ErrorAction.Retry`, `ErrorAction.Abort`같이 사용합니다.

  참고 : uipath academy
