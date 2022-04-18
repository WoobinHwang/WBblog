---
title: "Elasticsearch 설치"
author: "woobin"
date: '2022-04-18'
---

# 0418_Elasticsearch_install

- 설명: HTTP웹 인터페이스와 스키마에서 자유로운 JSON문서와 함께 분산 멀티테넌트 지원 전문 검색 엔진을 제공

- 시스템 업데이트 및 업그레이드

```bash
$ sudo apt update
$ sudo apt upgrade
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled.png)

- HTTPS관련 패키지 설치, 자바 설치 (설치중인 모습)

```bash
$ sudo apt install apt-transport-https
$ sudo apt install openjdk-11-jdk # Y
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%201.png)

- 설치한 버전 확인

```bash
$ java -version
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%202.png)

- 환경변수 설정하기 위해 아래 문구를 입력

```bash
$ sudo vi /etc/environment
```

JAVA_HOME=”/usr/lib/jvm/java-11-openjdk-amd64

![Untitled](/Images/0418_Elasticsearch_install/Untitled%203.png)

- 환경변수 업데이트

```bash
$ source /etc/environment
```

- 적용 되었는지 확인

```bash
$ echo $JAVA_HOME
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%204.png)

### Elasticsearch 설치

- GPG keys를 확인하여 설치, 라이브러리 설치

```bash
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%205.png)

- elasticsearch 설치

```bash
$ sudo apt-get update

$ sudo apt-get install elasticsearch
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%206.png)

- elasticsearch 서비스를 시작

```bash
$ sudo systemctl start elasticsearch
```

- 실행 시켰을 때 아래의 에러가 뜬다면 다음 코드를 입력해야한다.

System has not been booted with systemd as init system (PID 1).
Can't operate. Failed to connect to bus: Host is down

```bash
$ sudo -b unshare --pid --fork --mount-proc /lib/systemd/systemd --system-unit=basic.target
$ sudo -E nsenter --all -t $(pgrep -xo systemd) runuser -P -l $USER -c "exec $SHELL"
$ sudo systemctl enable elasticsearch
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%207.png)

- 다시 서비스를 실행시켜본다.

```bash
$ sudo systemctl start elasticsearch
```

- 실제로 작동하는지 확인해본다.

```bash
curl -X GET "localhost:9200/"
```

![Untitled](/Images/0418_Elasticsearch_install/Untitled%208.png)

- 인터넷 환경에서도 확인해본다
[http://localhost:9200/](http://localhost:9200/)

![Untitled](/Images/0418_Elasticsearch_install/Untitled%209.png)

- 끝났으면 서비스를 중지한다.

```bash
$ sudo systemctl stop elasticsearch
```

- 끝