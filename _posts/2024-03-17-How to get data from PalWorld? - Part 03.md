---
title: How to Get Data from PalWorld? - Part 03
description: Continuing Part of the guide on collecting data from PalWorld using Unreal Engine and UE4SS.
categories: [Project]
date: 2023-03-17
tags: [Data Collection, Unreal Engine, UE4SS]
---


### Monitoring the Game 

이제 기반은 마련했지만 어떻게 시작해야 할 지 막막할 수 있다.   
어떤 함수를 써야 하며, 이를 어떻게 연결하거나 호출해야 할까??   

만약 생전 처음 해보는 스포츠, 예를 들어 미식 축구 같이   
룰이 복잡하고 축구처럼 당장 필드에 들어가서 뛴다고 어떻게 할 수 있는게 아닐 때  
우리가 할 수 있는 최선이 무엇일까?   

일단 그냥 다른 선수들이 어떻게 하는지 벤치에 앉아서 지켜보는 것이다.   
외부에서 지켜보는 것만으로도 작동 방식과 룰을 어느 정도 감을 잡을 수 있다.   

우리는 내부 개발자가 아니기 때문에 내부 동작 코드에 대한 정보를 알지 못한다.   
따라서 우리가 할 수 있는 최선은 외부에서 역으로 파고 들어가는 방법이다.   
여기에는 정해진 방법이 없기 때문에 각자의 센스와 직관대로 하게 된다.   
사실 우리가 지금 하는게 해킹이기도 하다.   

가장 직관적인 방법은 게임의 작동 방식을 지켜보며 어떤 함수가 쓰이고 어떻게 호출 되는지 보는 것이다.   
펠스피어의 작동 방식이 궁금하면, 게임에서 직접 이걸 던져봐서 어떤 코드가 호출되는지 보면 된다.   
위에서 언급한 UE4SS의 **Live View**가 바로 여기에 쓰인다.   

좋은 센스를 가졌다면 적은 시행착오로 필요한 동작을 척척 가져다 쓸 수 있고,   
그게 아니라면 일일히 뒤져가며 겨우겨우 하나 만들고 쉬러 가야 할 수도 있다.   
~~괜히 해킹이 천재의 영역인게 아니다~~  

### Basic UE4SS Functions

모니터링을 해보며 어떤 재료들이 있는지 확인을 해보았다면  
이제는 이 재료들을 가지고 이리저리 컨트롤 해불 수 있어야 한다.   

이렇게 컨트롤 할 수 있도록 UE4SS는 Lua API 형식으로   
미리 만들어진 함수를 가져올 수 있다.   

그런데 일단 들어가기 전에, Lua 에서 아래 둘은 같지만 표현 방식만 다를 뿐이다.   
간단한 코드는 그냥 아래에 적는 것이 편하지만   
코드가 길고 복잡해지면 따로 함수로 빼놓는 것이 덜 헷갈린다.   

```lua
RegisterHook("/idk/some:function", function(self)  
	--some complicated logic  
end)
```

```lua
local function complicatedFunction(self)  
	--some complicated logic  
end  
  
RegisterHook("/idk/some:function", complicatedFunction)
```



#### RegisterHook 

```lua
RegisterHook("/Script/Engine.PlayerController:ClientRestart", function (Context)  
	-- do something  
end)
```

특정 부분에 Hook을 걸어 전체 코드가 실행되고 있을 때, 
여기에 끼어들어서 특정 코드를 실행시키도록 한다. 

동작을 초기화 시킬 때 유용해서 자주 쓰이는 함수다. 
예시 코드처럼, ClientRestart에 hook을 걸어서 게임을 재시작 했을 때 무언가 하게 만들 수 있다. 

BP인 Function에 hook을 걸면 해당 함수가 실행된 후에 ResisterHook이 실행되고, 
non-BP Function (native)에 걸면 함수가 실행되기 전에 ResisterHook이 실행된다고 한다. 
그래서 직접 콘솔을 보다 보면 이 ResisterHook이 어떨 때는 동작하고 다른 때에는 잠잠한 상황을 볼 수가 있다. 

또한 Mod file을 직접 실행해보면 첫 실행에 모든 것이 불러와지는 것은 아닌 것을 알 수 있다. 
이를 고려하지 않고, 무작성 호출을 하다 보면 코드가 제대로 동작하지 않고 꼬여버리는 상황이 생긴다. 
그래서 안정성을 위해 ResistorHook을 사용할 수 있다. 


#### NotifyOnNewObject 

이것도 자주 쓰이는 함수다. 
특정 오브젝트가 생성 되었을 때 작동하는 함수이다. 
이후 Callback 함수를 통해 특정 동작을 오브젝트 생성 이후에 실행시킬 수 있다. 
예를 들어 상자가 새로 설치될 때 콘솔로 알림 메세지를 보낼 수 있다. 

조심해야 할 것은 이런 오브젝트가 게임에서 유일하지 않을 때이다. 
동일 이름을 가진 복수의 오브젝트가 계속해서 생성된다면 
이 코드는 반복해서 이 함수를 호출할 것이고, 곧 엄청난 메모리 낭비가 될 수 있다. 
따라서 그룹화를 시켜서 한 번만 작동하도록 하거나, 
아래 예시 코드처럼 `local not_hooked = true`이런 식으로 스위치를 걸어두는 것이 좋다. 

```lua
local not_hooked = true  
RegisterHook("/Script/Engine.PlayerController:ServerAcknowledgePossession", function(Context)  
	if not_hooked then  
		ExecuteWithDelay(5000,function()  
		NotifyOnNewObject("/Game/Pal/Blueprint/MapObject/BuildObject/BP_BuildObject_HeaterElectric_BP_BuildObject_HeaterElectric_C", function(Context)  
		print("Found Electric Heater ")  
		end)  
	end)  
not_hooked = false   
end)
```

또는 

```lua 
NotifyOnNewObject("/Script/Engine.PlayerController", function(PlayerController) PlayerController.InputPitchScale = -2.5 end) 
NotifyOnNewObject("/Script/Engine.PlayerController", function(PlayerController) PlayerController.InputRollScale = 1.0 end)
```

이렇게 반복해서 하는 대신에 

```lua 
NotifyOnNewObject("/Script/Engine.PlayerController", function(PlayerController) 
	PlayerController.InputPitchScale = -2.5 PlayerController.InputRollScale = 1.0 end)
```

이런 식으로 한 번에 묶는 것이 추천된다. 


### **StaticFindObject**

필드 내의 오브젝트가 아닌 시스템의 클래스 객체(Object)를 불러오고 싶을 때는 
`StaticFindObject`를 사용할 수 있다. 

에를 들어 `PalUtility`는 게임 내에서 가시적으로 나타나지는 않지만 
코드 상에서 유용하게 쓰일 수 있는 함수들을 제공하기 때문에 이를 호출할 수 있다.  

```lua
local PalUtil = StaticFindObject("/Script/Pal.Default__PalUtility")  
PalUtil:AwesomeFunction()
```

여기서 강조할 점은, 
필요할 때마다 `StaticFindObject("/Script/Pal.Default__PalUtility")`이런 식으로 
불러오는 것이 아니라 
예시 코드처럼 `local PalUtil` 같이 cache로 저장해두는 것이 훨씬 효율적이다. 
caching 하지 않는다면 호출할 때마다 UE에서 생성된 모든 UObject가 저장되는 전역 변수인 GUObjectArray에서 매번 찾아서 불러와야 하기 때문에 굉장히 비효율적이다. 

> Object와 Instance의 차이 - Object(개념)의 실체화 = Instance -> 따라서 메모리에도 존재


### FindFirstOf / FindObject

둘 다 찾는 기능이지만 차이가 있다. 
`FindFirstOf`는 Instance를 대상으로 한다. 디폴트 인스턴스는 제외. 
FindObject는 이름대로 GUObjectArray에서 Object를 찾아준다. 


### LoopAsync

루프 걸 때 사용한다

```lua
LoopAsync(number_of_ms_to_sleep, function()    
	--something
	return false -- Loops forever
	return true -- End loop
end)
```

### FName + FText

UE에서 string 객체를 사용할 때는 FName, FText라는 특수한 타입을 사용하는데 
이를 lua에서 string으로 받으려면 이런 식으로 한 번 감싸줘야 한다. 

```lua
local fname = some_fname.ToString()  
local ftext = some_ftext
```

그 밖에 더 많은 UE4SS가 제공하는 함수는 
https://docs.ue4ss.com/dev/lua-api.html 
여기를 참조할 수 있다. 



___
#### Reference 

[UE4SS Documentation](https://docs.ue4ss.com/dev/lua-api.html)  
[PalWorld Modding Wiki](https://pwmodding.wiki/docs/lua-modding/ue4ss-functions)  
[Unreal Engine 5 Docs](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Engine/GameFramework/)  
