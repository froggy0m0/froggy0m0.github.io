---
layout: post
title: ⭐ 크레인 인형뽑기 게임
categories: Programmers
message: 프로그래머스 / 코딩연습 / 2019카카오 개발 겨울 인턴쉽 / 크레인 인형뽑기 게임
reference_url: https://programmers.co.kr/learn/courses/30/lessons/64061

---

​     

* ## 소스 코드

```python
def solution(board,moves):
    result = list() # 바구니 ?
    cnt = 0
    for j in moves:
        for i in range(len(board)):
            if board[i][j-1] != 0 :
                result.append(board[i][j-1])
                board[i][j-1] = 0
                break
        if len(result) >= 2 and result[-1] == result[-2]:
            result = result[:-2]
            cnt +=2

    return cnt
```

​    

* ## 풀이

  ​    