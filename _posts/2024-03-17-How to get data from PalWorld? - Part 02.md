---
title: How to Get Data from PalWorld? - Part 02
description: Continuing the guide on collecting data from PalWorld using Unreal Engine and UE4SS.
categories: [Project]
date: 2023-03-17
tags: [Data Collection, Unreal Engine, UE4SS]
---


### Lua Modding

Unreal Engine으로 만들어진 게임에 모드를 적용하는 것은 2가지 방법이 있다.   
하나는 Lua 파일을 Mod 폴더에 넣는 것이고,   
다른 하나는 Blueprint (BP)를 작성하여 `pak`파일로 Pal 폴더에 넣는 것이다.   

두 가지 방법 모두 장단점이 있다.   
Lua는 스크립트 언어이기 때문에 코딩에 익숙해야 하지만,   
빠르게 프로토타입을 만들고 테스트 할 수 있으며 BP보다 좀 더 많은 것을 코딩으로 해결할 수 있다.   
그에 반해 BP는 코딩 없이 블럭과 선으로 마치 레고처럼 결과물을 만들 수 있어 접근성이 좋지만,  
일일히 `pak` 파일을 만드는 것은 상당히 귀찮다.     
특히 Unreal Engine의 변수 호출 방식과 수많은 함수에 익숙하지 않은 초기에는  
코드 실행이 잘 되는지 계속 부딪혀가며 확인해야 하기 때문에 코드로 일단 돌려보는 것이 좋다고 생각한다.     

그리고 무엇보다 이 프로젝트에서는   
OS 환경과 연결하여 csv 파일을 기록할 것이기 때문에 Lua를 통한 Mod 파일을 작성하는 방법을 선택한다.  


### Preparations

Lua를 사용한 Mod를 PalWorld에 적용하려면     
우선 UE4SS라는 언리얼 엔진용 스크립팅 서비스를 설치해야 한다.     
Lua API를 통해 다양한 작업을 가능하게 해준다.     
이것이 없었다면 우리는 엄청난 노가다를 해야 했을 것이다.     
UE4SS은 GuiConsole을 제공하기 때문에 로그를 확인하며 디버깅을 할 때도 굉장히 편하다.     
또한 EnableHotReloadSystem = 1로 설정하면   
모드 수정 후 게임을 굳이 종료하고 다시 시작하지 않아도 Ctrl + R 로 바로 적용할 수 있다.       

#### 1. UE4SS - Unreal Engine 4/5 Scripting System   
   -> [Github Link](https://github.com/UE4SS-RE/RE-UE4SS)  

Lua API, C++ API, Live Viewer 등의 기능 지원     

[여기](https://github.com/UE4SS-RE/RE-UE4SS/releases) 최신 release 버전에서     
GUI console을 사용하기 위해 일반 버전인 'UE4SS.zip' 대신에      
개발자용인 'z-DEV-UE4SS.zip'을 다운 받고 압축 풀기    

`/{Gameroot}/GameName/Binaries/Win64/`에 압축을 풀고 나온 파일들을 붙여 넣는다.     
(`Mods`, `dwmapi.dll`, `UE4SS.dll`, `UE4SS-settings.ini`)    

외부 디렉토리에서 서로 다른 게임들에 UE4SS를 동시 적용하는 심화적인 방법은     
[이 링크](https://docs.ue4ss.com/dev/installation-guide.html)를 참고할 수 있다. ~~이건 몰라도 된다~~    


#### 2. PalWorld를 실행하여 추가적인 창이 뜨는지 확인한다.     

만약 잘 설치 되었다면 처음보는 창이 같이 실행될 것이다.     
여기에는 5가지의 탭이 있다.     

**Console** : 가장 많이 쓰게 된다. mod 파일에서 print()를 사용하면 여기에 나타난다.    
**Live View** : 메모리에 담겨 있는 항목들을 실시간으로 보여준다.     
**Watches** : Live View에서 특정 항목의 변화를 모니터링 하고 싶을 때 사용한다.     
**Dumpers** : Dump CXX Headers, Generate Lua Types 등의 기능이 있다.     
**BP Mods** : Lua 모드를 사용하기 때문에 여기서는 쓰지 않는다.     

#### 3. 자신이 만들 Mod file을 추가한다.       

```
Mods\
    ...
    MyLuaMod\
        scripts\
            main.lua
    enabled.txt
mods.txt
    ...
```

enabled.txt를 추가한다. 빈 내용이라도 상관없다.     
게임 실행 시 자동으로 enabled.txt가 있는 Mod를 추가해서 실행시킬 것이다.     
만약 실행이 되지 않는다면, mods.txt에 모드 파일 이름('MyLuaMod')를 추가하고     
MyLuaMod : 1 이런 식으로 추가해준다.     


#### 4. FModel 설치  

우리는 이제 모드를 이제 만들 수 있다.   
그런데 게임에 대한 정보가 없다.   

캐릭터의 이동 속도를 조절하고 싶은데   
이 값은 어디 저장 되어 있으며 어느 수치를 가지고 있는지    
외부에서는 도저히 알 수가 없다. 이는 직접 게임 데이터를 파헤쳐보아야 한다.     
그런데 이런 데이터들은 uasset 파일들로 되어 있기 때문에 직접 열어서 볼 수가 없다.     
그래서 이런 Blueprint, DataTable 등의 uasset 파일들을 볼 때 필요한 툴이 바로 **FModel**이다.     

프로그램을 다운 받고, 경로와 추가적인 파일을 넣어줘야 하는데     
자세한 설명은 https://pwmodding.wiki/docs/asset-swapping/StartingOut 여기에 잘 정리되어 있다.     

Content안의 DataTable 폴더를 json 으로 변환해서 저장하고,     
이후 VScode 등으로 불러오면 쉽게 검색하며 게임의 DataTable을 탐색할 수 있다.  



___
#### Reference 

[UE4SS Documentation](https://docs.ue4ss.com/dev/lua-api.html)  
[PalWorld Modding Wiki](https://pwmodding.wiki/docs/lua-modding/ue4ss-functions)  
[Unreal Engine 5 Docs](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Engine/GameFramework/)  
