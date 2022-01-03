---
layout: post
title: 주소 찾기 (/bin/sh, plt, got)
categories: Pwnable
message: 

---

## Plt & Got 주소

```python
import pwn from *

p = process("./test")
e = ELF("./test")

print(hex(e.got['system']))
print(hex(e.plt['system']))
print(hex(e.symbols['system']))
```

### 메모리 권한 확인하기

```bash
> ps -aux | grep test		#test 프로세스의 PID
.
.
.

pwndbg> shell cat /proc/`pidof test`/maps	
##메모리 권한 확인하기
##e.g) shell cat /proc/494/maps

pwndbg> shell cat /proc/`pidof test`/maps | grep -i stack		
##스택 메모리 rwx권한 확인 NX bit확인
##e.g) shell cat /proc/494/maps | grep -i stack
7ffffffde000-7ffffffff000 rw-p 00000000 00:00 0                          [stack]
.
.

```



### 필요한 주소 찾기

```bash
readelf -a /lib/x86_64-linux-gnu/libc.so.6 | grep "system"	#system함수 offset 0x453a0
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

## glibc 에서 /bin/sh 주소값

```bash
$ strings -tx /lib/x86_64-linux-gnu/libc-2.27.so | grep "/bin/sh"
2b3a22 /bin/sh
```
