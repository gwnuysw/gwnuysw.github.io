---
layout: post
title:  "uipath State Machine(상태머신)"
date:   2020-05-19 09:45:00 +0900
categories: jekyll update
comments : true
---

![상태머신](https://docs.microsoft.com/ko-kr/dotnet/framework/windows-workflow-foundation/media/state-machine-workflows/complete-state-machine-workflow.jpg)

> 출처 : [microsoft workflow foundation](https://docs.microsoft.com/ko-kr/dotnet/framework/windows-workflow-foundation/state-machine-workflows)


위 사진을 상태 머신이라고 합니다. 각 네모칸이 상태(State)를 나타내고 State끼리 연결된 화살표를 전환(Transition)이라고 합니다. 상태머신에 반드시 FinalState가 있어야 합니다.

## State
![State](https://files.readme.io/dc3897b-image_60.png)

State activity 자세한 모습입니다. State가 실행하면 제일 먼저 Entry안에 있는 내용을 실행합니다. 그리고나서 Transition, 화살표를 실행합니다.

## Transition
![Transition](https://files.readme.io/36784cd-image_61.png)

Transition에서 Trigger를 제일 먼저 실행하고 Condition을 평가합니다. Trigger는 어떤 activity가 올 수 있고, 아무 activity가 안 올 수도 있습니다. Condition 이 `false`면 **트랜지션을 취소하고 다시 Trigger를 실행**합니다. `True`인 경우 State에서 Exit을 실행하고 Action을 실행 합니다.

## Sharing Trigger
![상태머신예](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/RPA/TransitionSharingTrigger.PNG?raw=true)

이렇게 생긴 상태머신에서 init State와 Normal State에 Transition을 모두 True로 실행하면 Final State가 실행되지 않고 Init State와 Normal State만 왔다갔다 하게 됩니다.

![SharingTrigger](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/RPA/exampleStateMachine.PNG?raw=true)

이렇게 트리거를 하나로 두고 그 아래 트랜지션을 둔 경우 _공유 트리거 전환(shared trigger transitions)_ 이라고 합니다. 이 경우 위 예제에서 모든 전환 조건이 True이면 Final State로 전환합니다.

## State 전환 작업 실행 순서
1. State의 Entry실행
2. Transition의 Trigger 실행
3. Transition의 condition 평가 (False일 경우 Trigger다시 실행)
4. State의 Exit실행
5. Transition의 Action실행
6. 다른 State로 넘어감

참고 : https://docs.uipath.com/studio/docs/state-machines
