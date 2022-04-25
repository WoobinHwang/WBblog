---
title: "데이터베이스의 객체"
author: "woobin"
date: '2022-04-25'
---

# 0425_DataBase_Object

- 문자열 (영어)크기

![Untitled](/Images/0425_DataBase_Object/Untitled.png)

- 문자열 (한글) 크기
한글자당 3byte를 차지하기에 column의 크기를 명시해뒀다면 크기를 유념해서 입력해야한다.

![Untitled](/Images/0425_DataBase_Object/Untitled%201.png)

- 숫자열 크기

![Untitled](/Images/0425_DataBase_Object/Untitled%202.png)

- FLOAT형의 특징때문에 뒤는 0으로 잘려서 나오는 현상
FLOAT[(p)] : NUMBER의 하위 타입, p는 1~128, 디폴트 값은 128, 이진수 기준 최대 22byte

![Untitled](/Images/0425_DataBase_Object/Untitled%203.png)

- 날짜 데이터 타입

![Untitled](/Images/0425_DataBase_Object/Untitled%204.png)