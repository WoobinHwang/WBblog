---
title: "pip Library Update"
author: "woobin"
date: '2022-05-02'
---

# 0502_Python_pip_Upgrade

- 데스크탑에서 VSCode를 이용하는중...
- VSCode에서 git bash를 이용하는데 자꾸 이상한 글귀가 출력됨.

```bash
WARNING: You are using pip version 20.2.1; however, version 22.0.4 is available.
You should consider upgrading via the 'c:\users\zxcvb\appdata\local\programs\python\python38\python.exe -m pip install --upgrade pip' command.
```

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled.png)

 ——>pip를 업그레이드 하라는 뜻으로 보인다.

- pip의 버전을 확인

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%201.png)

 ——> 버전을 보아하니 추측이 확신으로 바뀜

- pip install --upgrade pip

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%202.png)

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%203.png)

 ——> 조금 불안하긴 했지만

- list로 확인해보니 최신버전이 다운되어있는 모습
->(심지어 노란 글귀도 안뜸)

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%204.png)

- 혹시 가상환경에서만 업그레이드가 되었나 싶기에 가상환경 해제하고 확인

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%205.png)

 ——이번에도 추측은 확신이 되었고...

- 다시 실행

$ pip install --upgrade pip

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%206.png)

- list로 확인

$ pip list

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%207.png)

 ——> 버전은 업그레이드 되었지만 가상환경이 아닌데도 너무 많은 패키지가 설치되어있는걸 확인

- 패키지 삭제

$ pip uninstall 패키지

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%208.png)

 ——> 명령어가 적용되는걸 확인하고 나머지도 지우러 감.

- 여러 라이브러리를 지우려 할 때

$ pip uninstall 패키지 패키지 패키지 ...

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%209.png)

- 청소 끝!

![Untitled](/Images/0502_Python_pip_Upgrade/Untitled%2010.png)