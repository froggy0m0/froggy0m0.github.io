---
layout: post
title: WSL을 사용한 Linux 설치
categories: Linux
message: 
reference_url: https://docs.microsoft.com/ko-kr/windows/wsl/install-win10
---

## WSL 설치

### Step 1

PowerShell로 Linux용 Windows 하위 시스템 옵션 기능을 설정합니다.

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-1.png">

### Step 2

WSL 2 요구 사항을 확인하기위해 powershell에서  __winver__ 를 입력하여 버전 및 빌드 번호확인

* x64 버전 __1903 이상__, __빌드 18362 이상__
* ARM64 __버전 2004 이상__, __빌드 19041 이상__

### Step 3

WSL 2 설치전 Virtual Machine 플랫폼 옵션 사용 설정

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-2.png">

### Step 4

WSL 2 기본 버전 설정

하단 명령어 입력후 재부팅

오류가 뜰경우 하단 Error1 참고

``` powershell
wsl --set-default-version 2
```

### Step 5

Microsoft Store에서 사용할 리눅스를 받아 설치한다.(Ubuntu, Kali Linux, SUSE, Debian 등..)



### 설치 완료

Ubuntu 18.04 LTS 설치이후 초기화면

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-6.png">

#### 설치이후 root 설정

```sh
$ sudo passwd root			#초기 root 암호 지정하기!
```



#### Error1

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-4.png">

arm 혹은 x64 프로세스 를 사용중이다면 하단 참고 url 4단계에 제공하는 패키지를 설치합니다.

https://docs.microsoft.com/en-us/windows/wsl/install-manual

#### Error 2
1. Step 4 진행이후 재부팅을 하지않았다면 재부팅 이후 진행
	2-1. 'Windows 기능 켜기/끄기' 에서 'Linux용 Windows 하위 시스템' 체크가 안되있다면 체크 이후 재부팅 이후 진행
	2-2.  'Windows 기능 켜기/끄기' 에서 'Linux용 Windows 하위 시스템' 체크가 되어있다면 체크해제하고 재부팅 이후 체크하고 재부팅후 진행

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-3.png">

#### Error 3

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-5.png">

바이오스 설정에서 가상화기술을 Enable로 설정해준다.



## WSL 명령어들..

```powershell
> wslconfig.exe /l					#wsl 조회
> wslconfig.exe /u Ubuntu-18.04		  #wsl 삭제
> wslconfig.exe /l					#wsl 재조회
```



#### ~~네트워크 재설치?~~

~~프로그램 및 기능 -> Windows 기능 켜기/끄기 -> Linux용 Windows 하위 시스템 재설치~~

~~장치관리자에서 Hyper -v ~~ ethernet 제거 후 재설치~~

