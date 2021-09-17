---
layout: post
title: 각종 셋팅..
categories: Memo
message: 백업용..
---

## Windows 10 setting

* ### 드라이버 자동 설치 해제

* ### 윈도우 검색중 웹검색 제거하기

  레지스트리 편집기(regedit) 를 이용해서 `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search` 로 이동

  해당 디렉터리에 오른쪽 화면에서       오른쪽마우스클릭 -> 새로만들기 -> DWORD(32비트) 값 (D) -> 이름 `BingSearchEnabled` 을 만들고 값 데이터 0 으로 설정`CortanaConsent` 의 값 데이터도 0으로 설정

  작업관리자에서 `windows 탐색기` 혹은 세부정보의 `explorer.exe` 를 다시실행 

* ### 시작 화면 전체화면 설정

  __설정 -> 개인 설정 -> 시작__ 탭에서 

  `전체 시작 화면` 사용 활성화

* ### 색 어둡게

  __설정 -> 개인 설정 -> 색__ 탭에서 

  `색 선택` 어둡게 로 설정

* 디스플레이 보정 & 텍스트 보정

  __설정 -> 디스플레이__ 탭에서

  하단의 `고급 디스플레이 설정` -> `디스플레이 1의 어댑터 속성을 표시 ` -> `색관리 탭의 색관리` -> `고급` -> `디스플레이 보정`

  __설정 -> 디스플레이__ 탭에서

  야간모드 -> 40 으로 설정

## Putty 터미널 설정

Default Foreground - 187 / 187 / 187

Default Background - 0 / 0 / 0

ANSI Black - 77 / 77 / 77

ANSI Green - 152 / 251 / 152

ANSI Yellow - 240 / 230 / 140

ANSI Blue - 205 / 133 / 63

ANSI Blue Bold - 135 / 206 / 235

ANSI Magenta - 205 / 92 / 92

ANSI Cyan - 255 / 160 / 160

ANSI Cyan Bold - 255 / 215 / 0

ANSI White - 245 / 222 / 179