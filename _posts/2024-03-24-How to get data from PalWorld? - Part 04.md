---
title: How to Get Data from PalWorld? - Part 04
description: Exploring advanced techniques for data collection in PalWorld using Unreal Engine and UE4SS.
categories: [Project]
date: 2023-03-17
tags: [Data Collection, Unreal Engine, UE4SS]
---


### Start from Scratch  

이제 기본 세팅이 끝났으니 PalWorld를 켜보고 테스트 해보자.   

테스트 목표는 베이스캠프 내에서 일하고 있는 Pal들의 위치를 추적하는 것이다. 이를 위해서는  
(1) 인스턴스로 메모리 어딘가에 존재하고 있을 Pal들을 찾고   
(2) 이 친구들의 위치를 어떤 방식으로든 호출하면 될 것이다.  
(3) 그렇게 모은 위치 데이터들을 테이블로 만들어서 외부에 저장한다.  

우선 Pal들을 호출하기 위해서 대체 어떤 이름으로 불러와야 하는지 모르니 게임을 실행시켜서 한 번 찾아보도록 한다.   

하지만 우리는 게임 개발자의 머릿속을 알 수 없으니 이 부분에서 시행착오를 할 수 밖에 없다.   
베이스 캠프에서 돌아다니고 있는 저 놈들을 코드 상으로 'Pal'이라 해놓았을지, 'Monster'인지 'Pawn'인지 'Worker'인지 우린 알 수 없다.   

(심지어 어떤 코드에서는 누가 일본 개발팀 아니랄까봐 대놓고 'Otomo'라고 명시되어 있는게 있었는데   
일본인들은 저게 무슨 뜻인지 바로 알아도 한국인은 모른다... 참고로 Otomo는 수행자, 동반자의 의미라고 한다.   
게임에서 스피어에 넣고 항상 데리고 다니며 전투나 여행에 활용하는 Pal들을 의미.)  

![Pasted image 20240324231445](https://github.com/votus777/votus777/assets/51744036/c24a16ab-fb4d-4fd4-a0b6-8280babdaeac)

일단 'Monster'로 검색을 해보니 Blueprint부터 애니메이션, 사운드 등 다양하게 검색된다.   
넒은 범위에서 좁혀나가는 것이 초반 탐색에서는 유용하다.   
이렇게 훑어보니    
`BP_AIResponsePreset_Escape_to_Battle_C /Game/Pal/Maps/MainWorld_5/PL_MainWorld5.PL_MainWorld5:PersistentLevel.BP_MonsterAIController_Wild_C_2147150525` 라는게 보인다.   
BP_MonsterAIController? 뭔가 단서를 잡은 것 같다.   
하지만 이게 어떤 Pal의 컨트롤러인지 모르겠다. 여기서 2147150525는 아마 고유번호 같은데 이를 한 번 검색해보자.     

![Pasted image 20240324232215](https://github.com/votus777/votus777/assets/51744036/5d20e83c-2ad1-47e3-b0e1-13f52677b2b5)

하단의 Property를 살펴보니 Character에 `BP_PlantSlime`이라 명시되어 있다.   
즉 `BP_MonsterAIController_Wild_C_2147150525`는 현재 플레이어 근처 어딘가에서 돌아다니는 야생 초롱이이다.  
실제로 베이스 캠프 옆에서 돌아다니고 있는 한 쌍의 초롱이들을 발견하였다.   
아무튼 이렇게 코드 상의 이름과 한국어 번역이 다른 경우가 매우 많다.     

그런데 `BP_MonsterAIController_Wild` 가 있으니 `BP_MonsterAIController_Basecamp`도 있지 않을까?   

![Pasted image 20240324233045](https://github.com/votus777/votus777/assets/51744036/fea123e0-61da-4858-8fdf-877c18e221b0)

역시 존재한다. 아까와 같은 방식으로 2146150270 이라는 UID를 가진 친구이다.   
Live View 검색 창에서는 `Path`에 있는 있는 경로까지도 포함해서 검색하다보니   
불필요하게 다른 것들도 같이 검색되는데 이제 불러올 이름을 알았으니   
Part-03에서 언급한 `FindAllOf` 함수를 사용하면 깔끔하게 필요한 인스턴스만 가져올 수 있다.     

```lua
local ActorInstances = FindAllOf("BP_MonsterAIController_BaseCamp_C")
```

그렇다면 한 번 베이스캠프에 있는 친구들의 이름을 불러와 보자.   
코드는 다음과 같다.   

```lua
local pal_list = {}

function CreateWorkerList()

  local ActorInstances = FindAllOf("BP_MonsterAIController_BaseCamp_C")
    
  if ActorInstances and #ActorInstances > 0 then
    print("[TestLua] We have AI Controllers! \n")

    for Index, ActorInstance in pairs(ActorInstances) do
      local str = tostring(ActorInstance.Pawn:GetFullName())
      local name = str:match("BP_([^%s]+)_C")
      local UID = str:match("C_%d+")
      pal_list[#pal_list+1] = { 
                        ["UID"] = UID,
                        ["Inst"] = ActorInstance, 
                        ["Name"] = name, 
                        ["Location"] = nil
                      }
	  print("[TestLua] New Worker has been found." .. " : " .. tostring(name))
    end

  else 
    print("[TestLua] No AI Controllers found.")
  end 
  
  print("[TestLua] Worker List has been created." .. " : " .. tostring(#pal_list))

end
```

그리고 이 함수를 실제로 게임 내에서 실행 시키기 위해서 main.lua 파일에 아래 코드를 추가한다.   
이렇게 함으로써 F5를 누르면 게임 내에서`CreateWorkerList` 함수를 실행시킬 것이다.   

```lua 
RegisterKeyBind(Key.F5, { ModifierKey.CONTROL }, function()

  print("[TestLua] Key F5 pressed \n")

  if #pal_list == 0 then 
    print("[TestLua] CreateWorkerList ")
    CreateWorkerList()  
  end 

end)
```

게임 내에서 실행시켜보자.  

![Pasted image 20240324235104](https://github.com/votus777/votus777/assets/51744036/1e96b6ca-62b9-4c9b-a20b-303d7b5b3052)

일꾼들의 이름이 잘 나오는 것을 알 수 있다. 그런데 문제가 있다.   
분명 현재 내가 위치해 있는 베이스 캠프의 Pal들은 6마리여야 하는데 테이블에는 28마리가 있다고 나온다. 왜일까?   

여기에 두 가지 이유가 있다.   
첫 번째로 내가 만든 베이스캠프는 사실 3군데가 있다. 여기는 농장 용도로 만든 곳이고   
물품 생산, 채굴 등의 용도로 두 곳이 더 있다.   
두 번째로 PalWorld의 게임 시스템 때문인데, 내가 베이스 캠프를 떠나 있더라도 Pal들을 내가 시킨 일들을 수행하고 있어야 한다.  
그렇기 때문에 멀리 떨어져 있으면 사라지는 `MonsterAIController_Wild` 와 달리 `MonsterAIController_BaseCamp`는 항상 살아있는 것이다.  
그래서 내가 베이스 캠프에 배치한 모든 Pal들이 튀어나온 것이다.   

지금은 내가 위치한 곳의 데이터만을 보고 싶으므로 내가 현재 위치한 Basecamp에서 일하는 Pal들을 가져와 본다.  
Basecamp_ID를 이용하는 방법도 있을 것 같지만 여기서는 어차피 Basecamp가 서로 멀리 떨어져 있으니 플레이어 거리 기반으로 한다.   

```lua
function CreateNearbyWorkerList()
  local FirstPlayerController = UEHelpers:GetPlayerController()
  local player_loc = FirstPlayerController.Pawn:K2_GetActorLocation()

  local ActorInstances = FindAllOf("BP_MonsterAIController_BaseCamp_C")

  if ActorInstances and #ActorInstances > 0 then
    print("[TestLua] We have AI Controllers! \n")

    for Index, ActorInstance in pairs(ActorInstances) do
      local pal_loc = ActorInstance.Pawn:K2_GetActorLocation()
      local distance = math.sqrt((pal_loc.X - player_loc.X)^2 + (pal_loc.Y - player_loc.Y)^2)

      if distance < 10000 then 
        local str = tostring(ActorInstance.Pawn:GetFullName())
        local name = str:match("BP_([^%s]+)_C")
        local UID = str:match("C_%d+")
        pal_list[#pal_list+1] = { 
                                  ["UID"] = UID,
                                  ["Inst"] = ActorInstance, 
                                  ["Name"] = name, 
                                  ["Location"] = nil
                                }
        print("[TestLua] New Worker has been found." .. " : " .. tostring(name))
      end
      
    end

    print("[TestLua] Worker List has been created." .. " : " .. tostring(#pal_list))

  else 
    print("[TestLua] No AI Controllers found.")
  end 
end
```

아까 F5를 누르는 코드에 함수를 변경하고,   
게임에서 `Ctrl + R`을 눌러 모드를 새로고침 한다. 그 다음 다시 F5를 눌러서 Consonle 창에서 결과를 확인한다.    

![Pasted image 20240325000123](https://github.com/votus777/votus777/assets/51744036/a19885bb-c260-4e3c-bd40-9db728caff6c)


이제 베이스 캠프에서 일하는 Pal들의 이름과 위치 좌표를 가져올 수 있으니    
다음 포스팅에서는 이를 csv파일로 저장할 수 있도록 해본다.   

___
#### Reference 

[UE4SS Documentation](https://docs.ue4ss.com/dev/lua-api.html)  
[PalWorld Modding Wiki](https://pwmodding.wiki/docs/lua-modding/ue4ss-functions)  
[Unreal Engine 5 Docs](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Engine/GameFramework/)  
