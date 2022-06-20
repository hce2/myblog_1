---
title: oracle 설치
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: ORACLE
---
SQL

- 설치 주소 : [https://www.oracle.com/kr/database/technologies/oracle19c-windows-downloads.html](https://www.oracle.com/kr/database/technologies/oracle19c-windows-downloads.html)

WINDOWS.X64_193000_db_home 을 다운 받는다.

![Untitled](images/oracle/Untitled.png)

압축을 풀고 맨 마지막에 있는 빨간 아이콘을 관리자로 실행한다.

![Untitled](images/oracle/Untitled%201.png)

![Untitled](images/oracle/Untitled%202.png)

![Untitled](images/oracle/Untitled%203.png)

![Untitled](images/oracle/Untitled%204.png)

![Untitled](images/oracle/Untitled%205.png)

p. 22

데이터베이스 버전, 문자 집합, 전역 데이터베이스 이름, 비밀번호를 설정하고, 컨테이너 데이터베이스로 생성은 체크란은 비워둔다.

비번 1234

기본 설치에 빨간 것은 비밀번호 때문임 신경 x

![Untitled](images/oracle/Untitled%206.png)

예

![Untitled](images/oracle/Untitled%207.png)

설치

![Untitled](images/oracle/Untitled%208.png)

허용

![Untitled](images/oracle/Untitled%209.png)

![Untitled](images/oracle/Untitled%2010.png)

![Untitled](images/oracle/Untitled%2011.png)

관리자 권한으로 실행

![Untitled](images/oracle/Untitled%2012.png)

![Untitled](images/oracle/Untitled%2013.png)

![Untitled](images/oracle/Untitled%2014.png)

![Untitled](images/oracle/Untitled%2015.png)

C:\sql_lecture\sqldeveloper-21.4.3.063.0100-x64\sqldeveloper에

응용프로그램 클릭

![Untitled](images/oracle/Untitled%2016.png)

아니오

![Untitled](images/oracle/Untitled%2017.png)

![Untitled](images/oracle/Untitled%2018.png)

새 데이터베이스 접속

비번 xiromjr

![Untitled](images/oracle/Untitled%2019.png)

![Untitled](images/oracle/Untitled%2020.png)

도구 , 환경설정 데이터베이스 nls   날짜 타입 변경    확인

![Untitled](images/oracle/Untitled%2021.png)

[https://github.com/gilbutITbook/006696/blob/master/01장 환경설정/expcust.dmp](https://github.com/gilbutITbook/006696/blob/master/01%EC%9E%A5%20%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95/expcust.dmp)

backup 폴더 생성 후 파이 옮기기

![Untitled](images/oracle/Untitled%2022.png)

cdm를 열고 백업 파일로 이동 후 

```sql
imp ora_user/xiromjr file=expall.dmp log=empall.log ignore=y grants=y rows=y indexes=y full=y
```

```sql
imp ora_user/xiromjr file=expcust.dmp log=expcust.log ignore=y grants=y rows=y indexes=y full=y
```

성공

![Untitled](images/oracle/Untitled%2023.png)