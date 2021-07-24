---
title: 'Data Slicing'

categories:
- study

image: 2.jpg

tags:
- AI
- Big Data

---

자꾸 까먹는 스킬들이 있어서 블로그에 몇 개를 정리해본다. 

# Slicing

- 연속적인 개체들에 (예: List. Tuple, string) 범위를 지정해 선택해서   
  객체들을 가져오는 방법 및 표기법을 의미

-  Sclicing을 하면 새로운 list 혹은 집합 객체를 생성하게 된다. 즉, 일부분을 복사해서 가져오는 것.


## 기본 형태
```

                                          +---+---+---+---+---+---+
                                             | P | y | t | h | o | n |
                                          +---+---+---+---+---+---+
              Slice position:  0   1   2   3   4   5   6     ->    p[5], p[0:1], p[0:2]
              Index position:   0   1   2   3   4   5       ->     'n',  ['P'], ['P','y']

  

         a[start : end : step] 
           
           Start : 슬라이싱을 시작할 위치
           end : 슬라이싱을 끝낼 위치로 end 자체는 포함하지 않는다
           step : stride라고도 하며 몇 개씩 끊어서 가져올지를 정한다. 
```

- 예제 1   
  `>>> a = ['a', 'b', 'c', 'd', 'e']`    
  `>>> a[ : : 2 ]`    
  `# 2칸씩 이동하면서 가져옵니다.`    
  `['a', 'c', 'e']`

- 예제 2  
  `>>> a = ['a', 'b', 'c', 'd', 'e']`    
  `>>> a[ : : -1 ]`    
  `# 전체를 거꾸로 가져옵니다.`    
  `['e', 'd', 'c', 'b', 'a']`

- 예제 3  
  `>>> a = ['a', 'b', 'c', 'd', 'e']`  
  `>>> a[ 3 : : -1 ]`  
  `['d', 'c', 'b', 'a']`

- 예제 4  
  `a[:]           # a copy of the whole array`

- 예제 5  
  `>>> p = ['P','y','t','h','o','n'] # Start over`  
  `>>> p[2:4] = ['s','p','a','m']`  
  `>>> p = ['P','y','s','p','a','m','o','n']`  
  `# 추가할 때에 항상 같은 길이일 필요는 없다`

- 예제 6  
  `>>> p = ['P','y','t','h','o','n']`  
  `>>> p[4:4] = ['x','y']`  
  `>>> p = ['P','y','t','h','x','y','o','n']`





`reference`
- https://stackoverflow.com/questions/509211/understanding-slice-notation
- https://twpower.github.io/119-python-list-slicing-examples
