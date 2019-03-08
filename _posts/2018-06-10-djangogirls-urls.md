---
layout: post
title:  "[DjangoGirls] - post list 기본 구현"
subtitle:   "Django urls, view, template"
categories: django
tags: djangogirls
comments: true
---

`mysite/config/urls.py`

```python
from django.conf.urls import url
from django.contrib import admin


urlpatterns = [
  url(r'^admin/', admin.site.urls'),
  # blog 패키지에 urls를 참고하겠다.
  url(r'', include('blog.urls')),
]
```

Django에서 처리 가능한 한의 페이지를 만들 때
최소 구현
-urlresolver (특정 페이지의 URL을 설정)
-view(URL로 들어온 request를 처리하고 response를 돌려줄 함수)


`app/blog/urls.py`

```python
from django.conf.urls import url
from .views import post_list


urlpatterns = [
  # url의 첫 번째 인자: 매치될 URL정규표현식
  # url의 두 번째 인자: view function
  #   view function
  #    > request를 받아서 response를 돌려주는 함수
  # blog.views에 있는 post_list함수를 두 번째 인자로 전달
  url(r'^&', post_list),

]
```

http://localhost:8000/
Browser > request > Django > config.urls > blog.urls > r'^$'에 매칭 > def post_list > return

`app/blog/views.py`


```python
from django.http import HttpResponse


def post_list(request):
  # HttpResponse: Http응답을 보여주기 위한 class
  return HttpResponse('Post List입니다')

```

HttpResponse에서 html구조 쓰면 너무 끔찍함 > 애플리케이션 템플릿으로 처리가능

`app/template/blog/post_list.html` 생성

```python

html:5 + tab > 내용~~
```



`app/blog/views.py` 을 수정해서 post_list.html의 파일이 내용을 읽어서 return해주는 HttpResponse에 전달한다.

```python
import os
from django.http import HttpResponse

# first/
#   first_file.txt
#   second/
#     second_file.txt
#     thrid/
#       module.property
#       fourth/
#         fourth_file.txt
# module.py에서
# 0. 현재 경로 : os.path.abspath(__file__)
# 1. third/ 폴더의 경로 :os.path.dirname(<현재경로>)
# 2. second/ 폴더의 경로 : os.path.dirname(<third폴더의 경로>)
# 3. second/second_file.txt의 경로 : os.path.join(<second폴더의 경로>, 'second_file.txt')
# 4. fourth/ 폴더의 경로 : os.path.join(<현재 경로> , 'fourth')
# 5.fourt/fourth_file.txt의 경로 :os.path.join(<현재경로>, 'fourth', 'fourth_file.txt')

def post_list(request):
  cur_file_path = os.path.abspath(__file__)
  blog_dir_path = os.path.dirname(cur_file_path)
  app_dir_path = os.path.dirname(blog_dir_path)
  templates_dir_name = os.path.join(app_dir_path, 'templates')
  blog_template_file_path = os.path.join(templates_dir_path, 'blog', 'post_list.html')
  # 잘 모르겠을 땐 runserver실행 & print(blog_template_file_path)
  html = open(blog_template_file_path, 'rt').read()
  return HttpResponse(html)

```
