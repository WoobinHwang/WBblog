---
title: "Apache Spark 설치"
author: "woobin"
date: '2022-04-19'
---

# 0419_Apache_Spark_install

- spark 설치 **(자바 설치가 선행되어야 함)**
- [https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz) 홈페이지 들어가서
- [https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz) 링크를 클릭해서 설치

![Untitled](/Images/0419_Apache_Spark_install/Untitled.png)

- tgz 압축을 풀기 위한 WinRAR archiver 설치

![Untitled](/Images/0419_Apache_Spark_install/Untitled%201.png)

- [https://github.com/cdarlint/winutils](https://github.com/cdarlint/winutils) 링크 들어가서 spark의 버전과 맞는 폴더의 winutlis.exe 파일 설치(download)

![Untitled](/Images/0419_Apache_Spark_install/Untitled%202.png)

- 설치한 프로그램들을 hadoop폴더로 모아서 정리

- winrar 설치 ( 자바 설치를 선행 )

![Untitled](/Images/0419_Apache_Spark_install/Untitled%203.png)

- 스파크를 Extract to “spark-3.2.0-bin-hadoop3.2\” 클릭해서 설치

![Untitled](/Images/0419_Apache_Spark_install/Untitled%204.png)

- spark 폴더가 생성됨

![Untitled](/Images/0419_Apache_Spark_install/Untitled%205.png)

- spark 폴더를 c드라이브로 옮김

- conf 폴더(설정 폴더)의 log4j.properties 를 메모장으로 열고, 
log4j.rootCategory=INFO, console 부분을
log4j.rootCategory=ERROR, console 로 수정

![Untitled](/Images/0419_Apache_Spark_install/Untitled%206.png)

- c드라이브에 winutils 폴더 내부에 bin 폴더 만듦

![Untitled](/Images/0419_Apache_Spark_install/Untitled%207.png)

- c드라이브에 tmp 폴더 내부에 hive 폴더 만듦
- 폴더가 없을 경우 ***ChangeFileModeByMask error (3)*** 의 **에러가 나올 수 있음.

![Untitled](/Images/0419_Apache_Spark_install/Untitled%208.png)

- winutils.exe 파일을 hadoop에서 hive폴더 내부로 옮김

- cmd(명령 프롬프트)에서 명령어 작성

```bash
$ winutils.exe chmod 777 \tmp\hive
```

![Untitled](/Images/0419_Apache_Spark_install/Untitled%209.png)

**-> chmod**: 파일들이나 디렉토리의 파일 시스템 모드들을 바꿈. 그 모드들은 허가나 특별한 모드들을 포함.

—>1= 실행 권한, 2= 쓰기 권한, 4= 읽기 권한
—> chmod 777: 모든 사용자들에게 모든 권한을 허용 (권장되지 않음) ——> 따라서 user권한(1+2+4) group권한(1+2+4) other권한(1+2+4) 

⇒ 1(읽기 됨) + 2(쓰기 됨) + 3(실행 됨) 을 뜻함.

- 시스템 환경변수 편집 실행

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2010.png)

- 환경 변수(N)... 클릭

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2011.png)

- ‘사용자 변수’ 탭의 새로 만들기 클릭

- SPARK_HOME 환경변수 설정

SPARK_HOME
C:\spark

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2012.png)

- JAVA_HOME 환경변수 설정

JAVA_HOME

C:\jdk

- HADOOP_HOME 환경변수 설정

HADOOP_HOME

C:\winutils

- 파이썬 환경 설정

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2013.png)

- 끝나면 4개 항목이 추가된 것을 확인

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2014.png)

- 사용자 변수 중에 Path를 찾아서 아래의 코드를 추가
%SPARK_HOME
%\bin%JAVA_HOME%\bin

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2015.png)

- cmd(명령 프롬프트) 켜고 spark폴더로 접속

$ cd c:\spark

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2016.png)

- pyspark 실행

$ pyspark

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2017.png)

- http:README.md파일을 불러와서 아래 코드가 실행되는지 확인

```bash
$ rd= sc.textFile(”README.md”)
$ rd.count()
# 109
```

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2018.png)

---

- 주피터노트북에서 사용 할 때는 아래 설정을 추가해야 함.
PYSPARK_DRIVER_PYTHON
jupyter
PYSPARK_DRIVER_PYTHON_OPTS
notebook

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2019.png)

- 추가된 것을 확인

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2020.png)

- C드라이브에 추가한 폴더:
jdk, jre hadoop, spark, tmp, winutils

![Untitled](/Images/0419_Apache_Spark_install/Untitled%2021.png)