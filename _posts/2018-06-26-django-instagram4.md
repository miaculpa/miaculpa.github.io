---
layout: post
title:  "django로 인스타그램만들기 04 - post list에서 이미지 출력"
subtitle:   ""
categories: django
tags: insta
comments: true
---


### '/'에 접근했을 때 post_list URL로 이동
redirect또는 HttpResponseRedirect사용해서 index view를 구현하고 url과 연결해 봅시다.

redirect를 사용하기 위해 `posts/urls.py`에 app_name과 url name을 지정해 줍니다.

```python
from django.urls import path

from . import views

app_name = 'posts'

urlpatterns = [
    path('', views.post_list, name='post-list'),
    path('<int:pk>/', views.post_detail, name='post-detail'),
]
```



`config/views.py`

```python
from django.shortcuts import redirect


def index(request):
    # return HttpResponseRedirect('posts/')
    # url이름으로 redirect > posts/urls에 app_name지정
    return redirect('posts:post-list')
```



`config/urls.py`

```python
from . import views

urlpatterns = [
    path('', views.index),
    # ...
]

```




---
### post_list.html에서 post 이미지를 출력

{% raw %}

`templates/posts/post_list.html`

`{{ post.photo }}`만 작성해주면 post/img_name.jpg가 출력됩니다.
db에 들어가보면 post의 photo에 문자열이 들어가 있는데 그게 그대로 출력 된 것 입니다.

localhost:8000/media/name.jpg 로가면 이미지 볼 수 있는데 src속성에 이 주소를 넣어주면 이미지를 출력할 수 있습니다.

`/media/{{ post.photo }}`를 바꿔서 `{{ post.photo.url }}`로 작성해 줍니다.


```html
{% extends 'base.html' %}

{% block content %}

<h1>post list</h1>
<!-- QuerySet이 전달되서 for문으로 순회 -->
{% for post in posts %}
   <!--
        post의 author.username과
        post.photo를 출력
   -->
    <div>
        <div>{{ post.author.username }}</div>
        <!-- imagefield나 filefield의 url을 파이썬에서 자동으로 만들어줌 -->
        <img width=100% src="{{ post.photo.url }}" alt="">
    </div>
{% endfor %}

 {% endblock %}


```


{% endraw %}
