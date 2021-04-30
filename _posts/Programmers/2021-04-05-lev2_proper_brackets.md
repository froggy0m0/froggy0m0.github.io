---
layout: post_programmers
title: ⭐⭐ 올바른 괄호
categories: Programmers
message: 프로그래머스 / 연습문제 / 올바른 괄호
reference_url: https://programmers.co.kr/learn/courses/30/lessons/12909

---

# 올바른 괄호

​       

​     

* ## 소스 코드

```python
def solution(s):
    count = 0
    for i in s:
        if count < 0: return False
        elif i == '(' : count += 1
        elif i == ')' : count -= 1
    return True if count == 0 else False
```

* ## 풀이 

괄호는 항상 여는괄호 `' ( '`  부터 나와야되고

올바른 괄호는 여는괄호와 닫는괄호가  짝을 이루는 것을 이용했다.

또한 닫는괄호가 먼저 나오는 경우 False를 리턴했다.

​    
