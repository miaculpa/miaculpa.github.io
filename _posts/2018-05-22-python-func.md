---
layout: post
title:  "[Python 03함수] - 함수"
subtitle:   "함수"
categories: language
tags: python
comments: true
---
## 함수

입력 > function > 출력

```
def 함수명(매개변수[parameters]):
      수행할 문장1
      수행할 문장2
      ...

```

>매개변수(parameter)와 인자(arguments)의 차이
매개변수는 함수에 입력으로 전달된 값을 받는 변수를 의미한다.
ex) 함수 정의에 매개변수를 사용했다.
인자는 함수를 호출할 때 전달하는 입력값을 의미한다.
ex) 함수에 인자를 전달했다.


함수에서 리턴해 주는 값이 없을 경우, 아무것도 없다는 뜻을 가진 `none` 객체를 얻는다.
ex) print문은 수행할 문장에 해당하는 부분일 뿐이므로 결과값은 없다.

---

위치인자(Positional arguments) | 키워드인자(Keyword arguments)
---|---
매개변수의 순서대로 인자를 전달하여 사용하는 경우 | 매개변수의 이름을 지정하여 인자로 전달하여 사용하는 경우


---

##### 기본 매개변수 값의 정의 시점
> 기본 매개변수 값은 함수가 정의되는 시점에 계산되어 계속해서 사용된다.
메모리 주소가 같으면 같은 객체이기 때문이다.

```python
>>> def return_list(value, result=None):
...   if result is None:
...     result = []
...   result.append(value)
...   return result
...
>>> return_list('apple')
['apple']
>>> return_list('banana')
['banana']
```

함수가 실행되는 시점에 기본 매개변수값을 계산 하고싶을 때는 위와 같이 써준다.





##### 위치인자, 키워드인자 묶음
함수에 위치인자로 주어진 변수들의 묶음은 `*매개변수명` 으로 사용할 수 있다.
함수에 키워드인자로 주어진 변수들의 묶음은 `**매개변수명` 으로 사용할 수 있다.
```
def print_arg(*arg):
  print(args)
  >튜플 형태를 갖는다.


def print_kwargs(**kwargs):
  print(kwargs)
  >딕셔너리 형태를 갖는다.
```


##### docstring은 주석역할

```python
>>> def print_arg(*args):
  'Print positional arguments' #주석
  print(args)

>>>help(print_args)  #help(함수명) 주석출력
```


##### 함수를 인자로 전달

파이선에서는 함수 역시 객체이므로, 함수에서 인자로 함수를 전달, 실행, 리턴 가능

```python
>>>def func1():
    print('!')
>>>def func2(func1):
    func1()
```

### 함수 안에서 선언된 변수의 효력 범위
>파이썬에서는 각 함수마다 독립정인 스코프(영역)을 가진다.
메인 프로그램의 영역은 global scope이라고 하며, 전역 스코프 내부에서 독립적인 영역을 갖고 있는 경우 local scope라고 한다.
각 영역에 해당하는 데이터들은 locals()함수를 사용해 확인 할 수 있으며, 전역 영역의 데이터들은 globals()함수를 사용한다.


```python
champion = 'Lux'

>>> def change_global_champion():
      global champion
      champion = 'Ahri'
      print('after change_global_champion : {}'.format(champion))

>>> change_global_champion()
>>> print('print global champion : {}'.format(champion))
```
함수는 독립적인 영역이고 전역에서 쓰인 변수를 함수 내부에서 쓰면 전역변수를 무시한다. 따라서 위의 경우, change_global_champion함수는 champion변수에 새로운 값을 대입한다. 만약 로컬스코프에서 글로벌 스코프 변수를 변경해야 한다면 글로벌 영역에 이미 존재하는 값을 사용함을 명시해 주어야 한다.





내부함수에서의 로컬 스코프(nonlocal)
전역 스코프가 아닌, 자신의 바로 바깥 영역의 로컬 스코프(자신보다 한 단계 위의 로컬 스코프)의 데이터를 참조하고자 한다면, nonlocal키워드를 사용한다.

```python

champion champion ==  'Lux'

  def  local1local1():
    champion ():     ch = 'Ahri'
    print('local1 locals() : {}'.format(locals()))

    def local2():
        nonlocal champion
        champion = 'Ezreal'
        print('local2 locals() : {}'.format(locals()))
    local2()
    print('local1 locals() : {}'.format(locals()))

print('global locals() : {}'.format(locals()))
local1()
```


### lambda 함수

```
lambda <매개변수> : <표현식>
```


### 클로져(Closure)

함수가 정의된 환경을 말하며, 파이썬 파일이 여러개일 경우 각 파일은 하나의 모듈역할을 하고, 각 모듈은 독립적인 환경을 가진다.
독립된 환경은 각자의 영역을 전역 영역으로 사용한다.

함수의 전역 영역은 해당 함수가 정의된 모듈의 전역 영역으로, 전역변수는 모듈의 영역에 영향을 받는다.



### 데코레이터 (decorator)
