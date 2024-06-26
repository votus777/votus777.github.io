---
title: 'AWS에 관하여'

categories:
- Study

image: 3.jpg

tags:
- AI
- Big Data
- AWS 
- MLops 

---

AWS는 IT 인프라 구축에 필요한 서비스들을 API를 통해 제공한다.  
파이썬의 모듈화처럼 각 서비스들을 세부화시켜서 종류가 굉장히 많고,또한 복잡한 이름들을 가지고 있다.
이를 한번 간략하게 총정리 해본다.


## 1. Data Storage  

- **Amazon S3** : Simple Storage Service. 사용한 용량만큼 월마다 비용을 지불한다. 
  Key-Value 형태로 되어있고 큰 파일또한 저장 가능하며 확장성 또한 좋다. API request를 이용할 수 있다.  
- **Amazon EBS** : Elastic Block Store. S3와 다르게 파일 단위가 아닌 블럭 단위로 접근이 가능하다. 
  EC2와 연계되어 사용할 수 있도록 설계. 
- **Amazon EFS** : Elastic File System. EC2 instance에 연결하여 같은 file system을 공유할 수 있도록 한다.   
- **Amazon Glacier** : 아카이브성 데이터(엑세스는 잘 하지 않지만 많은 용량의 장기간 저장 필요)를 저장할 수 있음. 백업 전용 스토리지. 

S3가 고전적인 웹하드처럼 인터넷 연결로 어디서든지 파일을 저장하고 불러올 수 있는 기능을 제공한다면, 
EBS는 EC2 위에서 안정적이고 원활한 컴퓨팅 환경을 조성할 수 있도록 일종의 가상 드라이브를 만들어준다.따라서 독립형 스토리지가 아니다. 
스냅샷을 통한 시점 백업 가능. 하나의 EC2와 직접 연결되어 훨씬 빠른 EC2 Instance Store도 존재하지만 사용 중에만 유지 된다는 차이점이 있다.
EFS는 같은 네트워크 안에서 많은 사용자가 동시에 접근할 수 있는 스토리지이다. 여러 EC2를 연결하거나, 사내 네트워크에 연결할 수도 있다. NAS. 
Glacier는 정말 괴악한 서비스인데 저장은 간편하지만 나중에 이것을 읽는데만 해도 당장 볼 수가 없으며 비용을 지불해야 한다. 정말 저장만 하는 용도.

## 2. Database   
  
- **Amazon RDS** : SQL DB, 정형화된 schema가 존재할 경우 사용  
- **DynamoDB** : Severless, NoSQL. 비관계형 데이터 완전 관리형(별도의 EC2 인스턴스가 필요없음). 즉각적인 데이터 저장과 쿼리에 좋음 (Low latency)    
- **Amazon Redshift** : DB 웨어하우스. 페타바이트 급의 여러 Data storage & source를 넘나들면서 데이터를 가져올 경우 사용.  
- **Amazon Aurora** : 엔터프라이즈급 관계형 DB. Redshift에 auto scaling 기능 추가.    
- **Document DB** : NoSQL, MongoDB query API. JSON docs. 문서 기반 데이터베이스.   

EC2에 DB를 구축하는 것보다 AWS를 이용하는 것이 효과적이긴 하지만 
DB의 경우 24시간 돌아가야 하므로 t2.nano 인스턴스에 MySQL을 설치하는게 저렴할 수도 있다. 즉 AWS DB는 기업 사용자용이다.
Amazon DynamoDB의 경우 IAM을 통한 엑세스 제어와 서로 다른 리전에서 같은 테이블을 볼 수 있는 글로벌 옵션이 지원된다. 
JSON 문서를 바로 저장할 수도 있다. 시간 당이 아니라 사용량에 따라 비용 지불. 사용량에 따라 알아서 규모를 확장 및 축소한다.  

## 3. Computing Service

- **EC2** : IaaS. VM in Cloud Server. 인스턴스 기반.
- **Amazon Light Sail** : Light & Cheap computer. 
- **AWS Lambda** : Serverless Computing. 함수 기반. 
- **Amazon ECS** : 컨테이너 기반 컴퓨팅을 지원한다. 이는 하나의 인스턴스로서 관리된다.  
- **Amazon Fargate** : Docker img를 업로드 하면 container를 지속해서 활성화 시켜줌.
- **Amazon App Runner** : Docker img를 업로드 하면 request가 올 때만 활성화 시켜줌.
- **AWS Elastic Beanstalk** : PaaS. 웹 어플리케이션의 배포 및 관리 서비스. DB, DNS 등 다른 서비스와 쉽게 연결 가능. 

ECS, EKS(Elastic Kubernetes Service),ECR(Elastic Container Registry), Fargate는 컨테이너 형식으로 움직인다. 
AWS Fargate는 관리 오버헤드를 줄이면서 더 많은 제어 원한을 제공하는 옵션을 지원한다. 서버, 클러스터 관리.  
AWS Lambda는 이벤트가 발생하면 코드를 실행하는 앱 엔진으로 켜 놓은 시간이 아닌 실제 실행 시간과 용량을 기준으로 과금된다. 


|　　일반　　|　　컴퓨팅 최적화　　|　　메모리 최적화　　|　　스토리지 최적화　　|　　가속화 컴퓨팅　　|  
|:---:|:---:|:---:|:---:|:---:|  
|　　범용　　|　　고성능　　|　　In-memory DB　　|　　분산 파일 시스템　　|　　Machine Learning　　|  
| a1,m4,m5,t2,t3 | c4,c5 | r4,r5,x1,z1 | d2,h1,i3 | f1,g3,g4,p2,p3 |    

  
## 4. Networking Ingress & Load Balancing 

- **Amazon ELB** : Elastic Load Balancer. 서버 분산 처리. Low latency인 상태에서 지속 구동.   
- **Amazon Cloud Front** : CDN (Content Delivery Network) Service. User와 가까운 곳에서 request를 받고(low latency), 자주 받는 request를 Cache로 만들어 
효율적인 서비스를 할 수 있도록 해줌. 
- **AWS App mesh** : Manage traffic ingress and flow. 세분화된 서비스와 원활하게 통신할 수 있도록 지원. 
- **API Gateway** : Serverless endpoint. request 받을 때만 cost 지불. 
- **AWS AppSync** : Serverless, GraphQL API. 백엔드 API 개발. 


서비스로 들어오는 연결을 처리하는 역할인 ELB는 
CLB(Classic Load Balancer),ALB(Application Load Balancer),NLB(Network Load Balancer)로 나뉜다.  
CloudFront는 세계 어디서나 빠른 속도로 파일을 제공하도록 최적화 해준다. 서명 URL이나 서명 쿠키를 이용해 인증된 사용자에게만 콘텐츠를
제공하는 유로 콘텐츠 서비스도 가능하다. 정적 콘텐츠에 이용할 수 있다. 