---
title: "Apache NiFi 설치"
author: "woobin"
date: '2022-04-18'
---

# 0418_Apache_NiFi_install

- 설명.1 : 소프트웨어 시스템 간 데이터 흐름을 자동화하도록 설계된 아파치 소프트웨어 재단의 소프트웨어 프로젝트
- 설명.2 : 시스템 간 데이터 전달을 효율적으로 처리, 관리, 모니터링 하기에 최적화된 시스템

- curl을 이용하여 nifi를 현재 경로에 설치

```bash
$ sudo wget [https://downloads.apache.org/nifi/1.16.0/nifi-1.16.0-bin.tar.gz](https://downloads.apache.org/nifi/1.16.0/nifi-1.16.0-bin.tar.gz)
```

- tar.gz 압축 풀기

```bash
$ sudo tar xvzf nifi-1.16.0-bin.tar.gz
```

- NiFi 아이디 비밀번호 세팅
아이디: human
비밀번호: human1234567890 (10자리 이상)

```bash
$ sudo ./bin/nifi.sh set-single-user-credentials human human1234567890
```

![Untitled](/Images/0418_Apache_NiFi_install/Untitled.png)

- NiFI 실행(ubuntu)

```bash
$ sudo ./bin/nifi.sh start
```

![Untitled](/Images/0418_Apache_NiFi_install/Untitled%201.png)

- [https://127.0.0.1:8443/nifi](https://127.0.0.1:8443/nifi/login) 접속 ( 시간이 많이 소요됨. )

![Untitled](/Images/0418_Apache_NiFi_install/Untitled%202.png)