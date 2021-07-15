---
title: 'AWS에 관하여'

categories:
- AWS
image: 5.jpg

tags:
- AI
- Big Data
- AWS 
- MLops 

---

AWS는 IT 인프라 구축에 필요한 서비스들을 API를 통해 제공한다.  
파이썬의 모듈화처럼 각 서비스들을 세부화시켜서 종류가 굉장히 많고,또한 복잡한 이름들을 가지고 있다.
이를 한번 간략하게 총정리 해본다.


##1. Data Storage  

- **Amazon S3** : Simple Storage Service. 사용한 용량만큼 월마다 비용을 지불한다. 
  Key-Value 형태로 되어있고 큰 파일또한 저장 가능하며 확장성 또한 좋다. API request를 이용할 수 있다.  
- **Amazon EBS** : Elastic Block Store. S3와 다르게 파일 단위가 아닌 블럭 단위로 접근이 가능하다. 
  EC2와 연계되어 사용할 수 있도록 설계. 
- **Amazon EFS** : Elastic File System. EC2 instance에 연결하여 같은 file system을 공유할 수 있도록 한다.   

S3가 고전적인 웹하드처럼 인터넷 연결로 어디서든지 파일을 저장하고 불러올 수 있는 기능을 제공한다면, 
EBS는 EC2 위에서 원활한 컴퓨팅 환경을 조성할 수 있도록 일종의 가상 드라이브를 만들어준다.따라서 독립형 스토리지가 아니다.  
EC2와 직접 연결되어 훨씬 빠른 EC2 Instance Store도 존재하지만 사용 중에만 유지 된다는 차이점이 있다.
EFS는 같은 네트워크 안에서 많은 사용자가 동시에 접근할 수 있는 스토리지이다. 여러 EC2를 연결하거나, 사내 네트워크에 연결할 수도 있다.   
