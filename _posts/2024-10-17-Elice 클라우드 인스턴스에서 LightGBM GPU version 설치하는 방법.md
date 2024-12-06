---

title: Elice 클라우드 인스턴스에서 LightGBM GPU version 설치하는 방법
description: LG Aimers 5기 해커톤 진행 중 제공 받은 주피터 노트북 환경에 GPU를 사용할 수 있는 LightGBM을 설치해봅니다. 
categories: [Cloud, GPU]
date: 2024-10-17
tags: [Cloud, GPU, LightGBM GPU]

---

**2024.10월 기준으로 작성된 내용입니다.**     


그냥 gpu 명시를 해주거나, 따로 gpu 버전을 설치 하면 되는 타 라이브러리와 다르게   
LightGBM은 GPU 버전 설치와 함꼐 환경 변수 설정 등의 추가적인 변경이 필요하다.   

게다가 직접적인 접근이 불가능한 외부 클라우드 인스턴스 환경에서는 설치가 훨씬 까다로워진다.   
여러 방법들을 구글링 해보고 섞어가며 시도하며 찾아낸 방법을 기록해 둔다.   

물론 얼마 지나지 않아 환경 업데이트 등으로 쓸모 없어지겠지만   
타 ML 모델의 GPU 사용에 필요한 자원과 의존성에 대한 힌트는 될 수 있을 것이다.  


```python
!echo "deb http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse" | sudo tee /etc/apt/sources.list
!echo "deb http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse" | sudo tee -a /etc/apt/sources.list
!echo "deb http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse" | sudo tee -a /etc/apt/sources.list
!echo "deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse" | sudo tee -a /etc/apt/sources.list
```


```python
!sudo apt-get update
```

```python
!sudo apt-get install cmake libboost-dev libboost-system-dev libboost-filesystem-dev -y
```

```python
!cmake --version # 여기서 cmake version 3.22.1이면 업데이트 필요 (3.28버전 이상) 
!sudo apt-get remove --purge cmake -y 
!wget https://github.com/Kitware/CMake/releases/download/v3.28.0/cmake-3.28.0-linux-x86_64.sh 
!chmod +x cmake-3.28.0-linux-x86_64.sh !sudo ./cmake-3.28.0-linux-x86_64.sh --skip-license --prefix=/usr/local 
!cmake --version # 버전 확인

```

```python
!cd LightGBM &&cd build && cmake -DUSE_GPU=1 .. && make -j4
```

```python
!pip install lightgbm \ --config-settings=cmake.define.USE_GPU=ON \ --config-settings=cmake.define.OpenCL_INCLUDE_DIR="/usr/local/cuda/include/" \ --config-settings=cmake.define.OpenCL_LIBRARY="/usr/local/cuda/lib64/libnvidia-opencl.so.1"

```

```python
!sudo mkdir -p /etc/OpenCL/vendors && echo "libnvidia-opencl.so.1" | sudo tee /etc/OpenCL/vendors/nvidia.icd
```

> 마지막 코드 실행 후 인스턴스 새로고침 필요!!



```python
lgb_model = LGBMClassifier(device='gpu')
```

