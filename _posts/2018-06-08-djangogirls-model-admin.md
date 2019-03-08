---
layout: post
title:  "[DjangoGirls] - blog 앱 생성 및 관리자에 추가"
subtitle:   "Django 모델"
categories: django
tags: djangogirls
comments: true
---

### 애플리케이션 생성  

 `django-admin startproject mysite` : mysite라는 프로젝트를 생성, source root를 app폴더로 지정

`python manage.py startapp blog` : 프로젝트 mysite내부에  blog라는 애플리케이션 생성
보통 의미단위로 애플리케이션 만든다.
이제 이 애플리케이션을 장고가 관리 하도록 해야한다.
mysite/settings.py에 INSTALLED_APPS에 `blog` 입력 (패키지명 추가)



### 블로그 글 모델 만들기  

`app/blog/models.py` : 이 파일에 블로그 글 모델 정의  

```python
from django.db import models
from django.conf import
from django.utils import timezone  

# models라는 모듈의 Model을 상속받는다. Post가 데이터베이스에 저장되어야 한다고 알려줌  
class Post(models.Model):
  # ForeignKey : 다른 모델에 대한 링크를 의미
  author = model.ForeignKey(
    settings.AUTH_USER_MODEL,
    on_delete=models.CASCADE,
  )
  # CharField는 길이가 정해진 문자열, TextField 길이 무제한
  title=models.CharField(max_length=200)
  text=models.TextField(blank=True)
  created_date=models.DateTimeField(default=)
  published_date=models.DateTimeField(blank=True)

  # Post의 인스턴스 메서드
  def publish(self):
    self.publisehd_date=timezone.now()
    # 메모리상에서 데이터를 변경한 후 save를 호출해야 데이터베이스에 저장됨  
    self.save()

  #객체를 문자열로 표현하고 싶을 때 __str__  
  def __str__(self):
  return self.title  

```

### 데이터베이스에 모델을 위한 테이블 만들기  

`python manage.py makemigration`  : 장고 모델에 POst모델을 추가

`python manage.py migrate` : 실제 데이터베이스에 모델 추가를 반영


### 장고 관리자

`app/blog/admin.py`

```python
from django.contrib import admin
form .models import Post

admin.site.register(Post)

```

웹서버 실행 :`python manage.py runserver` 를 실행 후 localhost:8000/admin  

`python manage.py cratesuperuser`




tip: keymap > completion > basic ctrl+space (개발툴에서 자동완성만들어줌)
