# 0424_Sellenium_LOLchampions_name

- 셀레니움을 이용하여 LOL게임의 챔피언 이름들 추출

- 웹페이지를 보며 30개밖에 안나오는 리스트를 더보기 버튼을 눌러 리스트를 더 불러올 필요가 있다고 생각했다.

![Untitled](/Images/0424_Sellenium_LOLchampions_name/Untitled.png)

- 더보기 버튼이 나오지 않을때까지

![Untitled](/Images/0424_Sellenium_LOLchampions_name/Untitled%201.png)

- 처음에는 for문으로 원하는 조건이 나올때까지 돌리려는 생각을 했지만 for문 특성상 정해진 횟수를 제어 할 수 없었다.

- 그래서 찾아낸게, while문으로 특정 조건을 걸어두지 않으면 무한루프가 돌아간다는것을 찾아냄.

```python
while True:
    more.click()
    time.sleep(1)
    target= driver.find_elements_by_css_selector(fir)
    if divmod(len(target), 30)[1] != 0 :
        break
```

![Untitled](/Images/0424_Sellenium_LOLchampions_name/Untitled%202.png)

- 이름은 챔피언 박스를 list로 만들어 names에 append함수를 이용하여 추출

```python
names= []
for i in target:
    names.append(i.text)
print(names)
```

![Untitled](/Images/0424_Sellenium_LOLchampions_name/Untitled%203.png)

- 최종 코드

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Chrome()
driver.get("https://game.naver.com/lounge/League_of_Legends/db/champion")
time.sleep(5)

# # 1번째 챔피언 이름 추출
fir= ".card_content-header__title__oJkdR"
# target : 챔피언 박스 list
target= driver.find_elements_by_css_selector(fir)

sec= ".gamedb_icon__1jSHy"
# more : 더보기 버튼
more= driver.find_element_by_css_selector(sec)

while True:
    more.click()
    time.sleep(1)
    target= driver.find_elements_by_css_selector(fir)
    if divmod(len(target), 30)[1] != 0 :
        break

names= []
for i in target:
    names.append(i.text)
print(names)

# 종료 단계
driver.close()
print("end...")
```

- 당장 원하는 기능은 구현 했지만 while문의 조건 (list를 30으로 나눴을 때의 나머지가 0이 아니라면) 이 완벽하지 않기에 수정 할 필요는 있어보임.
- 조건을 error가 뜰때까지나 css_selector의 기능이 가능하다면 같은 조건식을 찾아내면 수정하면 흠이 없을거라는 생각중이다.
- ps) 웹페이지에 최종 목록의 수가 명시되어있는데 이걸 이용해도 괜찮을거라는 생각.

![Untitled](/Images/0424_Sellenium_LOLchampions_name/Untitled%204.png)