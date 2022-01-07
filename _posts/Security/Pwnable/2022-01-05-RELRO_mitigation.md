---
layout: post
title: RELRO 기법이 걸려있을때?
categories: Pwnable
message: 
---

## RELRO?

RELRO(RELocation Read Only)는 쓰기 권한이 불필요한 영역에 쓰기 권한을 제거하여 메모리를 보호하는 기법이다.

이중 범위에따라 __Partial RELRO__ 와 __Full RELRO__ 가있다.

* ### Partial RELRO

Partial RELRO 일 경우 __.init_array__ 와 __.fini_array__ 에대한 쓰기 권한이 제거되어있다.

> GOT overwrite 를 활용하자!

* ### Full RELRO

got에는 쓰기 권한이 제거 되어있고 data와 bss에 쓰기 권한이 있다.

> 라이브러리 함수인 hook overwrite 를 활용하자! 
>
> e.g) malloac hook, free hook