---
title: 'ε-greedy Build'

categories:
- study

image: 24.jpg

tags:
- algorithm 

---

< Epsilon-Greedy Build  >

> ε (epsilon) : 수학적으로 아주 작은 임의의 양수를 의미 통계학에서는 오차범위, 물리학에서는 유전상수 등으로 다양하게 쓰인다.  

특히 ML 중 Reinforcement Learning 분야에서는 Epsilon-greedy algorithm 혹은 Epsilon-greedy action selection 등에 나타난다.  

강화학습은 현재 상태(state)에서 관찰을 할 수 있는 행동(action) 중 가장 큰 보상(reward)을   
가져다 주는 행동이 무엇인지 학습하는 것이다.    

`greedy algorithm`은 당장의 최대 보상을 쫓아가는 방식을 취한다.   

하지만 실제로는 reward는 즉각적인 보상으로 나오지 않는 경우가 많은데(지연된 보상),   
이런 경우 지금 눈 앞에 놓여있는 보상만을 따라가는 것이 최선이 아니게 될 수 있다.     
즉, 이 보상을 덥석 무는 것이 아니라 조금 더 심사숙고 해서 다른 더 좋은 보상이 나올 수 있는지 탐색하는 과정이 추가될 수 있다.  

이것이 **Exploitation** VS **Exploration** 의 Trade Off 관계가 된다.   
**Exploit**는 현재 상태에서 최대 보상을 주는 행동을 그대로 선택하는 것이고      
**Explore**는 현재 상태에서의 최대 보상을 선택하는 대신, 적은 보상을 얻더라도 새로운 시도를 해보는 것이다.     
쉽게 말해 도박이다. 이 trade-off 관계는 Multi-armed Bandit problem이 잘 보여준다.     
역시 도박 아니랄까 슬롯머신을 당기는 문제다.  

비유를 하자면 소개팅을 받기로 했는데 웬일인지 동시에 서로 다른 사람에게서 3명을 소개 받게 되었다.   
A,B,C 이렇게 있는데 일단 프사를 보니 A가 가장 괜찮아 보인다.   
그래서 바로 A를 선택하고 소개팅에 나가는 것이 Exploit이다.  

그렇게 기대감을 잔뜩 안고 카페에 나갔는데 얼굴만 반반하지 행동하는 것과 하는 말이 완전 개차반이다.   
즉, 기대할 수 있는 보상은 커보였으나 전체 보상은 오히려 기대값 이하가 된 것이다. 충분한 Explore가 따라주지 않았기에 생긴 낭패다.   
반대로 B를 선택 했을때, 얼굴은 평범하지만 매너 있고 상대를 즐겁게 할 줄 아는 나이스한 사람이었다면 어땠을까.   
보상에 대한 기대값은 적었지만 실제 전체 보상은 보다 커졌을 것이다.     
하지만 그렇다고 A,B,C 모두 만나러 explore 하는 것은 굉장히 많은 시간, 물질적 비용이 발생할 것이다.   
또한 항상 더 큰 보상이 있다는 확신도 없다.     
이런 상황에서 어떻게 행동할 것인지가 이 trade-off 관계의 핵심이 된다.  
(Multi-armed Bandit problem에서는 소개팅 한 번으로 끝나는 것이 아니라   
여러번 이루어진다는 상황을 가정하는 등 복잡한 조건들이 추가된다).   

이를 풀어내기 위해 여러 알고리즘이 연구되었다.  
`Markov Decision Process (MDP)`, `Explore-First Algorithm`, `Gittins index`, `upper confidence bound (UCB)`, `Thompson Sampling`   
등이 있다고 한다.   

가장 직관적이면서 심플하고 지금 프로젝트에 쓰이고 있는 `ε -greedy algorithm` 또한 이 문제를 해결하기 위한 것이라고 한다.  

기존의 `greedy algorithm`을 발전시킨 `ε -greedy algorithm`은 1-ε 일 때와 ε 일 때 서로 다른 선택을 하도록 만드는 것이다.   
그리고 ε는 앞서 말했듯이 임의의 작은 양수이고, 확률로서 움직인다. 즉 랜덤으로 explore과 exploit를 수행한다.   
동전 던지기로 소개팅에 나가는 것과 같아서 항상 좋은 결과를 보장해주지는 않지만,  
50 % 확률로 현재 최대 보상을 주는 선택지만 고르거나, 또는 보상이 적더라도 다른 시도를 해보게 되는 것이다.      

아래 코드와 같이 간단하게 구조를 만들어 볼 수 있다.  

```python
p = 0.5
for ep in range(1, num_epochs): 
        eps = np.random.rand() 
        if eps < p:
            output1 = 1
            update()
        else:
            output2 = 2
            update()
```

Exploration 와 Exploitation의 trade-off 관계를 확률로 조절함으로써 임의성을 부여하고   
강화학습에 쓰이는 NN의 경우 초반에 많은 탐색을 시도하게 된다.    
 
다만 ε 을 상수로서 고정하게 되면 학습 후반부에서도 여전히 삽질을 많이 하고 있을 가능성이 커지고,   
최적화된 결과에 다가가지 못할 수 있다.   
  
그러나 이는 모델링 단계에서 ε을 적응시켜서 계속 업데이트하는 방안이 있다고 본다(scheduling).   
그리고 어디까지나 random에 기반한 알고리즘이기에 놓치거나, 학습을 많이 하지 못하는 케이스가 생길 수 있다.  
이것에 대해 다른 알고리즘이 계속 연구되고 있다고 한다.
  
  
  

___
< Reference >

[](https://yjjo.tistory.com/41)[https://yjjo.tistory.com/41](https://yjjo.tistory.com/41)  
[](http://sanghyukchun.github.io/96/)[http://sanghyukchun.github.io/96/](http://sanghyukchun.github.io/96/)  
[](https://codetorial.net/articles/cartpole/epsilon_greedy_policy.html)[https://codetorial.net/articles/cartpole/epsilon_greedy_policy.html](https://codetorial.net/articles/cartpole/epsilon_greedy_policy.html)  
[](https://yjjo.tistory.com/41)[https://yjjo.tistory.com/41](https://yjjo.tistory.com/41)  
[](http://tcpschool.com/deep2018/deep2018_machine_reinforcement)[http://tcpschool.com/deep2018/deep2018_machine_reinforcement](http://tcpschool.com/deep2018/deep2018_machine_reinforcement)  
[](http://sanghyukchun.github.io/96/)[http://sanghyukchun.github.io/96/](http://sanghyukchun.github.io/96/) # 설명이 굉장히 좋은 블로그  
