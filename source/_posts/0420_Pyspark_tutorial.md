---
title: "Pyspark 연습"
author: "woobin"
date: '2022-04-20'
---

# 0420_Pyspark_tutorial

spark홈페이지
[https://spark.apache.org/docs/latest/quick-start.html](https://spark.apache.org/docs/latest/quick-start.html)

- [SimpleApp.py](http://SimpleApp.py) 작성
(홈페이지에서 볼 수 있는 예제문에서 input()을 추가함)

```
from pyspark.sql import SparkSession

logFile = "README.md"  # Should be some file on your system
spark = SparkSession.builder.appName("SimpleApp").getOrCreate()
logData = spark.read.text(logFile).cache()

numAs = logData.filter(logData.value.contains('a')).count()
numBs = logData.filter(logData.value.contains('b')).count()

print("Lines with a: %i, lines with b: %i" % (numAs, numBs))

input("Typing....")

spark.stop()
```

![Untitled](/Images/0420_Pyspark_tutorial/Untitled.png)

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%201.png)

- [README.md](http://README.md) 작성

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%202.png)

- README.md파일 내용

This program just counts the number of lines containing ‘a’ and the number containing ‘b’ in a text file. Note that you’ll need to replace YOUR_SPARK_HOME with the location where Spark is installed. As with the Scala and Java examples, we use a SparkSession to create Datasets. For applications that use custom classes or third-party libraries, we can also add code dependencies to spark-submit through its --py-files argument by packaging them into a .zip file (see spark-submit --help for details). SimpleApp is simple enough that we do not need to specify any code dependencies.

We can run this application using the `bin/spark-submit` script:

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%203.png)

- 제대로 작성 됐는지 cat 명령어로도 확인가능

```bash
$ cat README.md
```

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%204.png)

- 스파크 홈페이지에 연동하여 실행

```bash
$ $SPARK_HOME/bin/spark-submit --master local[4] [SimpleApp.py](http://simpleapp.py/)
```

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%205.png)

- =⇒

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%206.png)

- 윈도우에서 확인

[http://localhost:4040/](http://localhost:4040/)

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%207.png)

- ***배포* 
=⇒ spark-submit**

### 예제 1.

- data 폴더와 [friends-by-age.py](http://friends-by-age.py) 작성

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%208.png)

- spark 실행 후 [http://172.22.156.103:4040](http://172.22.156.103:4040/environment/) 접속

```bash
$ $SPARK_HOME/bin/spark-submit --master local[4] [friends-by-age.py](http://friends-by-age.py)
```

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%209.png)

- =⇒

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%2010.png)

 ——> 500개의 record가 있는것을 확인

### 예제 2.

- [titalspent.py](http://titalspent.py) 작성
(main함수 삭제)
(input이라는 객체를 생성했어서 input을 inputs로 변경)

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%2011.png)

- 실행하고 UI로 확인
[http://172.22.156.103:4040/](http://172.22.156.103:4040/)

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%2012.png)

- =⇒

![Untitled](/Images/0420_Pyspark_tutorial/Untitled%2013.png)

 ——> 10000개의 record가 있다는것을 확인 가능