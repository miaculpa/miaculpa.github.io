---
layout: post
title:  "[Python 05클래스] - 클래스"
subtitle:   "클래스"
categories: language
tags: python
comments: true
---
## 클래스
> 클래스는 객체를 만들기 위한 틀이다.
클래스정의는 대문자로 구분하고 '\_'를 쓰지 않는다.

### 객체지향 프로그래밍
>파이썬의 모든 것은 객체이며, 객체를 사용할 때는 변수에 해당 객체를 참조(reference)시켜 사용한다.

```python
>>>class Shop:
      def __init__(self, name):
        self.name = name
```
\_\_init\_\_ 은 클래스를 사용한 객체의 초기화 메서드이다. 매직 메서드라고 한다.


```python
>>> lotteria = Shop('롯데리아')
```

롯데리아는 객체이고, Shop 클래스형 인스턴스이다.


**클래스 속성**

```python
>>>class Shop:
      description = 'Pyhon Shop class'
      def __init__(self, name):
        self.name = name
```

어떤 하나의 클래스로부터 생성된 객체들이 같은 값을 가지게 하고 싶을 경우 클래스 속성을 사용한다.
마찬가지로 공통된 메서드를 사용하게 하고 싶을 경우 클래스 메서드를 사용한다.

### 메서드
인스턴스 메서드의 첫번째 인수는 항상 self이다. self에는 메서드를 호출하는 인스턴스 자신이 자동으로 전달된다.

### 속성 접근 지정자(attribute access modifier)
속성 이름을 \_\_로 시작하면, 외부에서의 접근을 제한한다. 이 경우를 private 지정자라고 한다.

의미로 구분하는 속성 접근 지정자
* private : 외부에서의 접근이 불가능
* protected : 상속받은 자식 클래스에서는 접근 가능
* public : 외부 접근 가능

### 상속(Inheritance)
추가적인 기능이 필요할 경우, 기존의 클래스를 상속받은 새 클래스를 사용한다.
상속되는 클래스를 부모클래스라고 하며, 상속을 받는 클래스는 자식클래스라고 한다.

```python
class Restaurant(Shop):
  pass
```

**부모 클래스의 메서드 호출**

```python
class Restaurant(Shop):
  def __init__(self, name, shop_type, address, rating):
      super().__init__(name, shop_type, address)
      self.rating = rating
```

### 다형성 동적 바인딩
* 다형성 : 하나의 코드가 여러 역할을 한다. ex) str()
* 동적 바인딩 : 타입을 신경 쓰지 않고 인스턴스를 사용할 수 있게 해주는 방식.  
            동적으로 속성을 바인딩하는 과정은 오로지 해당 객체가 해당하는 속성을 가졌는지만 검사한다.

---
### 실습
1.일상생활에서 접할 수 있는 서로 연관되는 어떠한 것들에 대하여 3개의 클래스를 만들고, 각각에게 영향을 줄 수 있는 메서드를 만들고 사용해서 다른 인스턴스의 속성에 영향을 주는 코드를 작성해본다.
ex) 사람 클래스와 고양이 클래스 -> 사람이 '먹이준다' 메서드 실행 시 고양이 인스턴스를 전달, 고양이 인스턴스의 포만감을 +

```
class Human:
    def __init__(self, name):
        self.name = name


    def feed(self, cat):
        cat.fulness += 10
        print(f'{cat.name}의 포만감이 {cat.fulness}만큼 증가')

class Cat:
    def __init__(self, name):
        self.name = name
        self.fulness = 0

    def eat(self, food):
        food.feed(self)
```

```
ahyeon = Human('아현')
rao = Cat('라오')
```

```
>>>ahyeon.feed(rao)
라오의 포만감이 10만큼 증가
```

2.외부에서 조작하면 문제가 생길 수 있는 속성을 private하게 지정되도록 이름을 바꾸고, property를 만들어 본다. setter가 필요없는 속성은 읽기전용으로 남겨두며, 변경해야 하는 속성은 setter를 구현하고 제한조건을 만든다
ex) 음식을 먹고 포만감을 늘리는 메서드 -> 포만감이 100이상이면 먹지 않도록

```

```
