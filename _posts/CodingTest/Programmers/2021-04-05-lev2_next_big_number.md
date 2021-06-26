---
layout: post
title: ⭐⭐ 다음 큰 숫자
categories: Programmers
message: 프로그래머스 / 연습문제 / 다음 큰 숫자
reference_url: https://programmers.co.kr/learn/courses/30/lessons/12911

---



* ## 소스 코드

```python
def solution(n):
    nCnt = format(n, 'b').count('1')

    while True:
        n += 1
        resultCnt = format(n, 'b').count('1')
        if nCnt == resultCnt: return n
```

​     

* ## 풀이

  ​    

`nCnt`  기존 매개변수의 이진수의 '1' 의 카운트갯수

`resultCnt`  매 반복마다 증감시킨 이진수의 '1'의 카운트갯수

`nCnt`와 `resultCnt`와 비교해서 답을찾았다.    

​     

- ## 기타...

```python
def solution(inp):
    inp = format(inp,'b')
    if inp.count('1') == len(inp) or inp.count('1') == 1:       #1111, or 1000               
        return int(inp,2) + int(2**(len(inp)-1))
    elif inp[inp.find('0'):].find('1') == -1 : 
        #1100, 11100000  0을찾고 0이시작  되는인덱스에서 마지막인덱스까지 1이없다면
        return int('10'+inp[1:][::-1],2)                        
        #자릿수를 증가해야되기때문에 10에 인덱스1 을 뺀 나머지를 역으로붙여줌
    else:
        a = len(inp[inp.rfind('01') + 2:])  # 1011 11101  01을찾고 그두자리를 바꿔줌
        plus = (10 ** a) * 9
        inp = str(int(inp) + plus)
        le = len(inp) - (len(str(plus)) - 1)
        inp = inp[:le] + inp[le:][::-1]
        return int(inp, 2)
```

_이전에 짜놨던코드 ... 위코드보다 복잡하지만 연산속도는 더빠르다..(맨위 코드는 `1000조` 정도의 연산부터 아주 약간의 연산시간이 필요했다 . 0.03초/// 해당코드는 연산속도가없는것같다)_

_테스트케이스로는 모두통과한걸로 나오고 정확성에서는 살짝 의문이든다._

