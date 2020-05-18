---
layout: post
title:  "Uipath 터미널 자동화"
date:   2020-03-05 14:55:00 +0900
categories: jekyll update
comments : true
---

# 터미널과 MainFrame자동화

## 설치

터미널 액티비티를 사용하기 위해서는 "UiPath.Terminal.Activities"라는 패키지를 다운 받아야 한다.

## 터미널 위자드

Uipath의 터미널 위자드는 (Mainframe/AS400/VT)와 동작하는 TN3270/TN5250/VT 터미널의 자동화를 지원 합니다.

### 'terminal session' 액티비티

provider 콤보박스에 따라 connection type이 바뀐다.

**Supported Providers**

- Attachmate Reflection
  - 다운로드 경로 : https://www.attachmate.com/products/reflection/2014/
  - 사용법 : profile mode를 사용할때 profile file은 반드시 Attachmate Reflection Profile 폴더 아래에 있어야 하고, 'C:\Users\\Documents\Attachmate\Reflection' 가 디폴트다.
- IBM Personal Communications
  - 다운로드 경로 : http://www-03.ibm.com/software/products/en/pcomm
  - 사용법 : profile 모드만 지원 합니다. 프로파일은 ‘C:\Users\\AppData\Roaming\IBM\Personal Communications’ 여기 아래에 있어야 합니다.
- Uipath implementation
  - 서버주소와 터미널 타입을 따로 입력해야함

#### recording wizard
커넥션 설정이 완료 되면, uipath terminal wizard가 자동으로 시작된다.

uipath가 터미널 화면 영역을 잡는 방법에는 두가지가 있다.

- 좌표로 찾기
  - 제일 속편한 방법이라고 한다. 스크린의 행과 열에 따라서 화면 영역을 잡는다. 좌표로 찾는 방법중에 더 발전된 방법은 LabeledBy/FollowedBy/Index 속성을 이용하는 방법이다. 스크린이 조금씩 바뀌는 상황에서 쓰면 좋다. 이때 좌표는 사용 불가, LabeledBy/FollowedBy 가 중복 될때는 index 속성을 사용한다.

- 비주얼  element로 잡기

##### Recorded Action 창

사용 가능한 명령들

- Save&Exit : 터미널 연결을 종료하고 레코딩 액션을 저장한다.
- Stop/Start(레코딩) : 레코딩
- Actions Commands : 특정 Terminal Activity를 생성하는 명령어들이다.
- Refresh : 스크린 화면 청소

## Terminal activity

공통 속성
- DelayMS : 활동이 실행 된 후 대기하는 시간을 나타냄, 밀리세컨 단위
- TimeoutMS : 활동이 실행될 때까지 기다리는 시간을 나타냄, 밀리세컨 단위
- WaitType : 활동을 실행할 터미널이 준비 상태에 있어야 하는지 여부를 지정, 권장값은 "준비"

### Terminal Session activity

주 터미널 액티비티고, 터미널 서버와의 연결을 관리한다. 터미널의 컨테이너라고 보면 된다. 디폴트로 액티비티가 종료하면 터미널의 연결도 끊어진다.(액티비티 종료하기전에 터미널 연결 끊는 작업을 따로 안해줘도 됌)

- Connection String : 커넥션 매개변수를 담고 있는데, 이 값은 연결 설정 하면 자동으로 만들어 진다.
- OutputConnection : optional, 터미널 연결 변수, 터미널 종료후, 다른 터미널에 연결하는 것과 같은 상황에서 쓴다.
- CloseConnection : ExistingConnection 속성과 함께 쓰인다. 터미널 섹션 액티비티 종료후에 연결이 종료 되었는지 알 수 있다.
- ExistingConnection : TerminalConnection Type을 저장하는 변수

여러 터미널 세션을 이용해야 하는 상황이라면, terminal connection을 변수에 넣어서 재사용 하는 방식으로 써야한다.

### Set Field Activity

값을 선택된 영역에 text 값으로 입력하는 액티비티, 편집 가능한 영역이어야한다.

### Get Field Activity

선택된 영역의 text 값 얻을때 씀

### Get Text Activity

전체 스크린의 Text 값을 얻을때 씀

### Send Control Key Activity

터미널에 특수 키를 입력할때 쓴다. 엔터나 f1~f24 같은거, 일반적으로 화면전환에 많이 쓰인다.

### Wait Field Text Activity

선택된 영역에 값이 입력 될때 까지 얼마나 기다릴 것인지 정하는 액티비티

### Wait Screen Text Activity

Wait Field Text Activity랑 비슷하다 차이점은 전체 화면을 감시 한다는 것.

### Advanced Activity

Advanced Activity는 터미널의 좌표와 커서위치에 따라 작동하는 terminal activity입니다. 자동화가 제대로 작동 되길 바란다면 **반드시!!** 화면의 좌표가 바뀌지 않는 경우에만 사용 해야 합니다.

#### Move Cursor Activity

마우스 커서의 위치를 특정 위치로 옮긴다. 커서는 선택된 영역의 맨 앞으로 갑니다.

#### Send Keys Activity

커서가 올라와 있는 영역에 키보드 입력을 합니다. 반드시 Move Cursor Activity를 이용해서 편집 가능한 영역에 커서가 올라가 있어야 합니다.

#### Set Field at Position Activity

Set Field Activity랑 비슷하지만 좌표를 이용해서 영역을 선택한다는게 다릅니다. 그래서 속성에 행과 열을 입력해야 합니다.

#### Get Field at Position Activity

Get Field Activity와 비슷하지만 좌표를 이용해서 영역을 선택합니다. 똑같이 행과 열을 입력해야 합니다.

#### Get Text at Position Activity

특정 position의 설정한 길이 만큼의 text를 읽어옵니다. 그래서 좌표와 길이(숫자)를 입력해야 겠죠?

#### Wait Text at Position Activity

Wait Text at Position Activity와 비슷하지만 행렬 좌표를 이용합니다.
