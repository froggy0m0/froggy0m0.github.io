---
layout: post
title: 메모리 보호기법
categories: Pwnable
message: 메모리 보호기법
---

## RELRO

```bash
$ gcc -z relro			# Partial RELRO 
$ gcc -z relro -z now		 # Full RELRO
$ gcc -z norelro 			# No RELRO
```

### CANARY

```bash
$ gcc -fstack-protector		# CANARY보호 설정
$ gcc -fno-stack-protector	# CANARY보호 해제
```

## NX

```bash
$ gcc -z exestack
```

## PIE

```bash
$ gcc -no-pie		# PIE 해제
$ gcc -fpie			#.text 랜덤
$ gcc -fpie -pie	# PIE 설정
```

## ASLR

```bash
##ASLR 확인
$ cat /proc/sys/kernel/randomize_va_space
2

##ASLR 변경
$ cat 0 > /proc/sys/kernel/randomize_va_space
```

| Value | comment                        |
| ----- | ------------------------------ |
| 0     | ASLR 없음                      |
| 1     | stack, library 영역 적용       |
| 2     | stack, library, heap 영역 적용 |

## etc..

32bit Compile

```bash
# gcc-multilib 설치이후
$ gcc -m32		#32bit
$ gcc -m64		#64bit
```

```bash
-mpreferred-stack-boundary=2
```

