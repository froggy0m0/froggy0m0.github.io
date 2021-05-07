---
layout: post
title: 나의 windows10 셋팅
categories: Memo
message: 백업용..
---

* ## 윈도우 검색중 웹검색 제거하기

레지스트리 편집기(regedit) 를 이용해서 `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search` 로 이동

해당 디렉터리에 오른쪽 화면에서       오른쪽마우스클릭 -> 새로만들기 -> DWORD(32비트) 값 (D) -> 이름 `BingSearchEnabled` 을 만들고 값 데이터 0 으로 설정`CortanaConsent` 의 값 데이터도 0으로 설정

작업관리자에서 `windows 탐색기` 혹은 세부정보의 `explorer.exe` 를 다시실행 

* ## 시작화면 전체화면 설정

