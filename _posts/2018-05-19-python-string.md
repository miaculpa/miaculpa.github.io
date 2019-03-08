---
layout: post
title:  "[Python 01자료형] - 문자열 자료형"
subtitle:   "문자열 자료형"
categories: language
tags: python
comments: true
---
>파이썬3에서는 문자열에서 기본적으로 유니코드(Unicode)를 사용하며, 불변(immutable)한 자료형이다.

문자열(string)이란 문자, 단어 등으로 구성된 문자들의 집합이다.
```
"Life is too short, You need Python"
"a"
"123"
```

따옴표로 둘러싸여 있으면 문자열이다.

### 문자열을 만드는 법
#### 1) 큰따옴표 사용
```python
"Hello world"
```

#### 2) 작은따옴표 사용
```python
'Hello world'
```

#### 3) 큰따옴표 3개를 연속으로 사용 (여러줄인 문자열에 사용가능)
```python
"""Hello world
Python is fun"""
```

#### 4) 작은따옴표 3개를 연속으로 사용 (여러줄인 문자열에 사용가능)
```python
'''Hello world
Python is fun'''
```

>문자열 안에 따옴표 포함시킬 때는 사용되지 않은 인용 부호를 사용하거나 백슬래쉬(\)사용
```python
>>> a = "She's beautiful"
>>> b = '"Python is very fun." he says.'
>>> c = "she\'s beautiful
```

### 문자열 연산하기
```python
>>> head = "Python"
>>> tail = "is fun!"
>>> head + tail
'Python is fun!'

>>> a = "Python"
>>> a * 2
'PythonPython'
```

### 문자열 인덱싱과 슬라이싱
인덱싱(Indexing)이란 가리킨다는 의미이고, 슬라이싱(Slicing)은 잘라낸다는 의미이다.

문자열 인덱싱 : `a[번호]`는 문자열 내 특정한 값을 뽑아내는 역할을 한다. 이 것을 인덱싱이라 한다.
```python
>>> a = "You need Python"
>>> a[3]
'u'
>>> a[0]
'Y'
>>> a[4]
' '
>>> a[-5]
'y'
```

문자열 슬라이싱 : 문자열에서 단어를 뽑아내는 방법이다. `a[시작번호:끝번호:(step)]`는 문자열에서 시작번호부터 끝번호까지 step만큼씩 뛰어넘어 문자를 뽑아내는 역할을 힌다.
주의할 점은 끝 번호에 해당하는 것은 포함하지 않는다.
a[0:4]를 수식으로 나타내면 0<= a < 4
```python
>>> a = "You need Python"
>>> a[0:4]
'You'
```
### 문자열 포매팅
문자열 포매팅이란 문자열 내에 어떤 값을 삽입하는 방법이다.

#### 1) 옛스타일(%)
```
>>> a = "apples"
>>> "I eat %s" % (a)
I eat apples
```

**포맷코드** | **설명**
%s | 문자열
%d | 10진수
%x |16진수
%o | 8진수
%f | 10진 부동소수점수
%e | 지수로 나타낸 부동소수점수
%g | 10진 부동소수점수 혹은 지수로 나타낸 부동소수점수
%% | 리터럴 %

예제는 문자열을 대입했으나 이 외에도 다양한 것들을 대입시킬 수 있다.

#### 2) 새 스타일({}, format)
```python
>>>"I eat {} apples".format(3)
'I eat 3 apples'
>>>"I eat {}".format(apples)
'I eat apples'
```

#### 3) f문자열 포매팅
파이썬 3.6버전부터 f문자열 포매팅 기능을 사용할 수 있다. 이전버전에선 사용할 수 없는 기능이므로 주의해야 한다.
문자열 앞에 f접두사를 붙이면 f문자열 포매팅 기능을 사용할 수 있다.

```python
>>> name = '영롱한'
>>> f'내 이름은 {name}입니다.'
'내 이름은 영롱한입니다.'
```

### 문자열과 관련된 함수들

길이 : 내장함수 **len** 사용
```Python
>>> a = 'apples'
>>> len(a)
6
```

문자열 나누기 : 문자열의 내장함수 **split** 을 사용
```python
>>> a = "Life is too short"
>>>a.split( )
['Life','is','too','short']
>>> girlsday = "민아, 유라, 소진, 혜리"
>>> girlsday.split(',')
['민아', '유라', '소진', '혜리']
```

문자열 결합 : 문자열의 내장함수 **join** 을 사용
```python
>>>girlsday_list = girlsday.split(',')
>>>girlsday_str = ','.join(girlsday_list)
>>>print(girlsday_str)
민아, 유라, 소진, 혜리
```
