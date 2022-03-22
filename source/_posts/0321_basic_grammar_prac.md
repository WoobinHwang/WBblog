---
title: "파이썬 기초문법 연습1"
author: "woobin"
date: '2022-03-21'
---

# 논리형 연산자

## 비교 연산자


```python
var = input("입력해주세요.")
print(type(var))
```

    입력해주세요.1
    <class 'str'>
    


```python
var = int("1")
print(type(var))
```

    <class 'int'>
    


```python
var = int(input("숫자를 입력해주세요."))
print(type(var))
```

    숫자를 입력해주세요.1
    <class 'int'>
    


```python
num1 = int(input("숫자를 입력하세요."))
num2 = int(input("숫자를 입력하세요."))

var = num1 >= num2
print(var)
```

    숫자를 입력하세요.5
    숫자를 입력하세요.3
    True
    


```python
greeting = "Hello kaggle!"
print(greeting[10])
```

    l
    

# 슬라이싱

## - 범위를 지정하고 데이터를 가져온다.


```python
greeting = "Hello kaggle!"

print(greeting[:])
print(greeting[6:])
print(greeting[:5])
print(greeting[3:9])
print(greeting[0:9:2])
print(greeting[13])
```

    Hello kaggle!
    kaggle!
    Hello
    lo kag
    Hlokg
    

# 문자열 포매팅

## 바로 대입


```python
print("I eat %d apples." %3)
'I eat %s apples.' %"two"
```

    I eat 3 apples.
    




    'I eat two apples.'



## 변수 대입


```python
num1 = 4
print("I have %d apples." %num1)

num2 = "three"
print("I have %s balls" %num2)
```

    I have 4 apples.
    I have three balls
    

## 두개의 변수 대입


```python
num1 = 3; num2 = "four"
print("i love %d, and hate %s" %(num1, num2))
```

    i love 3, and hate four
    

## 종류

코드	설명
%s	문자열(String)
%c	문자 1개(character)
%d	정수(Integer)
%f	부동소수(floating-point)
%o	8진수
%x	16진수
%%	Literal % (문자 % 자체)

## f 문자열 포매팅


```python
name = "홍길동"; age = 27 
print(f"제 이름은 {name}, 나이는 {age}세 입니다.")
f"제 이름은 {name}, 내년엔 {age+1}세 입니다."
```

    제 이름은 홍길동, 나이는 27세 입니다.
    




    '제 이름은 홍길동, 내년엔 28세 입니다.'




```python
print(f'{"hi":<10}')  # 왼쪽 정렬
print(f'{"hi":>8}')  # 오른쪽 정렬
print(f'{"hi":^10}')  # 가운데 정렬
print(f'{"hi":=<3}')  # 왼쪽 정렬
print(f'{"hi":->10}')  # 오른쪽 정렬
print(f'{"hi":!^6}')  # 가운데 정렬
```

    hi        
          hi
        hi    
    hi=
    --------hi
    !!hi!!
    

### 확인용 코드


```python
print(f'{"hi":!<4}')
print(("hello"[1]))
```

    hi!!
    e
    

## 소수점 나타내기


```python
pi = 3.141592
print(f'{pi:0.4f}')  # pi의 소수점 4자리까지 표시
print(f'{pi:7.3f}')  # pi의 소수점 3자리까지, 총 7자리로 맞추기
```

    3.1416
      3.142
    

# 리스트
- 시퀀스 데이터 타입
- 데이터에 순서가 존재하냐! 슬라이싱이 가능해야함.
- 대괄호['값1', '값2', '값3', ...]


```python
a = []  # 빈 리스트 생성
a_func = list()  # 빈 리스트 생성
b = [1]  # 숫자가 요소가 될 수 있다.
c = ['apple']  # 문자열도 요소가 될 수 있다.
d = [1, 2, ['apple']]  # 리스트 안에 또 다른 리스트를 요소로 넣을 수 있다.

print(a)
print(a_func)
print(b)
print(c)
print(d)

print(type(d))
```

    []
    []
    [1]
    ['apple']
    [1, 2, ['apple']]
    <class 'list'>
    

## 리스트 슬라이싱


```python
a = [1, 2, 3, 4, 5, 6, 7, 8]
print(a[0])
print(a[3:])
print(a[:3])
print(a[1:3])
print(a[1:3])
print(a[1:6:2])
```

    1
    [4, 5, 6, 7, 8]
    [1, 2, 3]
    [2, 3]
    [2, 3]
    [2, 4, 6]
    


```python
a = [["apple", "banana", "cherry"], 1]
print(a[0][1])
print(a)
print(a[0][1][-2])
print(a[0][2][2])
```

    banana
    [['apple', 'banana', 'cherry'], 1]
    n
    e
    


```python
a = [1, 2, 3, 4, 5, 6, 7, 8]
print(a[::-1])  # 역순
print(a[::2])
```

    [8, 7, 6, 5, 4, 3, 2, 1]
    [1, 3, 5, 7]
    

## 리스트 연산자


```python
a = ["john", "evan"]
b = ["alice", "eva"]

c = a+ b
print(c)
```

    ['john', 'evan', 'alice', 'eva']
    


```python
c = a * 3
d = b * 0

print("a * 3 = ", c)
print("b * 0 = ", d)
```

    a * 3 =  ['john', 'evan', 'john', 'evan', 'john', 'evan']
    b * 0 =  []
    

## 리스트 수정 및 삭제


```python
a = [0, 1, 2]
a[1] = "b"
print(a)
```

    [0, 'b', 2]
    

### 리트스 값 추가하기


```python
a = [100, 200, 300]
a.append(400)
print(a)

# a.append([500, 600])
# print(a)

a.extend([500, 600])
print(a)
```

    [100, 200, 300, 400]
    [100, 200, 300, 400, 500, 600]
    


```python
a = [0, 1, 2]
# a.insert(인덱스번호, 넣고자하는값)
a.insert(1, 100)
print(a)
```

    [0, 100, 1, 2]
    

### 리스트 값 삭제하기


```python
a = [4, 3, 2, 1, "A"]
a.remove(1)  # 리스트 안에 있는 값을 삭제
print(a)
a.remove("A")
print(a)
```

    [4, 3, 2, 'A']
    [4, 3, 2]
    


```python
a = [1, 2, 3, 4, 5, 6, 7, 8]

del a[1]  # 인덱스 번호
print(a)

del a[1:5]
print(a)
```

    [1, 3, 4, 5, 6, 7, 8]
    [1, 7, 8]
    


```python
b = ["a", "b", "c", "d"]
x = b.pop()
print(x)
print(b)
```

    d
    ['a', 'b', 'c']
    

### 그 외 메서드


```python
a = [0, 1, 2, 3]
print(a)

a.clear()
print(a)
```

    [0, 1, 2, 3]
    []
    


```python
a = ["a", "b", "c", "d"]
print(a.index("a"))
print(a.index("b"))
print(a.index("c"))
```

    0
    1
    2
    


```python
a = [1, 4, 5, 2, 3]
b = [1, 4, 5, 2, 3]

a.sort()
print(a)

# 내림차순, sort
b.sort(reverse=True)
print("sort(reverse=True): ", b)
```

    [1, 2, 3, 4, 5]
    sort(reverse=True):  [5, 4, 3, 2, 1]
    


```python
# c = [4, 3, 2, 1, "a"]
# c.sort()  # 문자형이 다르기떄문에 에러가 남.
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-177-4435d5800da3> in <module>()
          1 c = [4, 3, 2, 1, "a"]
    ----> 2 c.sort()
    

    TypeError: '<' not supported between instances of 'str' and 'int'


# 튜플
- List와 비슷하다.
- 비슷한점 : 슬라이싱, 인덱싱 등등
- (vs 리스트)다른점: 튜플은 수정 삭제가 안된다.



```python
tuple1 = (0)  # 끝에 콤마(,)를 붙이지 않을 때 => int
tuple2 = (0,)  # 끝에 콤마를 붙일 때 => tuple
tuple3 = 0, 1, 2
print(type(tuple1))
print(type(tuple2))
print(type(tuple3))
```

    <class 'int'>
    <class 'tuple'>
    <class 'tuple'>
    


```python
a = (0, 1, 2, 3, "a")
print(type(a))

b = list(a)
print(b)
b[1] = "b"

a= tuple(b)
print(a)
# a[4] = 41
# del a[4]
```

    <class 'tuple'>
    [0, 1, 2, 3, 'a']
    (0, 'b', 2, 3, 'a')
    

## 튜플 인덱싱 및 슬라이싱 하기


```python
a = (1, 2, 3, 4, "a")
print(a[1])
print(a[3])
print(a[4])
```

    2
    4
    a
    

## 더하기 곱셈 연산자 사용


```python
a = ("홍길동", "임꺽정")
b = ("철수", "영희")
c = (1, 2)

print(a+b)
print(a*2)
print(b*0)
print(c*2)
```

    ('홍길동', '임꺽정', '철수', '영희')
    ('홍길동', '임꺽정', '홍길동', '임꺽정')
    ()
    (1, 2, 1, 2)
    

## 딕셔너리
- key - value 값으로 나뉨


```python
dict_01 = {"teacher" : "evan",
           "class" : 604,
           "students" : 24,
           "학생이름" : ["A", "B"],}
print(dict_01)
print(dict_01["teacher"])
print(dict_01["학생이름"])
```

    {'teacher': 'evan', 'class': 604, 'students': 24, '학생이름': ['A', 'B']}
    evan
    ['A', 'B']
    


```python
print(type(dict_01.keys()))
print(type(dict_01.values()))
print(list(dict_01.keys()))
print(list(dict_01.values()))
```

    <class 'dict_keys'>
    <class 'dict_values'>
    ['teacher', 'class', 'students', '학생이름']
    ['evan', 604, 24, ['A', 'B']]
    


```python
dict_01.items()  # 개별 key,value는 튜플 상태로 저장되어있음.
```




    dict_items([('teacher', 'evan'), ('class', 604), ('students', 24), ('학생이름', ['A', 'B'])])




```python
print(dict_01.get("teacher", "값 없음"))
print(dict_01.get("선생님", "값 없음"))
print(dict_01.get("class"))
```

    evan
    값 없음
    604
    

# 조건문 & 반복문
- 일상에서의 조건문

## 조건문


```python
weather = "맑음"
if weather == "비":
  print("우산을 가져간다.")
else :
  print("우산을 가져가지 않는다.")
```

    우산을 가져가지 않는다.
    

- 등급표 만들기
- 60점 이상 합격/ 불합격


```python
result = int(input())
if result >= 60:
  print("합격입니다.")
else :
  print("불합격입니다.")
```

    59
    불합격입니다.
    


```python
import random
score = random.randrange(50,101)
print(str(score) + "점")

if score >= 90:
  print("A등급입니다")
elif score >= 80:
  print("B등급입니다")
else :
  print("F등급입니다.")
```

    90점
    A등급입니다
    

## 반복문
- 안녕하세요 10번 반복하세요


```python
for i in range(10) :
  print("안녕하세요", i+1)
```

    안녕하세요 1
    안녕하세요 2
    안녕하세요 3
    안녕하세요 4
    안녕하세요 5
    안녕하세요 6
    안녕하세요 7
    안녕하세요 8
    안녕하세요 9
    안녕하세요 10
    


```python
count = range(50)
print(count)

for n in count:
  print(str(n + 1) + "번째")
  if (n + 1 ) == 5:
    print("그만합시다!")
    break
  print("도전")
```

    range(0, 50)
    1번째
    도전
    2번째
    도전
    3번째
    도전
    4번째
    도전
    5번째
    그만합시다!
    


```python
a = "hello"

for x in a:
  print(x)
  if x == "l":
    break
```

    h
    e
    l
    


```python
alphabets = ["a", "b", "c"]
for index, value in enumerate(alphabets):
  print(index, value)

# enumerate() 는 index 와 밸류값을 호출
```

    0 a
    1 b
    2 c
    

# 종합문제 Q1
- 문자열 바꾸기


```python
test = "a:b:c:d"
test = test.split(":")
print(test)
test = "#".join(test)
print(test)
```

    ['a', 'b', 'c', 'd']
    a#b#c#d
    

# 종합문제 Q2
- 딕셔너리값 추출하기


```python
a = {'A':90, 'B':80}
print(a.get("C", "70"))
```

    70
    

# 종합문제 Q3
- extend의 특징


```python
주소값이 변하지 않는것을 확인 할 수 있다.
```
