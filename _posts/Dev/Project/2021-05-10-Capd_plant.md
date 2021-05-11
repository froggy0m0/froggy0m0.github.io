---
layout: post
title: 라즈베리파이를 이용한 식물 재배기
categories: Project
message: 캡스톤디자인

---

# 식물 재배기   

### 리눅스 

### python3

```
yum install python3 -y

python3 -v
python -v

pip install --upgrade pip

pip -V
```



### 시스템 자동화

```c
//   home/user/test.c
#include<stdio.h>
#include<time.h>

void main(void)
{
time_t timer;
struct tm *t;
timer = time(NULL);
t = localtime(&timer);

FILE *fp = fopen("text.txt","a+");

fprintf(fp,"%dH %dM %dS\n",t->tm_hour, t->tm_min, t->tm_sec);

fclose(fp);
}
```

```
## 컴파일
[user@localhost ~]$ gcc -o test test.c

##쉘
[user@localhost ~]$ vi test.sh

#! /bin/bash
/home/user/test
```

```
## crontab

[user@localhost ~]$ crontab -e
* * * * * /home/user/test.sh
```



### DB

mysql> create database Plant;

```
mysql> create database PlantDB;
Query OK, 1 row affected (0.00 sec)

mysql> use plantdb;
```



```sql
-- 관리자가 입력할 식물의 정보
CREATE TABLE plant(
	plantcode VARCHAR(10) NOT NULL,
	name VARCHAR(20) NOT NULL,
	water int DEFAULT '0' NOT NULL,
	temperature int DEFAULT '0' NOT NULL,
	humidity int DEFAULT '0' NOT NULL,
    -- txt TEXT NOT NULL,
	PRIMARY KEY(plantcode),
	CHECK(plantcode LIKE 'p__')
);
-- insert 
insert into plant VALUES ('p01','tomato','2','25','30');
insert into plant VALUES ('p02','abcdef','2','27','35');
```



```sql
-- 사용자 정보            관리하는 식물명 ?? 없음
CREATE TABLE userinfo(
    id VARCHAR(20) NOT NULL,
    pw VARCHAR(20) NOT NULL,
    plantcode VARCHAR(10) NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY (plantcode) REFERENCES plant (plantcode)
    -- CHECK (plnatcode in ('p01','p02')) -- 제약조건에 해당하는 식물만 등록가능
);

insert into userinfo VALUES ('jj001','12345','p03'); -- 외래키오류??
```

```sql
-- 라즈베리파이로 부터 받아오는 저장될 값들
CREATE TABLE management(
    id VARCHAR(20) NOT NULL,
    plantcode VARCHAR(10) NOT NULL,
    temperature INT NOT NULL,
    humidity INT  NOT NULL,
    ddate DATETIME NOT NULL,		-- yyyy-mm--dd
    PRIMARY KEY(id,plantcode),
    FOREIGN KEY (id) REFERENCES userinfo (id),
    FOREIGN KEY (plantcode) REFERENCES plant (plantcode)
);

insert into management VALUES ('jj003','p04','23','23','2001-12-12'); -- 외래키오류??
```

```sql
CREATE VIEW v1
AS SELECT temperature,humidity,ddate
FROM management
WHERE id = $ and plantcode = $ ; -- $는 코드에서 받아오는걸로
```

#### pymysql

<img src="https://snaklad.github.io/assets/img/GitHubPage/2021-05-10-Capd_plant-1" width="600" height="300">

### 안드로이드

### 라즈베리파이

