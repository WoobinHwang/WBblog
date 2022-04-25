---
title: "Oracle SQL 설치"
author: "woobin"
date: '2022-04-25'
---

# 0425_Oracle_SQL_install_on_window

# Oracle SQL 설치

- 구글 oracle database 19c download 검색

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled.png)

- 오라클 로그인 후 다운로드하기

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%201.png)

- db_home이라는 폴더에 압축 풀어둠.
setup파일 관리자권한으로 실행

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%202.png)

- 단일 인스턴스 데이터베이스 생성 및 구성 (default)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%203.png)

- 데스크탑 클래스 (default)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%204.png)

- 가상계정 사용 (default)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%205.png)

- 컨테이너 데이터베이스 설정 (해제)
데이터베이스 명: myoracle
비밀번호: 1234

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%206.png)

- 바로 설치 (default)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%207.png)

- 설치 진행중

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%208.png)

### 세팅하기

- 설치가 끝났다면, SQL Plus 관리자 권한으로 실행

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%209.png)

- 실행시 사용자명과 비밀번호 설정 해야함.
사용자명은 system
비밀번호는 1234로 지정

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2010.png)

- 테이블 스페이스를 myts라는 이름으로 100MB 크기로 생성
데이터가 증가하면 5MB씩 자동 증가 옵션 추가

```bash
$ CREATE TABLESPACE myts DATAFINE 'C:\sql_lecture\db_homes\oradata\MYORACLE\myts.dbf' SIZE 100M AUTOEXTEND ON NEXT 5M;
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2011.png)

- 사용자 생성하고 권한 부여
사용자명 ora_user
비밀번호 evan

```bash
CREATE USER ora_user IDENTIFIED BY evan DEFAULT TABLESPACE MYTS TEMPORARY TABLESPACE TEMP;
# 사용자가 생성되었습니다.

GRANT DBA TO ora_user;
# 권한이 부여되었습니다.
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2012.png)

- 사용자 계정으로 DB에 접속

```bash
connect ora_user/evan

selcet user from dual;
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2013.png)

---

# Oracle Develop 설치

- 구글에 sql developer download 검색

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2014.png)

- 로그인 후 설치

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2015.png)

### 세팅하기

- 도구 - 환경설정 - 데이터베이스 - NLS
시간기록 TZ형식을 복사하여 시간기록형식에 붙여넣음

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2016.png)

- 코드를 입력하고 실행(F5)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2017.png)

- 아래 두개 프로그램 다운로드
[https://github.com/gilbutITbook/006696/tree/master/01장 환경설정](https://github.com/gilbutITbook/006696/tree/master/01%EC%9E%A5%20%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95)

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2018.png)

- c드라이브 아래에 백업 폴더 만들고 방금 다운한 두개의 파일을 옮김.

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2019.png)

- 명령프롬프트(cmd)창에서 설치

```bash
imp ora_user/evan file=expall.dmp log=empall.log ignore=y grants=y rows=y indexes=y full=y

 imp ora_user/evan file=expcust.dmp log=expcust.log ignore=y grants=y rows=y indexes=y full=y
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2020.png)

- sql developer에서 명령어 입력 후 실행

```bash
SELECt table_name FROM user_tables;
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2021.png)

## 연습

- SQL Plus를 실행

- 로그인하기
사용자명 : system
비밀번호 : 1234

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2022.png)

- 연결하기

```bash
connect ora_user/evan
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2023.png)

- 확인하기

```bash
show user
```

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2024.png)

- 테이블 생성해보기

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2025.png)

- 사이드바에서도 생성된걸 확인 가능 가능

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2026.png)

- 상단에 SQL탭을 누르면 코드가 나온다.

![Untitled](/Images/0425_Oracle_SQL_install_on_window/Untitled%2027.png)