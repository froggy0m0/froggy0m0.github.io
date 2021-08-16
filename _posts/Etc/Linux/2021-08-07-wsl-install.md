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

WSL 2 요구 사항 __winver__ 를사용하여 버전 및 빌드 번호확인

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

오류가 뜰경우 재부팅한다.

``` powershell
wsl --set-default-version 2
```

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-3.png">

### 설치 완료

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-6.png">

### Step 5

Microsoft Store에서 사용할 리눅스를 받아 설치한다.

### Error 1

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-4.png">

arm 혹은 x64 프로세스 를 사용중이다면 하단 참고 url 4단계에 제공하는 패키지를 설치합니다.

Error 2

<img src="https://Froggy0m0.github.io/assets/img/Linux/2021-08-07-wsl-install-5.png">

바이오스에서 가상화기술을 Enable로 설정해준다.



