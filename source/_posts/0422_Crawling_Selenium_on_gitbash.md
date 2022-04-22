---
title: "Selenium 연습"
author: "woobin"
date: '2022-04-22'
---

# 0422_Crawling_Selenium_on_gitbash

- 가상환경 만들고 적용

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled.png)

- 패키지 설치

$ pip install selenium

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%201.png)

- 크롬 버전 확인

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%202.png)

- 크롬 드라이버 다운로드
크롬에 chrome driver 검색하고 홈페이지 들어가기
[https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%203.png)

- 맞는 버전과 운영체제로 다운받음.

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%204.png)

- 드라이버를 내가 사용할 폴더로 옮김

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%205.png)

- [temp.py](http://temp.py) 작성

```bash
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome()
driver.get("https://www.google.com/")
```

- 실행

$ python temp.py

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%206.png)

- 입력창에 리그 오브 레전드를 입력

```bash
elem= driver.find_element_by_name("q")
elem.send_keys("python")
```

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%207.png)

- 검색버튼 누르기

```bash
elem.send_keys(Keys.RETURN)
```

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%208.png)

- 클래스 이름으로 조회 찾으려고 했더니 다수의 동일한 클래스 이름들이 조회됨.

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%209.png)

- 0번째의 text를 target 객체로 만들어 출력

```bash
$ target= driver.find_elements_by_css_selector(".LC20lb.MBeuO.DKV0Md")[0].text
$ driver.close()
$ print(target)
```

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%2010.png)

- 연습 끝!

- 여러 시행착오 끝에 네이버같은 링크를 들어갈 때 새로운 탭이 열리는 경우엔 셀레니움이 인식을 못하는 것을 알아냄!

- 계속 실패하던 더보기 버튼 누르는것까지 구현함.

```bash
$ from selenium import webdriver
$ from selenium.webdriver.common.keys import Keys
$ import time

$ driver = webdriver.Chrome()
$ driver.get("https://game.naver.com/lounge/League_of_Legends/db/champion")
$ time.sleep(5)

# # 1번째 챔피언 이름 추출
$ fir= ".card_content-header__title__oJkdR"
$ target= driver.find_elements_by_css_selector(fir)[0].text
$ print("--------------")
$ print(target)
$ print("--------------")

# # 더보기 버튼 클릭
$ sec= ".gamedb_icon__1jSHy"
$ driver.find_element_by_css_selector(sec).click()

# 종료 단계
# driver.close()
$ print("good")
```

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%2011.png)

![Untitled](/Images/0422_Crawling_Selenium_on_gitbash/Untitled%2012.png)