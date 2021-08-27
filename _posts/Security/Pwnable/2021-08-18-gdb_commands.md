---
layout: post
title: 자주 쓰는 디버거 명령어 
categories: Pwnable
message: 
---

| 명령어(축약)         | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| start                | 진입점에 중단점을 설정하고, 실행                             |
| quit(q)              | 종료                                                         |
| break(b)             | break 중단점 설정 ex) b*main                                 |
| continue(c)          | 계속 실행                                                    |
| disassemble(disas)   | disassemble 결과 출력                                        |
| run(r)               | 해당 프로그램을 처음부터 실행                                |
| step into(si)        | 명령어 실행(해당 함수 내부 까지 자세히)                      |
| next instruction(ni) | 명령어 실행(해당 함수까지 들어가지 않음)                     |
| info(i)              |                                                              |
| kill(k)              |                                                              |
| pdisas(pd)           |                                                              |
| u, nearpc, pd        | 디스어셈블 결과 가독성 좋게 출력                             |
| x                    | 메모리 조회                                                  |
| context              | 레지스터, 코드, 스택 백트레이스의 상태 출력                  |
| telescope(tele)      | 메모리 조회, 메모리값이 포인터잂 경우 재귀적으로 따라가며 모든 메모리값 출력 |
| vmmap                | 메모리 레이아웃 출력                                         |





