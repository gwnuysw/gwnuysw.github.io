---
layout: post
title:  "Game Assets in Roblox"
date:   2021-11-08 17:55:00 +0900
categories: jekyll update
comments : true
---

> Refference : https://developer.roblox.com/en-us/articles/game-assets

텍스쳐, 모델, 오디오, 비디오를 포함한 모든 게임 에셋은 온라인에 저장됩니다. 다른 엔진과는 다르게 플레이어와 개발자를 위한 로컬 에셋이 존재하지 않습니다. 이는 협업을 돕고, 오래된 디바이스에서 플레이하는 플레이어의 스토리지 부담을 덜어줍니다.

## General Asset Information
모든 에셋에는 각 Roblox계정과 관련된 고유 ID를 가지고 있습니다. 에셋을 업로드 하면 자동적으로 Roblox moderation 팀에 제출 됩니다. Moderation은 몇 분 경과 되며, 한번 승인 되면 여러분의 에셋은 Roblox 스튜디오 에서 사용할 수 있습니다.

## Images
3d 텍스쳐, 입자 효과, GUI 메뉴등에 이용하는 이미지들은 스튜디오를 통해 업로드 할 수 있습니다. Roblox에서는 .png, .jpg, .tga, .bmp 이미지만 활용합니다. View탭의 Asset Manager 윈도우를 클릭하고 게임이 배포되면, import 버튼을 누르고 import 하면 됩니다.

## Meshes
MeshPart는 physically-simulated object입니다. .fbx, .obj 형식으로 업로드가능합니다. image import하는 방식과 같은 방식으로 import 합니다.
