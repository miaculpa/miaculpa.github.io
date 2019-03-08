---
layout: post
title:  "[Python 01자료형] - 리스트 자료형과 튜플 자료형"
subtitle:   "리스트 자료형과 튜플 자료형"
categories: language
tags: python
comments: true
---

## 리스트

>파이썬에 내장된 시퀀스 타입에는 문자열, 리스트, 튜플이 있다.
문자열은 잉용부호(작은따옴표, 큰따옴표)를 사용하며, 리스트는 대괄호[], 튜플은 괄호()를 사용하여 나타낸다.
시퀀스 타입의 객체는 인덱스 연산을 통해 내부 항목에 접근 할 수 있다.

```
>>> empty_list1 = []
>>> empty_list2 = list()
>>> empty_list3 = ['a', 'b', 'c']
```

다른 데이터를 리스트로 변환가능
```
>>>list('Hello world')
['H', 'e', 'l', 'l', 'o' ' ', 'w', 'o', 'r', 'l', 'd']
```

슬라이스 연산, 인덱스 연산
```python
>>> sample_list = ['Jan', 'Feb', 'Mar', 'Apr', 'May']

>>> sample_list[0::2]
['Jan', 'Mar', 'May']

>>> sample_list[1] = 'hi'
>>> sample_list
['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
```



### 리스트 관련 함수들

##### 1. 리스트에 요소 추가(append)

```python
>>> sample_list = ['Jan', 'Feb', 'Mar', 'Apr', 'May']
>>> sample_list.append('Jun')
['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']

```

##### 2. 리스트 병함(extend, +=)


```python
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']

>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits += colors
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

##### 3. 특정 위치에 리스트 항목 추가(insert)
insert(a, b)는 리스트의 a번째 위치에 b를 삽입하는 함수이다.

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> frutis.insert(0, 'mango')
>>> fruits
['mango', 'apple', 'banana', 'melon']
```


##### 4. 리스트 항목 삭제(del, remove)
del은 리스트 함수가 아닌 파이썬 구문이므로 del <리스트>[오프셋]형식

```python
>>> del fruits[0]
>>> fruits
['apple', 'banana', 'melon']
```
값으로도 리스트 항목을 삭제할 수 있다.

```python
>>> fruits.remove('mango')
>>> fruits
['apple', 'banana', 'melon']
```

pop()은 리스트의 맨 마지막 요소를 돌려 주고 그 요소는 삭제하는 함수이다.

```python
>>> a = [1,2,3]
>>> a.pop()
3
>>> a
[1, 2]
```

##### 5. 값으로 리스트 항목 오프셋 찾기(index)

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> fruits.index('apple')
0
```

##### 6. 존재 여부 확인(in)

```python
>> 'apple' in fruits
True
```

##### 7. 값 세기(count)

```python
>>>fruits.append('apple')
>>>fruits.append('apple')
>>>fruits.count('apple')
3
```

##### 8. 리스트 정렬(sort)

```python
>>> a = [1,4,3,2]
>>>a.sort()
>>>a
[1,2,3,4]
```

---
## 튜플
>튜플은 리스트와 비슷하다. 하지만 정의 후 내부 항목의 삭제나 수정이 불가능하다.
리스트는[]으로 둘러싸지만 튜플은 ()로 둘러싼다.

```python
>>>empty_tuple()
>>>colors = 'red',
```
튜플을 정의할 때는 괄호가 없어도 무관하다. 또한 튜플의 요소가 1개일 때는 요소의 뒤에 쉼표(,)를 붙여야 한다.
