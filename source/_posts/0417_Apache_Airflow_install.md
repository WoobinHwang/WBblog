---
title: "Apache Airflow 설치"
author: "woobin"
date: '2022-04-17'
---

# 0417_Apache_Airflow_install

- airflow를 설치하기 위해 pip를 설치

```bash
$ sudo apt install python3-pip
```

- 라이브러리 설치

```bash
$ sudo pip3 install virtualenv
```

- 가상환경 생성

```bash
$ virtualenv venv
```

- 가상환경 접속

```bash
$  source venv/bin/activate
```

- bashrc 설정 (아래 두줄이 같은 결과가 나오게 해야함)

```bash
$ vi ~/.bashrc
$ source ~/.bashrc
# $ source venv/bin/activate
$ echo $AIRFLOW_HOME
$ pwd
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled.png)

- 가상환경 취소 / 적용하는 방법

```bash
$ deactive
$ source venv/bin/activate
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%201.png)

- PostgreSQL, Slack, Celery 패키지 설치 (진행중인 상황)

```bash
$ pip3 install ‘apache-airflow[postgres, slack, celery]’
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%202.png)

- DB 초기화

```bash
$ airflow db init
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%203.png)

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%204.png)

- username 설정

```bash
$ airflow users create --username airflow --password airflow --firstname name --lastname airflow --role Admin --email my@email.com
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%205.png)

- webserver를 실행

```bash
$ airflow webserver -p 8081
```

- 정상적으로 작동 되는지  [http://localhost:8081/login/](http://localhost:8081/login/) 에 접속

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%206.png)

- airflow.cfg 파일에 들어가서 load_examples 부분이 True이면 False로 바꾸고 저장.

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%207.png)

- DB 초기화

```bash
$ airflow db reset
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%208.png)

- 웹서버 실행 (DB가 비어있는것을 확인)

```bash
$ airflow webserver
```

![Untitled](/Images/0417_Apache_Airflow_install/Untitled%209.png)