---
title: MongoDB + Tableau 연결
description: Tableau에 MongoDB를 live로 연결하는 방법
categories: [Tips, Tableau]
date: 2022-02-12
tags: [Data Visualization, mongodb, tableau  ]


---

# Tableau 
- Interactive data visualization software
- Focused on BI (Business Intelligence)
- Drag & Drop, No coding skill 
- Support data preperation tools 
- Free one-year Tableau licenses to students 

데이터 시각화 및 분석을 위해 **Tableau**를 사용해보았다. 
데이터는 JSON, CSV, EXCEL 형식의 파일을 직접 업로드 할 수도 있고 
서버에 연결해서 데이터를 가져올 수도 있다. 

나는 mongoDB에 데이터를 넣어놓았기 때문에 Tableau와 이를 연결해보기로 한다. 

1. 우선 이 둘을 연결하기 위한 **MongoDB BI Connector**가 필요하다. 
(다운로드 링크 : [link](https://www.mongodb.com/try/download/bi-connector))  
   다운 받은 후 설치를 진행한다.   
   mongoDB가 설치되어 있다면 자동으로 C드라이브의 mongoDB 폴더 내로 설치 경로가 잡힌다.    

2. 설치해놓은 Tableau Desktop을 실행하고 우측의 'To a server' -> 'more' -> 'MongoDB BI Connector'를 선택한다.    

3. mongodb는 현재 로컬 서버이기 때문에 server는 127.0.0.1, port는 3307이다. 

4. **하지만 하단의 로그인을 누르기 전에 mongosqld.exe를 실행해서 `sampling mongoDB for schema`가 끝난 이후여야 한다.**  
 	이를 하지 않으면 백날 눌러봤자 에러만 난다. 이걸 몰라서 한참 해맸다.   
	mongosqld.exe는 방금 전 BI connector가 설치된 파일 경로 안에 있다.   
	(C:\Program Files\MongoDB\Connector for BI\2.14\bin\mongosqld.exe)   
	이는 종료하지 말고 계속 켜두도록 한다. 
	
	구글링을 해보면서 MongoDB ODBC Driver와 System DSN를 편집하는 방법이 계속 나왔는데 이미 Tableau에서 connector 지원을 하는 이상 굳이 이렇게 번거롭게 하지 않아도 될 듯 싶다. 
	
5. 다시 서버 이름과 포트를 넣어주면 연결이 완료되고 mongoDB안의 데이터를 Tableau에서 볼 수 있게 된다. 
