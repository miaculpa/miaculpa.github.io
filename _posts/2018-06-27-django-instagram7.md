---
layout: post
title:  "django로 인스타그램만들기 07 - login_view 구현"
subtitle:   ""
categories: django
tags: insta
comments: true
---

이제 코드를 작성해 봅시다.

그런데 아직 user에 관련된 application을 만들지 않았습니다.

`python manage.py startapp members`로 새로운 application을 만들어 줍니다.

INSTALLED_APPS에 members를 추가해 줍니다.

customize하기 전에 기본 user를 사용해 볼 것이기 때문에 아직 model은 작성할 필요 없습니다.

{% raw %}


### members.urls를 include, path구현('members/login/')
`config/urls.py`에 members의 url을 include 해줍니다.

 `path('members/', include('members.urls')),`


`members/urls.py`

```python
from django.urls import path

from . import views

urlpatterns = [
    path('login/', views.login_view, name='login')
]
```

---

### `templates/members/login.html`에 form을 구성

```html
{% extends 'base.html' %}


{% block content %}

<div>
    <form action="" method="POST">
        <input type="text" name="username">
        <input type="text" name="password">
        <button type="submit">로그인</button>
    </form>
</div>


{% endblock %}
```

---

### login_view구현

`members/views.py`

```python
from django.shortcuts import render


def login_view(request):
    print(request.POST)
    return render(request, 'members/login.html')
```
request.POST를 프린트 함수에 넣고 runserver를 실행해 보면 request.POST는 QueryDict라는 걸 알 수 있습니다.

```shell
<QueryDict: {'csrfmiddlewaretoken': ['utAiWOnc8KQNaszzREJnfAueiz7y2b7mhyi55a9C7opNheYBOMF35xGKbIiorXux'], 'username': ['ah'], 'password': ['1234']}>
```


다음과 같이 login_view를 작성합니다.

```python
from django.contrib.auth import authenticate, login
from django.shortcuts import render, redirect


def login_view(request):
    # print(request.POST)
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        # 받은 username과 password에 해당하는 User가 있는지 인증
        user = authenticate(request, username=username, password=password)

        # 인증에 성공하면 post:post-list로 이동
        if user is not None:
            # 세션값을 만들어 DB에 저장하고, HTTP response의 Cookie에 해당값을 담아보내도록 하는
            # login()함수를 실행한
            login(request, user)
            return redirect('posts:post-list')
        # 인증에 실패한 경우 다시 members:login으로 이동
        else:
            # 다시 로그인 페이지로 redirect
            return redirect('members:login-view')
    # GET 요청일 경우
    else:
        # form이 있는 template을 보여준다
        return render(request, 'members/login.html')

```

`templates/base.html`에서 로그인, 로그아웃버튼을 만듭니다.

```html
<body>
    <div>
        <h1>Instagram</h1>
        <div>
            {% if user.is_authenticated %}
                <a href="">로그아웃</a>
            {% else %}
                <a href="{% url 'members:login' %}">로그인</a>
            {% endif %}
        </div>
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

{% endraw %}

---
### logout_vew구현


```
def logout_view(request):
    logout(request)
    return redirect('index')
```
logout_view와 url을 연결해 줍니다.
