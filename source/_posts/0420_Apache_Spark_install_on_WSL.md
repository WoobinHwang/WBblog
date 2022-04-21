---
title: "PySpark 설치 WSL환경"
author: "woobin"
date: '2022-04-20'
---

# 0420_Spark_install_on_WSL

- java 8버전이나 11버전 설치 선행 필요
spark홈페이지에서 다운받고 spark라는 폴더에 설치 선행 필요

- 환경변수 세팅

```bash
$ vi ~/.bashrc
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled.png)

- 아래 문구를 추가

```bash
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ export SPARK_HOME=/mnt/c/spark
$ export PATH=$JAVA_HOME/bin:$PATH
$ export PATH=$SPARK_HOME/bin:$PATH
$ export PYSPARK_PYTHON=/usr/bin/python3
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%201.png)

1. → JAVA_HOME에는 아래 경로를 입력

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%202.png)

1. → SPARK_HOME에는 아래 경로를 입력

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%203.png)

- bashrc파일 업데이트하고 제대로 설정이 되었는지 확인

```bash
$ source ~/.bashrc
$ echo $JAVA_HOME
$ echo $SPARK_HOME
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%204.png)

- pyspark 실행

```bash
$ pyspark
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%205.png)

- 아래 코드가 실행되는지 확인

```bash
$ rd = sc.textFile("README.md")
$ rd.count()
# 109
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%206.png)

- 종료

```bash
$ quit()
```

![Untitled](/Images/0420_Apache_Spark_install_on_WSL/Untitled%207.png)