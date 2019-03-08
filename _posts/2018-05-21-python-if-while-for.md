---
layout: post
title:  "[Python 02제어문] - if문, while문, for문"
subtitle:   "if문 while문 for문"
categories: language
tags: python
comments: true
---
## if문

```
>>> if 조건:
      조건이 참일 경우
    else:
      조건이 거짓일 경우
```

```
>>> if 조건1:
      조건1이 참일 경우
    else:
      조건1이 거짓일 경우
      if 조건2:
        조건1은 거짓이나, 조건2는 참일 경우
      else:
        조건1,2가 모두 거짓일 경우
```

```
>>> if 조건1:
      조건1이 참일 경우
    elif 조건2:
      조건1은 거짓이나, 조건2가 참일 경우
    else:
      조건1,2가 모두 거짓일 경우
```

파이썬에서 if문을 만들 때는 if조건문: 아래 문장부터 속하는 모든 문장에 `들여쓰기(indentation)` 을 해주어야 한다.

조건문 뒤에는 반드시 `콜론(:)` 이 붙는다.


---
## for문

```
for 변수 in 리스트(또는 튜플, 문자열):
  수행할 문장1
  수행할 문장2
```
```python
>>> test_list = ['one', 'two', 'three']
>>> for i in test_list:
        print(i)
one
two
three
```

```
[i for i in range(1, 100) if i%7 == 0 or i%9 ==0]
```

---
## while문

```
while 조건:
  조건이 참일경우 실행
  조건이 거짓이 될 경우까지 계속해서 반복

```
