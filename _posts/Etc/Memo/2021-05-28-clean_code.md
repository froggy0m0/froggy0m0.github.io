---
layout: post
title: Clean Code
categories: Memo
message: --
---

- 1. camel case      : Pascal Case

  2. 의도가 드러나는 이름 

  3. 정확한 의미를 주는것도 좋지만 불필요한 네이밍이라면 빼는게좋다

  4. - typedef struct Bug{

     - - int * bugHead;  -> int * head
       - int * bugBody;  -> int * body
       - int * bugTail;    -> int * tail

- 1. 좋은이름의 판단기준들

  2. 1. 기억하기 쉬워야한다.

     2. - 주관적인 기준으로 범용적으로 사용되는 단어 ex)        hangeul과 같은단어 X

-  

- 1. 변수에 모든 의미를 충분히 담아라 (약어사용 X)

  2. - add -> address 
     - sal -> salary

-  

- 1. 키워드와 비슷한 이름은 허용하지 말것

-  

- 1. 중괄호는 다른 라인과 분리 되어있어야하며 라인을 같이 쓰면 안된다.

- if(..)

- { //단독으로

- //Do something

- }

-  

- 1. 변수는 소문자로시작, 클래스는 대문자로 시작하는 규칙

- 변수와 클래스는 명사, 메서드나 함수는 동사

- 단순하게 일회성(루프)으로 사용되는 변수들은 I, j, k 등으로 임시적으로 사용 해도 좋다

- 임시로 사용되는 변수는 temp를 사용!

- 인덱스는 index

- 단, 넓은 유효 범위를 지닌 객체들은 그에 마땅한 이름을 지어주자.

-  

-  