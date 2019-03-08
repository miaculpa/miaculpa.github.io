---
layout: post
title:  "django로 인스타그램만들기 03 - post list와 post detail view함수 작성, 템플릿 설정, view와 url연결"
subtitle:   ""
categories: django
tags: insta
comments: true
---




### config.urls에서 posts.urls include하기

`posts`앱 안에 'urls'모듈을 만들어 준 후

`config.urls`에서 `post.urls`를 include해줍니다.

```python
urlpatterns = [
    path('', views.index),
    path('admin/', admin.site.urls),
    path('posts/', include('posts.urls')),
    ]
  # ...

```




### view와 url의 연결구현

`posts/views.py`에 view함수를 작성해 줍니다.

```python

from django.shortcuts import render

from .models import Post


def post_list(request):
    return HttpResponse('post_list')


def post_detail(request, pk):
    return HttpResponse('post_detail')

```

그리고 view를 url과 연결 해 줍니다.

`posts/urls.py`에

```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.post_list),
    path('<int:pk>/', views.post_detail),
]
```

---

### view에서 template을 렌더링하는 기능 추가


view에서 template을 렌더링 해주기 전에
`config/settings.py`에 TEMPLATE설정을 해줍니다.

```python
#...
TEMPLATES_DIR=os.path.join(BASE_DIR, 'templates')
#...
TEMPLATES = [
    {
        # ...
        'DIRS': [
            TEMPLATES_DIR,
        ],
        #...
    }
]
```


이제 view를 수정해 template을 렌더링 해줍니다.

```python

from django.shortcuts import render

from .models import Post


def post_list(request):
    # 모든 포스트를 가져옴
    posts = Post.objects.all()
    context = {
        'posts': posts,
    }
    # context를 보냈으니까 template에서 사용 가능
    return render(request, 'posts/post_list.html', context)


def post_detail(request, pk):
    post = Post.objects.get(pk=pk)
    context = {
        'post': post,
    }
    return render(request, 'posts/post_detail.html', context)

```


---

### template에서 QuerySet또는 object를 사용해서 객체 출력, extend사용


{% raw %}

`templates/base.html`

```html
<!doctype html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Instagram</title>
</head>
<body>
    <div>
        <h1>Instagram</h1>
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```


`templates/posts/post_list.html`

```html
{% extends 'base.html' %}

{% block content %}

<h1>post list</h1>
<!-- post list에서는 QuerySet이 전달되기 때문에 for문으로 순회 -->
{% for post in posts %}
    <div>
        <div>{{ post}}</div>

    </div>
{% endfor %}

{% endblock %}


```

`templates/posts/post_detail.html`

```html
{% extends 'base.html' %}

{% block content %}

<h1>post detail</h1>

<div>{{ post }}</div>

{% endblock %}
```

{% endraw %}
