---
title: '선형성(Linearity)'

categories:
- Study

image: 10.jpg

tags:
- AI
- 인공지능
- 회귀
- 선형성

---


## 선형성(Linearity)


선형성을 가지고 있다는 것은
꼭 그것이 직선이 아니더라도 직선의 성질을 가지고 있다고 이해된다.  
단순히 어떠한 패턴을 가지고 있다는 것을 의미하지 않는다!!  

여기서 직선의 성질이란   
Homogeneity(동차성, 비례성)과 Additivity(가산성)이 성립된다는 것을 의미한다.


>(a는 실수)  ay=f(ax)=af(x) --> Homogeneity  

>y1+y2=f(x1+x2)=f(x1)+f(x2) --> Additivity : 중첩의 원리( superposition principle ) 

y = 2x + 1 를 보면 a = 4, b =5 일때  
f(a) + f(b) = f(4) + f(5) = 9 + 11 = 20 이지만 f(a+b) = f(4+5) = f(9) = 19가 나온다
→ additivity 실종

또한 f(a) * 10 = f(a*10) 일까?  
f(4) * 10 = 9 * 10 = 90인데 , f(4*10) = f(40) = 81 , 역시 다르다
→ homogeneity 실종

```
사족) 그래도 어떤 패턴을 가지고 있으니 선형성을 가지고 있는게 아니냐고 하면, 그건 맞다
미분하면 똑같이 y' = 2 가 나온다. 즉 y의 변화량과 x의 변화량에는 선형성이 있다.
```
그래서 선형성을 만족하는 방정식이 있다면
이는 곧 '예측 가능성'을 내포하고 있다는 것을 시사한다.  

1. 기저 방적식 여러개로 똑같이 모든 함수값을 표현할 수 있게 된다. 분해가 가능하다는 말.  
2. 또한 입력값을 중첩시킨다면, 결과값 또한 중첩되어 나올 수 있기에 결과가 예상이 가능하다

그렇기에 어떤 가정을 할 때에 보통은 이런 linear model이라 가정해서 쉽게 풀어나간다.

y = ax + b 처럼 비선형성을 가지게 되면 직관적이지 않아서
결과값을 예측하기 어려워지고 너무 복잡해지니까.

(다만 모든 비선형 방정식이 결과값이 예측이 불가능한 것은 아님,  
y = x ^ 2처럼 단순한 모델은 실제 변수만 대입해 보면 되니까,  
하지만 feature가 더 늘어난다면? 답도 없다)

그런데 이제는 머신러닝을 통해서 y = wx + b 에 접근할 수 있게 되었다.  

책 안이 아니라 실제 자연에 존재하는 비선형성 문제에 대해서도 다뤄보기 시작한 것이다.    

### 그런데 왜 linear regression이라 하는걸까? 앞서 말했듯이 y = wx + b 는 linear하지 않은데 말이다 


---
`reference`

- [https://sdolnote.tistory.com/entry/Linearity](https://sdolnote.tistory.com/entry/Linearity)  
- [https://groups.google.com/g/han.sci.math/c/pjasJZp8j38?pli=1](https://groups.google.com/g/han.sci.math/c/pjasJZp8j38?pli=1)  
- https://brunch.co.kr/@gimmesilver/18
