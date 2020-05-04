---
layout: post
title:  "dotnet cli로 C#프로젝트 만들기"
date:   2020-05-04 20:55:00 +0900
categories: jekyll update
comments : true
---

visual studio에서는 프로젝트를 솔루션 안에 넣습니다. 루트 디렉토리는 솔루션이 됩니다.

관련 링크 : https://docs.microsoft.com/en-us/visualstudio/ide/creating-solutions-and-projects?view=vs-2019

dotnet 명령어는 cmd에서도 해도 되고 powershell 에서 해도 됩니다.

# 만드는 순서

일단 워킹 디렉토리로 이동해서 명령어를 사용합니다.

1. `dotnet new sln -n "솔루션이름"`
2. `dotnet new console -n "프로젝트이름"`
3. `dotnet new classlib -n "라이브러리이름"`
4. `dotnet sln 솔루션파일이름 add 프로젝트이름/프로젝트이름.csproj`
5. `dotnet sln 솔루션파일이름 add 라이브러리이름/라이브러리이름.csproj`
6. `dotnet add 프로젝트이름/프로젝트이름.csproj reference 라이브러리이름/라이브러리이름.csproj`

1단계에서 솔루션을 만들고 2,3단계에서 프로젝트와 라이브러리를 만듭니다. 라이브러리는 선택사항입니다.
1단계에서 솔루션을 만들면 ~.sln이라는 text파일이 있는데 거기에 2,3단계에 대한 내용을 추가해 주어야 합니다.
그 작업이 4,5단계입니다. 그리고 6단계에서 프로젝트가 라이브러리를 참조하도록 만듭니다.

끝~

참고 : https://www.youtube.com/watch?v=r5dtl9Uq9V0&list=LLKywhLfBT0ykwRLlgOWUy8g&index=2&t=0s
