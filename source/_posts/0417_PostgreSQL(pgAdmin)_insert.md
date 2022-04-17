---
title: "PostgreSQL 설치"
author: "woobin"
date: '2022-04-17'
---

# 0417_PostgreSQL(pgAdmin)_insert

- 설명: 확장 가능성 및 표준 준수를 강조하는 객체-관계형 데이터베이스 관리 시스템의 하나

- PostgreSQL 서비스를 실행, 상태 확인

```bash
$ sudo service postgresql strat
$ sudo service postgresql status
```

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled.png)

- PostgreSQL 아이디 비밀번호 설정

```bash
$ sudo -u postgres psql -c “ALTER USER postgres PASSWORD ‘postgres’;”
```

- General탭에서 ‘Name’에 users를 입력(필자는 users2로 작성)

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%201.png)

- Column탭에는 아래의 테이블을 추가 (Save)

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%202.png)

- wsl에서 제대로 저장이 되었는지 확인하러 갑니다.
(l: List 로 추측됨)

```bash
$ sudo -u postgres psql

$ \l
```

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%203.png)

- dataengineering으로 접속
(c: connect 로 추측됨)

```bash
$ \c dataengineering
```

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%204.png)

- 데이터 테이블을 확인
(dt: datatable 로 추측됨)

```bash
$ \dt
```

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%205.png)

- 제대로 확인이 된다면 빠져나옴.
(q: quit 로 추측됨)

```bash
$ \q

$ sudo service postgresql stop
```

![Untitled](/Images/0417_PostgreSQL(pgAdmin)_insert/Untitled%206.png)