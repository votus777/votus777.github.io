---
title: How to Get Data from PalWorld? - Part 01
description: A guide on collecting data from PalWorld using Unreal Engine and UE4SS.
categories: [Project]
date: 2023-03-14
tags: [Data Collection, Unreal Engine, UE4SS]
---



### Introduction 
PalWorld는 2024년 1월 19일 얼리엑세스를 통해 출시되었으며  
2월 1일 기준 스팀 판매량 1200만장, 엑스박스 이용자 700만명,  
출시 한 달 뒤에는 스팀 1500만장, 엑스박스 1000만명으로 총합 2500만명을 넘었다.    
최근에 가장 큰 이슈가 되고 있는 게임이다.    

'Palworld'에서는 포켓몬스터의 포켓몬처럼 'Pal' 친구들을   
야생에서 붙잡아 자신의 캠프에서 노동(...)을 시켜 우리의 생존과 발전에 도움을 받을 수 있다.   
(실제로 전기 속성의 친구를 하루 종일 배터리 옆에 서있게 해서 '발전'하는 역할을 시킬 수 있다)

그런데 우리가 탐험을 하러 캠프를 나가있는 동안   
이 친구들이 농땡이를 피우지 않고 맡겨진 일을 제대로 하는건지 궁금하다.   
돌아올 때마다 창고에 쌓인 산출물을 통해 이 친구들이 무언가 일을 하고 있다고는 생각이 들지만   
과연 효율적으로 움직이며 아웃풋을 제대로 내고 있는지는 객관적인 데이터와 지표가 없으니 알 수 없다.  

구글, 링크드인 같은 테크 회사에서는 개발자의 업무 생산성을  
수행하는 작업에서의 **참여도, 속도, 품질, 안정성** 등을 기준으로 추적하며 지표를 만들어 측정한다. [Link](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity-bae)    
심회적으로는 Ease of Delivery, Time Loss, Change Failure Rate 처럼 복잡한 지표를 만들거나 정성적 지표와 혼합해서 측정하기도 한다.   

우리만의 지표를 선택하기 위해서는 구글의 'Goals, Signals, Metrics (GSM)' 프레임워크를 적용해 볼 수 있다.   

**1. 문제를 해결하고자 하는 목표 정의**    
**2.해당 목표를 달성했다는 것을 알려주는 신호 탐색**    
**3.적절한 지표를 선택**      

이런 데이터를 어떻게 찾고, 어떻게 기록할 수 있을까.   

save file은 우리가 게임을 계속할 때 필요한 최소한의 정보만을 담고 있을 뿐이고,  
log file은 게임 외부에서의 실행 과정 등을 나중에 오류가 생겼을 때 확인하기 위해 기록되는 것이다.   
보통 게임 개발 중 디버깅을 할 때 필요한 debugging 메뉴나 콘솔이 존재하지만   
보여주는 데이터은 매우 한정적이고 우리가 원하는 데이터를 보여준다는 보장도 없다.   

따라서 우리가 직접 일종의 센서를 만들어 본다.   
이 센서는 우리가 지정한 신호를 감지하며 매 초 혹은 특정 이벤트 발생 시에 데이터를 기록할 수 있다.   

그래서 우선 (1)우리가 게임을 컨트롤 할 수 있게 만들어 주는 기반 시스템인 UE4SS를 설치하고,   
여기에 (2)mod를 통해 우리가 원하는 동작을 코드로 실행시킬 것이다.   


___

### Appendix 

본문에서는 농담 삼아 업무 감시 시스템을 언급했지만,    
사실 내가 주목하는 'Palworld'의 키워드는 협업과 상호작용이다.   

로봇의 3요소는 움직임, 지능, 상호작용으로 이루어진다.   
움직임이 없으면 그것은 Software만으로 존재하는 가상의 Agent이고  
지능이 없으면 그것은 단지 복잡한 기계 장치이며  
상호작용이 없으면 업무 자동화 시스템일 뿐이다.   
이 세가지 요소가 모두 충족되어야 진정한 의미의 로봇이 될 수 있다.   

- **움직임 (Movement)**은 현대 자동차의 Boston Dynamic나 Tesla처럼  
Enigneering에 기술력이 있는 회사에서 발전되고 있다.   
- **지능 (Intelligence)**은 Deep Learning으로 급격한 발전을 이룬 AI 분야,   
특히 SW 기술력과 데이터, 컴퓨팅 자원이 풍부한 빅테크에서 연구되고 있다.   
- 그렇다면 **상호작용 (Interaction)**은 어디서 발전하고 있는가?   

나는 HRI (Human-Robot Interaction), HAI(Human-AI Interaction)는   
게임 분야에서 더욱 활발히 연구 되어야 한다고 생각한다.   

상호작용 개발, 측정, 평가가 모두 가능한 환경을 만들어 왔던 분야인데  
이러한 이점을 가장 잘 활용할 수 있어야 하지 않을까.   


___
#### Reference 

[UE4SS Documentation](https://docs.ue4ss.com/dev/lua-api.html)  
[PalWorld Modding Wiki](https://pwmodding.wiki/docs/lua-modding/ue4ss-functions)  
[Unreal Engine 5 Docs](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Engine/GameFramework/)  


