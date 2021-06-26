---
layout: post
title: ⭐⭐ 기능개발
categories: Programmers
message: 프로그래머스 / 코딩연습 / 스택/큐 / 기능개발
reference_url: https://programmers.co.kr/learn/courses/30/lessons/42586

---



* ## 소스 코드

```python
def solution(progresses, speeds):
    progresses = list(reversed(progresses))
    speeds = list(reversed(speeds))
    result = list() 
    while(progresses):
        count = 0
        for i in range(len(progresses)):
            progresses[i] += speeds[i]
        while(progresses and progresses[-1] >= 100):
            progresses.pop()
            count+=1
        if count != 0:
            result.append(count)
    return result
```

​     

* ## 풀이 

  ​     

`while(progresses):`

 progresses리스트가 없을때까지 while문이 반복된다.    

​     



`for i in range(len(progresses)):`

for 문을 통해서 모든 작업진도의 진행도를 올려줬다. (progress += speed)   

​       

`while(progresses and progresses[-1] >= 100):` 

이후 pop을통해 진행도가 100이상인 첫요소를 while문을 통해 계속 빼내준다     

​      

while문을 빠져나오고 count값을 반환될 result함수에 append 해주고

progress의 요소가 없을때 까지 반복하고 result값을 반환해준다

​     

* ## 아쉬운 부분

_흠.... 모든 테스트가 통과하긴하지만 무의미한 연산 (100이상이여도 빠져나가지 못한 요소들이 연산되는 부분)에 아쉬움이있다..._

