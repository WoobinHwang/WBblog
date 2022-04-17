---
title: "VSCode Remote 기능 연동하기"
author: "woobin"
date: '2022-04-17'
---

# 0417_VSCode_Remote_function

VSCode에서 remote를 검색하여 설치

[//]: # (/Images/0417_VSCode_Remote_function/Untitled 6.jpg)
![Untitled](/Images/0417_VSCode_Remote_function/Remote.jpg)

- 터미널을 켜고 WSL이 있는지 확인

![Untitled](/Images/0417_VSCode_Remote_function/Untitled.png)

- WSL에서 가상환경으로 설정

```bash
$ source venv/bin/activate
```

![Untitled](/Images/0417_VSCode_Remote_function/Untitled%201.png)

- Chapter03 파일 속 [hello.py](http://hello.py) 파일을 만들고 저장

![Untitled](/Images/0417_VSCode_Remote_function/Untitled%202.png)

- vscode의 wsl터미널에서 [hello.py](http://hello.py) 파이썬 파일 실행

```bash
$ python3 hello.py
```

![Untitled](/Images/0417_VSCode_Remote_function/Untitled%203.png)

- [hello.py](http://hello.py) 를 수정하고 다시 실행

![Untitled](/Images/0417_VSCode_Remote_function/Untitled%204.png)

![Untitled](/Images/0417_VSCode_Remote_function/Untitled%205.png)