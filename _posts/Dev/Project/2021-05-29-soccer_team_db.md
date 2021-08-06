---

layout: post
title: DataBase 전북현대모터스 설계
categories: Project
message: DataBase
order: 1

---

## 전북현대모터스 DB

2020년도 전북현대모터스 기반으로 한 데이터베이스 설계입니다.

그외 가상의 정보도 추가하였습니다.

BCNF 정규형 상태입니다.

### 구단

__구단 테이블__

```mysql
CREATE TABLE 구단(

구단번호 VARCHAR(10) NOT NULL,
구단명 VARCHAR(20) NOT NULL,
PRIMARY KEY(구단번호),
UNIQUE (구단명)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-1.png">

​     

__구단 정보 테이블__

```mysql
CREATE TABLE 구단정보(

구단번호 VARCHAR(10) NOT NULL,
연고지 VARCHAR(20),
창립년도 DATE,
PRIMARY KEY (구단번호),
FOREIGN KEY (구단번호) REFERENCES 구단 (구단번호)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-2.png">

### 선수

__선수 계약 테이블__

```mysql
CREATE TABLE 선수계약(

계약번호 VARCHAR(10) NOT NULL,
계약시작일 DATE,
계약종료일 DATE,
연봉 INT DEFAULT '0' NOT NULL,
PRIMARY KEY(계약번호),
CHECK (계약번호 LIKE  's-1___'),
CHECK(계약시작일 < 계약종료일)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-3.png">

​     

__선수 테이블__

```mysql
CREATE TABLE 선수(

선수번호 VARCHAR(10) NOT NULL,
이름 VARCHAR(20) NOT NULL,
나이 int,
포지션 VARCHAR(10) NOT NULL,
등번호 int NOT NULL,
구단번호 VARCHAR(10) NOT NULL,
계약번호 VARCHAR(10) NOT NULL,
PRIMARY KEY(선수번호),
FOREIGN KEY (구단번호) REFERENCES 구단 (구단번호),
FOREIGN KEY (계약번호) REFERENCES 선수계약 (계약번호),
UNIQUE (선수번호),
UNIQUE (등번호),
UNIQUE(계약번호),
CHECK (나이 >= 1 AND 나이 <= 99),
CHECK(선수번호 LIKE 'b__'),
CHECK(계약번호 LIKE 's-1___'),
CHECK(등번호 >= 1 AND 등번호 <= 99),
CHECK (포지션 in ('GK','DF','MF','FW’))

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-4.png">

### 코치

__코치 계약 테이블__

```mysql
CREATE TABLE 코치계약(

계약번호 VARCHAR(10) NOT NULL,
계약시작일 DATE,
계약종료일 DATE,
연봉 INT DEFAULT '0' NOT NULL,
PRIMARY KEY(계약번호),
CHECK(계약번호 LIKE  's-2___'),
CHECK(계약시작일 < 계약종료일)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-5.png">

​    

__코치 테이블__

```mysql
CREATE TABLE 코치(

코치번호 VARCHAR(10) NOT NULL,
이름 VARCHAR(20) NOT NULL,
역할 VARCHAR(10),
구단번호 VARCHAR(10) NOT NULL,
계약번호 VARCHAR(10) NOT NULL,
PRIMARY KEY(코치번호),
UNIQUE (계약번호),
FOREIGN KEY (구단번호) REFERENCES 구단 (구단번호),
FOREIGN KEY (계약번호) REFERENCES 코치계약 (계약번호),
CHECK(코치번호 LIKE 'c__'),
CHECK(계약번호 LIKE 's-2___’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-6.png">

### 의료진

__의료진 계약 테이블__

```mysql
CREATE TABLE 의료진계약
(
계약번호 VARCHAR(10) NOT NULL,
계약시작일 DATE,
계약종료일 DATE,
연봉 INT DEFAULT '0' NOT NULL,
PRIMARY KEY(계약번호),
CHECK(계약번호 LIKE  's-3___'),
CHECK(계약시작일 < 계약종료일)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-7.png">

​     

__의료진 테이블__

```mysql
CREATE TABLE 의료진(

의료진번호 VARCHAR(10) NOT NULL,
이름 VARCHAR(20) NOT NULL,
직책 VARCHAR(10),
구단번호 VARCHAR(10) NOT NULL,
계약번호 VARCHAR(10) NOT NULL,
PRIMARY KEY(의료진번호),
UNIQUE (계약번호),
FOREIGN KEY (구단번호) REFERENCES 구단 (구단번호),
FOREIGN KEY (계약번호) REFERENCES 의료진계약 (계약번호),
CHECK(의료진번호 LIKE 'm__'),
CHECK(계약번호 LIKE 's-3___’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-8.png">

### 직원

__직원 계약 테이블__

```mysql
CREATE TABLE 직원계약(

계약번호 VARCHAR(10) NOT NULL,
계약시작일 DATE,
계약종료일 DATE,
연봉 INT DEFAULT '0' NOT NULL,
PRIMARY KEY(계약번호),
CHECK(계약번호 LIKE  's-4___'),
CHECK(계약시작일 < 계약종료일)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-9.png">

​    

__직원 테이블__

```mysql
CREATE TABLE 직원(

직원번호 VARCHAR(10) NOT NULL,
이름 VARCHAR(20) NOT NULL,
나이 int(10),
부서 VARCHAR(20) NOT NULL,
입사년도 date,
구단번호 VARCHAR(10) NOT NULL,
계약번호 VARCHAR(10) NOT NULL,
PRIMARY KEY(직원번호),
UNIQUE (계약번호),
FOREIGN KEY (구단번호) REFERENCES 구단 (구단번호),
FOREIGN KEY (계약번호) REFERENCES 직원계약 (계약번호),
CHECK(직원번호 LIKE 'e__'),
CHECK(계약번호 LIKE 's-4___'),
CHECK (나이 >= 1 AND 나이 <= 99)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-10.png">

### 지도 (코치 - 선수)

```mysql
CREATE TABLE 지도(

지도번호 VARCHAR(10) NOT NULL,
코치번호 VARCHAR(10) NOT NULL,
선수번호 VARCHAR(10) NOT NULL,
지도일자 DATE,
종류 VARCHAR(30) NOT NULL,
PRIMARY KEY(지도번호),
FOREIGN KEY (코치번호) REFERENCES 코치 (코치번호),
FOREIGN KEY (선수번호) REFERENCES 선수 (선수번호),
CHECK(지도번호 LIKE 't__’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-11.png">

### 치료 (선수 - 의료진)

```mysql
CREATE TABLE 치료(

치료번호 VARCHAR(10) NOT NULL,
의료진번호 VARCHAR(10) NOT NULL,
선수번호 VARCHAR(10) NOT NULL,
치료일자 DATE,
부상내용 VARCHAR(30) NOT NULL,
PRIMARY KEY(치료번호),
FOREIGN KEY (의료진번호) REFERENCES 의료진 (의료진번호),
FOREIGN KEY (선수번호) REFERENCES 선수 (선수번호),
CHECK(치료번호 LIKE 'h__’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-12.png">

### 경기

```mysql
CREATE TABLE 경기(

경기번호 VARCHAR(10) NOT NULL,
경기일시 DATE NOT NULL,
홈팀 VARCHAR(10) NOT NULL,
원정팀 VARCHAR(10) NOT NULL,
홈팀스코어 INT DEFAULT '0' NOT NULL,
원정팀스코어 INT DEFAULT '0' NOT NULL,
PRIMARY KEY(경기번호),
CHECK(경기번호 LIKE 'g__’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-13.png">

### 출전 (선수 - 경기)

```mysql
CREATE TABLE 출전(

경기번호 VARCHAR(10) NOT NULL,
선수번호 VARCHAR(10) NOT NULL,
득점 INT DEFAULT '0',
도움 INT DEFAULT '0',
PRIMARY KEY(경기번호,선수번호),
FOREIGN KEY (경기번호) REFERENCES 경기 (경기번호),
FOREIGN KEY (선수번호) REFERENCES 선수 (선수번호)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-14.png">

### 재정

```mysql
CREATE TABLE 재정(

재정번호 VARCHAR(10) NOT NULL,
분류 VARCHAR(10) NOT NULL,
일시 DATE NOT NULL,
잔여금액 INT DEFAULT '0' NOT NULL,
지출내역 INT DEFAULT '0',
수입내역 INT DEFAULT '0',
PRIMARY KEY(재정번호),
CHECK (재정번호 LIKE  'f__’)

); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-15.png">

### 뷰

__지출 내역 뷰__

```mysql
CREATE VIEW 지출내역
AS SELECT 분류,일시,지출내역
FROM 재정
WHERE 지출내역 > 0
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-16.png">

​     

__공격 포인트 현황 뷰__

```mysql
CREATE VIEW 공격포인트현황(이름,도움,득점)
AS SELECT 선수.이름,  SUM(출전.득점현황),  SUM(출전.도움현황)
FROM         출전,  선수
WHERE   선수.선수번호   =   출전.선수번호
GROUP BY   선수.선수번호;
```



<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-17.png">

​     

__특정 선수 공격 포인트 검색__

```mysql
CREATE VIEW 공격포인트현황(이름,도움,득점)
AS SELECT 선수.이름,  SUM(출전.득점현황),  SUM(출전.도움현황)
FROM         출전,  선수
WHERE   선수.선수번호   =   출전.선수번호
GROUP BY   선수.선수번호;
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-18.png">

​     

### 트랙잭션

__계약 트랜잭션1__

```mysql
##새로운 선수 계약
Insert 
into 선수계약 
values('s-1024','2021-01-01','2022-01-01','9600'); 

##선수정보 입력
Insert 
into 선수 
values ('b24','홍길동','31','DF','36','01','s-1024'); 
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-19.png">

​     

__계약 트랜잭션2__

```mysql
##새로운 직원 계약
insert 
into 직원계약
values('s-4008','2021-01-01','2022-01-01','7700');

##직원정보 입력
insert 
into 직원 
values ('e08','홍길동','55','마케팅','2021-01-01','01','s-4008');
```

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-soccer_team_db-20.png">

