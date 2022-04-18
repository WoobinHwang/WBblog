---
title: "Ubuntu 설치"
author: "woobin"
date: '2022-04-18'
---

# 0418_Ubuntu_install

microsoft store 설치

![Untitled](/Images/0418_Ubuntu_install/Untitled.png)

아이디 패스워드 설정

![Untitled](/Images/0418_Ubuntu_install/Untitled%201.png)

버전 확인

![Untitled](/Images/0418_Ubuntu_install/Untitled%202.png)

- 수도 업데이트/업그레이드

```bash
$ sudo apt-get update
$ sudo apt-get upgrade # Y
```

![Untitled](/Images/0418_Ubuntu_install/Untitled%203.png)

- pip3 를 설치해달라는 문구를 확인

$ sudo apt install python3-pip

![Untitled](/Images/0418_Ubuntu_install/Untitled%204.png)

- virtualenv 패키지 설치

$ pip3 install virtualenv

폴더를 만들고 접속

![Untitled](/Images/0418_Ubuntu_install/Untitled%205.png)

가상환경 구축

$ virtualenv venv

![Untitled](/Images/0418_Ubuntu_install/Untitled%206.png)

가상환경 적용

$ source venv/bin/activate

### 라이브러리 설치중 java의 문제로 경고나 에러가 뜬다면 자바가 제대로 호출이 되는지 확인할것

```bash
$ sudo vi /etc/environment
```

- JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64”

- 업데이트 후 경로 확인 (현재 wsl환경이 문제인지 매번 업데이트해줘야하는 상황.)

```bash
$ source /etc/environment
$ echo $JAVA_HONE
```