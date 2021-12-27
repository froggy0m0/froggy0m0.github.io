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
$ gcc -fno-stack-protector	# CANARY 설정
$ gcc -fstack-protector		# CANARY 해제
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
$ cat /proc/sys/kernel/randomize_va_space
2
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

