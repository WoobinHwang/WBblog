---
title: "파이썬 기초문법 연습3"
author: "woobin"
date: '2022-03-22'
---

# 클래스를 만드는 목적
- 코드의 간결화!
  + 코드를 재사용!
- 여러 라이브러리를 사용 --> 클래스로 구현이 됨
  + list 클래스, str 클래스
  + 객체로 씀
  + 변수명으로 정의!
- 여러 클래스들이 모여서 하나의 라이브러리가 됨.
  + 장고 / 웹개발 / 머신러닝 / 시각화 / 데이터 전처리


```python
class Person:

  # class attribute
  country = "korean"

  #instance attribute
  def __init__(self, name, age):
    self.name = name
    self.age = age

if __name__ == "__main__":
  kim = Person("Kim", 27)
  Lee = Person("Lee", 25)

  # access class attribute
  print("Kim은 {}".format(kim.__class__.country))
  print("Lee는 {}".format(Lee.__class__.country))
```

    Kim은 korean
    Lee는 korean
    

## instance 메서드 생성
- list.append(), list.extend()


```python
class Person:

  # class attribute
  country = "korean"

  # instance attribute
  def __init__(self, name, age):
    self.name = name
    self.age = age

  # instance method 정의
  def sining(self, songtitle, sales):
    return "{} 판매량 {} 된 {}을 노래합니다.".format(self.name, sales, songtitle)


if __name__ == "__main__":
  Kim = Person("Kim", 27)
  Lee = Person("Lee", 25)

  # access class attribute
  print("Kim은 {}".format(kim.__class__.country))
  print("Lee는 {}".format(Lee.__class__.country))

  # call instance method
  print(Kim.sining("좋은 날", 10))
  print(Lee.sining("나쁜 날", 99))
```

    Kim은 korean
    Lee는 korean
    Kim 판매량 10 된 좋은 날을 노래합니다.
    Lee 판매량 99 된 나쁜 날을 노래합니다.
    

## 클래스 상속
- ex) 부모님 유산...
  + 부모님 집 (냉자고, 세탁기, TV, etc) # instance method, attribute
  + 사용은 같이 함
- 돈을 모음
  + 개인 노트북 구매(내 방에 구비) # 자식 클래스 instance method, attribute 추가 확장
  + 노트북은 내 것이지만, 가전제품을 구매해서 확장시켰다!


```python
class Parent:
  
  # instance attribute
  def __init__(self, name, age):
    self.name = name
    self.age = age
  
  # instance method 정의
  def whoAmI(self):
    print("I am Parent!!")

  def singing(self, songtitle):
    return "{} {}을 노래합니다.".format(self.name, songtitle)

  def dancing(self):
    return "{} 현재 춤을 춥니다.".format(self.name)

class Child(Parent):
  def __init__(self, name, age):
    # super() function
    super().__init__(name, age)
    print("Child Class is ON")

  def whoAmI(self):
    print("I am Child")

  def studying(self):
    print("I am Fast Runner")

if __name__ == "__main__":
   child_kim = Child("kim", 15)
   parent_kim = Parent("kim", 45)
   print(child_kim.dancing())
   print(child_kim.singing("연애"))
   # print(parent_kim.studying())
   child_kim.whoAmI()
   parent_kim.whoAmI()
```

    Child Class is ON
    kim 현재 춤을 춥니다.
    kim 연애을 노래합니다.
    I am Child
    I am Parent!!
    


```python
class TV:

  # init constructor
  def __init__(self):
    self.__maxprice = 500

  def sell(self):
    print("Selling Price: {}".format(self.__maxprice))

  def setMaxPrice(self, price):
    self.__maxprice = price

if __name__ == "__main__":
  tv = TV()
  tv.sell()

  # change price
  # 안바뀌는 코드의 예시
  tv.__maxprice = 1000
  tv.sell()

  # setMaxPrice
  # 값을 바꿀 수 있다?! 외부의 입력값을 업데이트 할 수 있다.
  tv.setMaxPrice(1000)
  tv.sell()
```

    Selling Price: 500
    Selling Price: 500
    Selling Price: 1000
    

## 클래스 내부에 조건문
- init constructor


```python
class Employee:

  # init constructor
  def __init__(self, name, salary):
    self.name = name
    
    # 조건문 추가
    if salary >= 0 :
      self.salary = salary
    else:
      self.salary = 0
      print("급여는 0원이 될 수 없다!. 다시 입력하십시요!")

  def update_salary(self, amount):
    # self.salary = self.salary+ amount
    self.salary += amount

  def weekly_salary(self):
    return self.salary / 7

if __name__ == "__main__":
  emp01 = Employee("Woobin", -50000)
  print(emp01.name)
  print(emp01.salary)
  emp01.salary = emp01.salary + 1500
  print(emp01.salary)
  emp01.update_salary(3000)
  print(emp01.salary)
  week_salary = emp01.weekly_salary()
  print(week_salary)
```

    급여는 0원이 될 수 없다!. 다시 입력하십시요!
    Woobin
    0
    

## 클래스 Docstring


```python
class Person:
  """
  사람을 표현하는 클래스

  ...

  Attributes
  -----------
  name : str
    name of the person

  age : int
    age if the person

  Methods
  --------

  info(additional=""): 
    prints the person's name and age

  """

  def __init__(self, name, age):
    """
    Constructs all the neccessary attributes for the person object

    Parameters
    ----------
    name : str
      name of the person

    age : int
      age if the person

    """

    self.name = name
    self.age = age

  def info(self, additional=None):
    """
    ~~~

    Parameters
    ---------
      additional = str, optional
        more info to be displayed (Default is None)  / A, B, C


    Returns
    -------
    None

    """

    print(f'My name is {self.name}. I am {self.age} years old.'+ additional)

if __name__ == "__main__":
  Person = Person("Woobin", age = 27)
  Person.info("직장은 없음")
  help(Person)
```

    My name is Woobin. I am 27 years old.직장은 없음
    Help on Person in module __main__ object:
    
    class Person(builtins.object)
     |  Person(name, age)
     |  
     |  사람을 표현하는 클래스
     |  
     |  ...
     |  
     |  Attributes
     |  -----------
     |  name : str
     |    name of the person
     |  
     |  age : int
     |    age if the person
     |  
     |  Methods
     |  --------
     |  
     |  info(additional=""): 
     |    prints the person's name and age
     |  
     |  Methods defined here:
     |  
     |  __init__(self, name, age)
     |      Constructs all the neccessary attributes for the person object
     |      
     |      Parameters
     |      ----------
     |      name : str
     |        name of the person
     |      
     |      age : int
     |        age if the person
     |  
     |  info(self, additional=None)
     |      ~~~
     |      
     |      Parameters
     |      ---------
     |        additional = str, optional
     |          more info to be displayed (Default is None)  / A, B, C
     |      
     |      
     |      Returns
     |      -------
     |      None
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
    
    

- 클래스
  - 객체를 만들어내기 위한 설계도 혹은 틀
  - 연관되어 있는 변수와 메서드의 집합

- 객체
  - 소프트웨어 세계에 구현할 대상
  - 클래스에 선언된 모양 그대로 생성된 실체
  - '클래스의 인스턴스'라고도 부른다
  - 객체는 모든 인스턴스를 대표하는 포괄적인 의미를 갖는다.

- 인스턴스
  - 설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실체
    - 즉, 객체를 소프트웨어에 실체화 하면 그것을 '인스턴스'라고 부른다.
    - 실체화된 인스턴스는 메모리에 할당된다.
    - 인스턴스는 객체에 포함된다고 볼 수 있다.
    - 객체는 클래스의 인스턴스이다.

# 객체와 인스턴스의 차이

클래스로 만든 객체를 인스턴스라고도 한다. 그렇다면 객체와 인스턴스의 차이는 무엇일까? 이렇게 생각해 보자. a = Cookie() 이렇게 만든 a는 객체이다. 그리고 a 객체는 Cookie의 인스턴스이다. 즉 인스턴스라는 말은 특정 객체(a)가 어떤 클래스(Cookie)의 객체인지를 관계 위주로 설명할 때 사용한다. "a는 인스턴스"보다는 "a는 객체"라는 표현이 어울리며 "a는 Cookie의 객체"보다는 "a는 Cookie의 인스턴스"라는 표현이 훨씬 잘 어울린다.
