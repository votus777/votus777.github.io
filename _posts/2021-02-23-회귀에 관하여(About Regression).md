---
title: ' 회귀에 관하여 (About Regression) '

categories:
- Study 
  
image: regression.jpg 
  
tags:
- Regression
- 회귀

---

## 회귀에 관하여


대체 어디로 돌아간다는 건가?


>오차 : 모집단에서 얻은 회귀식을 통해 얻은 예측값과 실제값의 차이  
잔차 : 표본집단에서 얻은 회귀식을 통해 얻은 예측값과 실제값의 차이
  



보통은 모집단을 직접 다루는 일은 잘 없고 표본을 쓰기 때문에 잔차를 쓴다.

즉, 이런 오차와 잔차, 예측값과 실제값의 차이가 평균으로 회귀한다...고  
누가 써놨으나 그냥 이해하기 편하게 오차의 합이 최소가 되도록 한다고 보자

그러다보면 언젠가 평균에 가까워지겠지..뭐



>1777년 4월 30일(외우기 쉽다)에 태어난 가우스는 
>
>'만약 측정값의 평균을 실제값이라 본다면, 오차도 정규분포 따른다고 볼 수 있지 않겠음??'  
>이라고 주장하면서 가우스 오차함수인지 뭔지를 만들었는데 이게 정규분포와 연관이 있나보다.  
>
>어쨌든 오차가 실제값에 가까워질때 생기는 자연스런 잡음, 노이즈인지   
>아니면 내가 어딘가에 손을 잘못대서   
>혹은 고려하지 못한 점이 있어서 생기는 것인지 구분할 때   
>오차가 정규분포를 이루는지 살펴본다고 한다 카더라..
>
>잔차가 정규분포를 이룬다면 이 잔차들은 평균으로 회귀하는 속성을 지니게 된다.
>이것의 평균이 정규분포를 이루지 않는다면...그건 뭔가 잘못된 것이다   
>데이터가 이상하든 모델링이 이상하든...
>  
>(사실 정규분포 자체가 가우스가 오차를 연구하다 나온 것이다)
>
>
>5/20 
>그런데 또 이제보니 오차항의 확률 분포도 다양하다고 한다   
>https://brunch.co.kr/@gimmesilver/38



아무튼 인공신경망(ANN)을 실제로 쓸 수 있으려면 이 오차를 줄여서 Accuracy를 높여야 한다.

그런데 우리는 이와 반대로 정확하지 않음,   
즉 모델 성능의 나쁨을 나타내는 지표로 '손실함수'라는 것을 사용한다.     
왜냐하면 정확도를 지표로 삼게 되면   
미분 값이 대부분 0이 되어 매개변수를 갱신할 수 없기 때문이다.


만약 손실함수의 미분값이 음수이면   
그 가중치 매개 변수를 양의 방향으로 변화시켜 오차를 줄여나간다. 반대로도 가능하다.


그러나 어쩌다 0이 되버리면 움직이지 않게 되기에 갱신을 멈춘다.   
그래서 정확도를 기준으로 하는 것이 아니라 손실함수를 사용하는 것이다.

즉 회귀 방식은 손실함수를 이용해 오차를 평균 값으로 회귀시킨다고 볼 수 있다.
그렇게 해서 최적의 w 가중치를 구하고 예측 모델을 만들 수 있는 것이다. 

