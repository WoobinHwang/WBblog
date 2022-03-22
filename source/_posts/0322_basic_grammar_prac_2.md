---
title: "파이썬 기초문법 연습2"
author: "woobin"
date: '2022-03-22'
---

# 기초 문법 리뷰


```python
# 리스트
book_list = ["A", "B", "C"]
# append, extend, insert, remove, pop, etc

# 튜플
book_tuple = ("A", "B",, "C")
# 수정 삭제가 불가능

# 딕셔너리
book_dictionary = {"책 제목" : ["A", "B"], "출판년도" : [2011, 2002]}
#keys(), values(), items(), get()
```

# 조건문 & 반복문


```python
# 조건문
if True :
  print("코드 실행")
elif True :
  pirnt("코드 실행")
else :
  pirnt("코드 실행")
```

    코드 실행
    


```python
# 반복문
for i in range (3):
  print(i+1, "안녕하세요")
```

    1 안녕하세요
    2 안녕하세요
    3 안녕하세요
    


```python
book_list = ["프로그래밍 R", "혼자 공부하는 머신러닝"]
for book in book_list:
  print(book)
```

    프로그래밍 R
    혼자 공부하는 머신러닝
    


```python
strings01 = "Hello world"
for char in strings01:
  print(char)
```

    H
    e
    l
    l
    o
     
    w
    o
    r
    l
    d
    


```python
num_tuple = (1, 2, 3, 4)
for num in num_tuple:
  print(num)
```

    1
    2
    3
    4
    


```python
num_dict = {"A": 1, "B": 2}
for num in num_dict:
  # print(num)
  print(num, num_dict[num])
```

    A 1
    B 2
    

# 반복문의 필요성


```python
product = ["요구르트", "우유"]
price = [1000, 1500]
quantities = [5, 3]

name = product[0]
sales = price[0] * quantities[0]
print(name + "의 매출액은 " + str(sales) + "원 입니다.")

name = product[1]
sales = price[1] * quantities[1]
print(name + "의 매출액은 " + str(sales) + "원 입니다.")
```

    요구르트의 매출액은 5000원 입니다.
    우유의 매출액은 4500원 입니다.
    


```python
product = ["요구르트", "우유"]
price = [1000, 1500]
quantities = [5, 3]

for i in range(2) :
  name = product[i]
  sales = price[i] * quantities[i]
  print(name + "의 매출액은 "+ str(sales) + "원 입니다.")

for i in range(len(product)) :
  name = product[i]
  sales = price[i] * quantities[i]
  print(name + "의 매출액은 "+ str(sales) + "원 입니다.")
```

    요구르트의 매출액은 5000원 입니다.
    우유의 매출액은 4500원 입니다.
    요구르트의 매출액은 5000원 입니다.
    우유의 매출액은 4500원 입니다.
    

## while
- 조건식이 들어간 반복문 
- if) for-loop: 정해진 범위 내에서 작동


```python
# 증가하는 식
count = 1
while count <= 5:
  print(count)
  count = count + 1

print("5를 초과했군요...")
```

    1
    2
    3
    4
    5
    5를 초과했군요...
    


```python
# 감소하는 식
count = 3
while count > 0:
  print(count)
  count = count - 1

print("0 미만이군요...")
```

    3
    2
    1
    0 미만이군요...
    

# 사용자 정의 함수 (user-Defined Function)
- why?

# 클래스(class)를 왜 쓸까
- 코드의 반복성을 줄이기 위해서 사용!

if) len() --> 누군가가 만들었고, 우리는 그걸 쓰는거임.
- 리스트의 길이 구할 때 씀.
- 리스트의 전체 길이를 구하겠다. --> 1회성? 범용성?




```python
# 형식
def 함수명():
  # 코드 실행
  return 값

함수명()
```


```python
def add(a, b):
  c = a + b
  return c
add(1,2)

def abs(a):
  if a < 0:
    a = -a
    return a
  else:
    return a
abs(-5)

def squared():
  a = int(input())
  a = a * a
  return a
print(add(1,2))
print(abs(-5))
print(squared())
```

    3
    5
    3
    9
    

basic.py로 저장할때 예시


```python
# /user/local/bin/python
# -*- coding : utf-8 -*-

def temp(content, letter):
  """content안에 있는 문자를 세는 함수
  
  Args:
    content(str): 탐색 문자열
    letter(str) : 찾을 문자열

  Return:
    int
  
  """
  print("함수 테스트")

  cnt = len([char for char in content if char == letter])
  return cnt

if __name__ == "__main__":
  # help(temp)
  docstring = temp.__doc__
  print(docstring)
```

    content안에 있는 문자를 세는 함수
      
      Args:
        content(str): 탐색 문자열
        letter(str) : 찾을 문자열
    
      Return:
        int
      
      
    

## 리스트 컴프리헨션
- for-loop 반복문을 한줄로 처리


```python
# 풀어서 쓴 것
my_list = [[10], [20, 30]]
# 목표 : [10, 20, 30]

flattened_list = []
for value_list in my_list:
  for value in value_list:
    flattened_list.append(value)
print(flattened_list)
```

    [10, 20, 30]
    


```python
# 한줄로 줄이기
my_list = [[10], [20, 30]]
flattened_list = [value for value_list in my_list for value in value_list]
print(flattened_list)
```

    [10, 20, 30]
    

###  예시


```python
letters = []
for char in "hello world":
  letters.append(char)
print(letters)
```

    ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
    


```python
letters2 = [char for char in "hello world"]
print(letters2)
```

    ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
    


```python
value_list = [1, 2, 3, 4, 5, 6]
print("평균: "+ str(sum(value_list) / len(value_list)))

midpoint = int(len(value_list) / 2)
# len(value_list) / 2 == 0 :
print((value_list[midpoint - 1] + value_list[midpoint]))
print((value_list[midpoint - 1] + value_list[midpoint]) / 2)
print(midpoint)
```

    평균: 3.5
    7
    3.5
    3
    


```python
def mean_and_midian(value_list):
  """숫자 리스트의 요소들의 평균과 중간값을 구하는 코드를 작성해라
  Args:
    value_list (iterable of int / float) : A list of int numbers

  Returns:
    tuple(float, float)
  """
  # 평균
  mean = sum(value_list) / len(value_list)

  # 중간값
  midpoint = int(len(value_list) / 2)
  if len(value_list) / 2 == 0:
    median = (value_list[midpoint - 1] + value_list[midpoint]) / 2 
  else:
    median = value_list[midpoint]
  
  return mean, median


if __name__ == "__main__":
  value_list = [1, 1, 2, 2, 3, 4, 5]
  avg, median = mean_and_midian(value_list)
  print("avg:", avg)
  print("median", median)
```

    avg: 2.5714285714285716
    median 2
    

## 그 외
- 데코레이터, 변수명 immutable, mutable, contex manager

# 개별 연습

## 팩토리얼 구현하기


```python
def factorial():
  print("a를 입력하세요")
  a = int(input())
  print(type(a))
  result = 1
  for num in range(a):
    num = num + 1
    result = result * num

  return result
factorial()
```

    a를 입력하세요
    3
    <class 'int'>
    




    6


