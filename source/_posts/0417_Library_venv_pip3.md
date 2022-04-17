# 라이브러리 / 가상 환경 / pip, pip3 기능

### 라이브러리 (Library):

- 소프트웨어 개발 시 자주 쓰거나 특정한 기능을 모듈로 만들어 사용자들이 사용 할 수 있게 만들어둔것.

### 가상 환경 (virtual environment):

- 작업하는 프로젝트마다 같은 라이브러리를 사용하더라도 쓸 때 마다 버전이 달라지곤 하는데, 이를 해결하기 위해 가상 환경이라는 독립적인 공간을 만들어 해결 할 수 있다.
- WSL(Windows Subsystem for Linux)에서 가상 환경을 만들고 적용 (예시)

```bash
$ sudo pip3 install virtualenv # 패키지가 없다면 설치
$ virtualenv venv
$ source venv/bin/activate
```

### pip, pip3:

- pip: python2 버전의 패키지를 설치하는 기능
pip3: python3 버전의 패키지를 설치하는 기능
—> 파이썬 버전에 맞게 사용

- WSL에서 python3 패키지를 패키지를 설치 (예시)

$ pip3 install elasticsearch

- 버전을 확인하는 temp.py를 작성

![Untitled](/Images/0417_Library_venv_pip3/Untitled.png)

- 버전 확인

```bash
$ python3 temp.py
```

![Untitled](/Images/0417_Library_venv_pip3/Untitled%201.png)

- WSL에서 다운그레이드 하고 버전 확인

$ pip3 install elasticsearch==7.6.0

$ python3 temp.py

![Untitled](/Images/0417_Library_venv_pip3/Untitled%202.png)

- 설치한 패키지 목록
- $ pip3 list

![Untitled](/Images/0417_Library_venv_pip3/Untitled%203.png)