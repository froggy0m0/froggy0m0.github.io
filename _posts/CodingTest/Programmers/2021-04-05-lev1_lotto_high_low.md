---
layout: post_programmers
title: ⭐ 로또의 최고 순위와 최저 순위
categories: Programmers
message: 프로그래머스 / 코딩연습 / 2021 Dev-Matching 웹 백엔드 개발자(상반기) / 로또의 최고 순위와 최저 순위
reference_url: https://programmers.co.kr/learn/courses/30/lessons/77484

---

* ## 소스 코드

```python
def solution(lottos, win_nums):
    Low = len(set(lottos) & set(win_nums))
    High = Low + lottos.count(0)
    return [min(6,7-High),min(6,7-Low)]
```

​      

* ## 풀이    


`lottos` 배열과 `win_nums` 의 배열의 교집합의 len으로 최저 당첨 갯수를 구하고

최고 당첨 갯수 는 구해진 LOW와 `lottos`의 배열에서 알아볼 수 없는 수(0)의 갯수을 더한다(그 원소의 갯수가 당첨된 번호라고 가정) 