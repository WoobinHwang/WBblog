---
title: "예외 처리"
author: "woobin"
date: '2022-04-25'
---

# 0425_Exception_Handling

- 에러가 나올 때 처리하는 코드를 찾아냄

```python
try:
	...
except (에러 이름):
	...
```

- 예시 코드

![Untitled](/Images/0425_Exception_Handling/Untitled.png)

![Untitled](/Images/0425_Exception_Handling/Untitled%201.png)

- 에러 메세지도 형식에 맞춰서 넣어야하는데,
에러 메세지는 터미널에서 찾아 넣으면 된다.

![Untitled](/Images/0425_Exception_Handling/Untitled%202.png)

![Untitled](/Images/0425_Exception_Handling/Untitled%203.png)

- 최종 코드

```python
a_li = ["a", "b"]
try:
    print(a_li[2])

except IndexError:
    pass
    print("no!")

print("end...")
```

- ——>셀레니움에 적용시킬 코드

```python
while True:

    try:
        more.click()
        time.sleep(1)
        more= driver.find_element_by_css_selector(sec)
        print(i)
    except Exception as error:
        print("Here!")
        i= "break"
        print(i)
        print(error)
        print("step")
        break
```