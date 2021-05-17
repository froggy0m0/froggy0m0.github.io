---
layout: post
title: mysql 한글설정 리눅
categories: Project
message: 캡스톤디자인
order: 1
---

## mysql 한글설정

### 작업환경

OS : CentOS 7 

Mysql version :  5.6..51

### ### .cnf 파일 설정

```
/etc/mysql/mysql.conf.d/client.cnf

[client]
default-character-set=utf8
```

```
/etc/mysql/mysql.conf.d/mysqld.cnf

[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
init_connect=SET collation_connection=utf8_general_ci
init_connect=SET NAMES utf8
```

```
/etc/mysql/mysql.conf.d/mysqldump.cnf

[mysqldump]
default-character-set=utf8
```

```
/etc/mysql/mysql.conf.d/mysql.cnf

[mysql.cnf]
default-character-set=utf8
```

### 시스템 재시작  

 systemctl restart mysqld 

### mysql명령어 입력

```mysql
ALTER DATABASE 데이터베이스명 character set utf8 collate utf8_general_ci;

ALTER TABLE 테이블명 CONVERT TO CHARACTER SET utf8;
```



