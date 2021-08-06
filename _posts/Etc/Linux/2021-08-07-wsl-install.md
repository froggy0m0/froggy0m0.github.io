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



### Step 2

WSL 2 요구 사항 __winver__ 를사용하여 버전 및 빌드 번호확인

* x64 버전 __1903 이상__, __빌드 18362 이상__
* ARM64 __버전 2004 이상__, __빌드 19041 이상__

### Step 3

WSL 2 설치전 Virtual Machine 플랫폼 옵션 사용 설정

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Step 4

WSL 2 기본 버전 설정

오류가 뜰경우 재부팅한다.

arm 혹은 x64 프로세스 를 사용중이다면 하단 참고 url 4단계에 제공하는 패키지를 설치합니다.

```powershell
wsl --set-default-version 2
```