---
layout: post
title: gdb 명령어정리
categories: Pwnable
message: 
---

## gdb

### 기본 명령어

| 명령어(축약)         | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| start                | 진입점에 중단점을 설정하고, 실행                             |
| quit(q)              | 종료                                                         |
| break(b)             | break 중단점 설정 <br>e.g) b*main                            |
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
| gdb.attach(p)        |                                                              |
| set                  | 특정 gdb실행 중에서 특정 주소 값 변경 <br>e.g) set *7fffffff1234 = 0x10 |



### 메모리 권한 확인하기

```bash
pwndbg> shell cat /proc/`pidof test`/maps	##메모리 권한 확인하기

pwndbg> shell cat /proc/`pidof test`/maps | grep -i stack		##스택 메모리 rwx권한 확인 NX bit확인
7ffffffde000-7ffffffff000 rw-p 00000000 00:00 0                          [stack]
```



### 필요한 주소 찾기

```bash
$ readelf -a /lib/x86_64-linux-gnu/libc.so.6 | grep "system"	#system함수 offset 0x453a0
   225: 0000000000138910    70 FUNC    GLOBAL DEFAULT   13 svcerr_systemerr@@GLIBC_2.2.5
   584: 00000000000453a0    45 FUNC    GLOBAL DEFAULT   13 __libc_system@@GLIBC_PRIVATE
  1351: 00000000000453a0    45 FUNC    WEAK   DEFAULT   13 system@@GLIBC_2.2.5
  
  ...
  ...
  ...
  
pwndbg> info proc map
process 67
Mapped address spaces:

          Start Addr           End Addr       Size     Offset 			objfile
			0x000~				0x000~		0x000~~		0x00~~		/home/
			.
			.
			.
			.
			
			
pwndbg> p printf
$2 = {int (const char *, ...)} 0x7ffff7a46f70 <__printf>
pwndbg> find 0x7ffff79e2000,0x7ffff7bc9000, "/bin/sh"
0x7ffff7b95e1a
1 pattern found.
pwndbg> x/s 0x7ffff7b95e1a
0x7ffff7b95e1a: "/bin/sh"
pwndbg>
```

```bash
$ strings -tx /lib/x86_64-linux-gnu/libc-2.27.so | grep "/bin/sh"
2b3a22 /bin/sh
```