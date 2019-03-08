---
layout: post
title:  "django로 인스타그램만들기 06 - User object와 authenticate함수, login함수 개념"
subtitle:   ""
categories: django
tags: insta
comments: true
---

reference
- [https://docs.djangoproject.com/en/2.0/topics/auth/default/](https://docs.djangoproject.com/en/2.0/topics/auth/default/) : Django docs -  User authentication

---

### User objects

User objects는 model에 정의한 class와 비슷하지만 다른 특별한 객체입니다.

User objects는 인증 시스템의 핵심이고, 특별한 기능들을 담당합니다.

밑의 다섯개의 field가 기본field입니다.

- username
- password
- email
- first_name
- last_name

---

### Creating users

User는 create가 아닌 created_user로 생성하는데 왜냐하면 password를 해싱된 값으로 저장해야 하기 때문입니다.


``` shell
>>> from django.contrib.auth.models import User

>>> user = User.objects.created_user('ah',
'a@a.com', 'password')
>>> user.save()
```


---

### Changing passwords

`manage.py changepassword *username*` 로 user의 비밀번호를 바꾸는 것이 가능합니다.

왜냐하면 manage.py를 실행할 수 있는 사람은 관리자뿐이기 때문입니다.


python code안에서 바꾸고 싶다면 set_password를 사용합니다.

```shell
>>> from django.contrib.auth.models import User
>>> u = User.objects.get(username='john')
>>> u.set_password('new password')
>>> u.save()
```

---

### Authentication users

authenticate함수는 credential(자격증명집합)을 확인하는 기능을 합니다.

login방식이 페이스북, 카카오톡로그인 등 여러개일 수 있습니다.
이 경우 username으로 user인지 확인이 불가능 합니다. 이 때 특별한 자격증명이 중간에 생겨서
자격증명이 유효한 경우 user객체를 반환해 줍니다.

authenticate에 credential이 인자로 전달 됩니다.

```python
from django.contrib.auth import authenticate
user = authenticate(username='john', password='secret')
if user is not None:
    # A backend authenticated the credentials
else:
    # No backend authenticated the credentials
```


---


### Authentication in Web request

로그인 안한 상태면 AnonymousUser인스턴스가, 로그인한 상태면 User인스턴스가 됩니다.


is_authenticated 속성이 있는데 true/false 로 로그인 상태를 판별할 수 있습니다.

```python
if request.user.is_authenticated:
    # Do something for authenticated users.
    ...
else:
    # Do something for anonymous users.
```

my view에서 request를 인자로 받습니다. authenticate함수 통과 후 if문을 통해
user가 있다면 login을 시켜주고 아니면 다시 login을 하게 하도록 합니다.

login함수는 user에 대한 특수한 키값을 만들어서 장고 db와 쿠키에 넣는 것 까지 해줍니다.

```python
from django.contrib.auth import authenticate, login

def my_view(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(request, username=username, password=password)
    if user is not None:
        login(request, user)
        # Redirect to a success page.
        ...
    else:
        # Return an 'invalid login' error message.
        ...
```
