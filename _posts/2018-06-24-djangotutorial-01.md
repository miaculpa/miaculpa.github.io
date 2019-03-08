---
layout: post
title:  "[DjangoTutorial] - 첫 번째 장고 앱 작성하기 part01, 02"
subtitle:   "01, 02"
categories: django
tags: djangotu
comments: true
---
reference
- [https://docs.djangoproject.com/ko/2.0/intro/tutorial01/]("https://docs.djangoproject.com/ko/2.0/intro/tutorial01/")

- [https://docs.djangoproject.com/ko/2.0/intro/tutorial02/]("https://docs.djangoproject.com/ko/2.0/intro/tutorial02 /")

---

django로 설문조사를 할 수 있는 사이트를 만들어 봅시다.

### 첫번째 장고 앱 만들기 part1

프로젝트 초기 설정을 다음과 같이 합니다.

```
# 가상환경 설정, pycharm interpreter설정
django-tutorial
├── app (django-admin startproject config / mv config app)
│   ├── config
│   │   ├── __init__.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   │ 
│   ├── db.sqlite3
│   ├── manage.py
│   └── polls (python manage.py startapp polls)
├──.gitingore(pycharm+all, django, python, macOS, linux, git)
├──.git (git init)
├── Pipfile
└── Pipfile.lock

```


#### 첫 번째 뷰 작성하기


`polls/view.py` 를 열어 다음 코드를 작성합니다.

```python
from django.http import HttpResponse


def index(request):
  return HttpResponse("Hello, world. You're at the polls index.")
```

view를 호출하려면 이와 연결된 url이 있어야 합니다. 이를 위해 URLconf가 사용됩니다.

`polls/urls.py` urls.py파일은 기본적으로 안들어 있기 때문에 polls 내부에 생성해 주어야합니다.      


```python
from django.urls import path, include

from . import views

urlspatterns = [
  path('', views.index, name='index'),
  # 최상위 URLconf에서 polls.url모듈을 연결하기 위해
  path('polls/', include('polls.urls')),
]
```


runserver로 잘 작동하는지 확인할 수 있습니다.
http://localhost:8000/polls/

---
#### path()함수의 인수
정규표현식은 개발자가 아닌 이상 보기 어렵기 때문에 path로 웹프레임워크에서 쉽게 표현할 수 있게 해줍니다.

1. path()인수 : route
route 는 URL 패턴을 가진 문자열을 말합니다.  요청이 처리될 때, Django 는 urlpatterns 의 첫 번째 패턴부터 시작하여, 일치하는 패턴을 찾을 때 까지 요청된 URL 을 각 패턴과 리스트의 순서대로 비교합니다. get parameter는 무시합니다.

2. path()인수 : view   
 view 함수의 HttpResponse객체를 자동으로 전달해줍니다.

3. path()인수 : kwargs   
임의의 키워드 인수들은 목표한 view 에 사전형으로 전달됩니다

4. path()인수 : name   
URL에 이름을 지으면 템플릿을 포함해 django어디에서나 참조가 가능해 집니다.

---
### 첫 번째 장고 앱 만들기, part2

#### 데이터베이스

`config/settings.py`에 DATABASES라는 딕셔너리가 한개 있습니다.
Django에서 기본적으로 사용하는 database는 밑의 네개 입니다.
다른 데이터베이스를 사용하고 싶으면 적절한 데이터베이스를 설치하고 DATABASES 'default' 항목을 다음의 키값으로 바꿔줍니다.

'django.db.backends.sqlite3',   

'django.db.backends.postgresql',       

'django.db.backends.mysql',    

'django.db.backends.oracle'  


`config/settings.py`의 윗쪽에 INSTALLED_APPS에는 모든 Django애플리케이션들의 이름이 담겨 있습니다. 애플리케이션을 만들 때 기본적으로 필요한 패키지들입니다.

django.contrib.admin -- 관리용 사이트

django.contrib.auth -- 인증 시스템.   

django.contrib.contenttypes -- 컨텐츠 타입을 위한 프레임워크.   

django.contrib.sessions -- 세션 프레임워크.   

django.contrib.messages -- 메세징 프레임워크.   

django.contrib.staticfiles -- 정적 파일을 관리하는 프레임워크.   


기본 애플리케이션이 사용하는 데이터베이스 내용을 적용시키기 위해서
`$ python manage.py migrate`를 실행해봅니다.

git에 database가 포함되지 않기 때문에 실제 프로젝트에서는 백업하는게 아주 중요합니다.

---

#### 모델 만들기
모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조를 말합니다.
우리가 만드는 설문조사(poll)앱을 위해 Question과 Choice라는 두개의 모델을 만들어 보겠습니다.

`polls/models.py`에 다음 코드를 작성합니다.

```python
from django.db import models


class Question(models.Model):
  # 각 필드는 Field클래스의 인스턴스
  question_text = models.CharField(max_length=200)
  pub_date = models.DateTimeField('date published')


class Choice(models.Model):
  question = models.ForeignKey(Question, on_delete=models.CASCADE)
  choice_text = models.CharField(max_length=200)
  votes = models.IntegerField(default=0)
```



---

#### 모델의 활성화
- models.py에 작성된 코드는 app 에 대하여 데이터베이스 스키마 생성 (CREATE TABLE statements)
- Question 과 Choice 객체에 접근하기 위한 Python 데이터베이스 접근 API 를 생성

하지만 이 project에게 polls 앱이 설치되어 있다는 것을 먼저 알려야 합니다.

`config/settings.py`의 INSTALLED_APPS에 다음을 추가해줍니다.

`polls.apps.PollsConfig` (polls/apps.py내의 PollsConfig클래스)

그 후 데이터베이스에 적용 시켜 봅니다.

`$ python manage.py makemigration`

`$ python manage.py migrate`


---
#### API가지고 놀기

`$ pip install ipython`

`$ python manage.py shell`

python 쉘에서 django가 접근할 수 있는 모듈 경로를 그대로 사용 할 수 있습니다.




```
>>> from polls.models import Choice, Question
>>> Question.objects.all()
<QuerySet []>   

# timezone > 현재시간
>>> from django.utils import timezone
>>> q=Question(question_text='What's new?', pub_date=timezone.now())
# Question인스턴스를 생성했지만 데이터베이스 안엔 없는 상태
>>>Question.objects.all()
<QuerySet []>


>>> q.save()
# save호출 후 데이터베이스에 저장됨
>>>Question.obejcts.all()
<QuerySet [<Question: Question object(1)>]>

# id는 데이터베이스에 추가할 때 생기는 것
>>> q.id
1

# 속성으로 접근가능
>>> q.question_text
"What's new>"

>>> q.question_text="What's up?"
# 변경사항 저장
>>> q.save()
```

객체를 표현하는데에 도움을 주기 위해 `polls/models.py`의 모델을 수정하여 __str__()메소드를 추가해 봅시다.

```python
from django.db import models

class Question(models.Model):
  # ...
  def __str__(self):
    return self.question_text


class Choice(models.Model):
  # ...
  def __str__(self):
    return self.choice_text

```



Question모델에 다음 메소드를 추가해 봅시다.

```python
def was_published_recently(self):
  # 지금시간에서 하루만큼 뺀 것
  # 24시간 이내에 발행 됐는가를 알려주는 함수
  # return 값은 true or false
  return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

이것을 적용하려면 shell을 exit 후 재실행 해줍니다.


```python
>>> from polls.models import Choice, Question

>>> from django.utils import timezone

>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>

>>> current_year = timezone.now().current_year

>>> q = Question.objects.first()

# datetime.datetime의 객체
>>> q.pub_date
datetime.datetime(2018, 6, 7, 4, 3, 21, 369057, tzinfo=<UTC>)

# .으로 속성에 접근하지만 query 안에서는 필터조건으로 쓸 수 없다. 대신 '__'사용
>>> q.pub_date.year
>>> Question.objects.filter(question__pub_date__year=current_year)

```


관계형 데이터베이스
객체끼리 연결을 가질 수 있습니다.
역방향으로 관계도 알아낼 수 있습니다.

Choice class를 보면 어떤 question에 속해 있는지 알 수 있습니다.
역으로 반대쪽 테이블에도 접근할 수 있습니다.

```python
from django.db import models


class Question(models.Model):
  question_text = models.CharField(max_length=200)
  pub_date = models.DateTimeField('date published')


class Choice(models.Model):
  # ForeignKey는 다른 객체와의 연걸, 즉 다른 클래스와의 연결
  question = models.ForeignKey(Question, on_delete=models.CASCADE)
  choice_text = models.CharField(max_length=200)
  votes = models.IntegerField(default=0)
```



```python
>>>q.choice_set.all()
<QuerySet []>

>>>Choice.objects.create(
  question=q,
  choice_text='Not much',
  votes=0,
)
<Choice: Not much>

>>> c1 = Choice.objects.first()

>>> c1.question
<Question: What's up?>

>>> q
<Question: What's up?>

>>> c1.question is == q
True

>>> c1.question is q
False

>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

>>> Choice.objects.all()
<QuerySet [<Choice: Not much>]>

# question에서 자기에게 연결된 choice를 불러옴
# 역방향으로의 관계 (reverse manager)
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>]>

# question안써도 된다. 관계가 이미 정의되어 있으므로
>>> q.choice_set.create(
    choice_text = 'Just hacking',
    votes=0,
)

# 두 명령은 정확히 같은 말 입니다.
>>> q.choice_set.all()
>>> Choice.objects.filter(question=q)

```




> Tip!

우분투에서 dbsqlite 설치하기
`$ sudo apt-get install sqlitebrowser`  



dbsqlite를 실행해서 db.sqlite3를 실행해 봅니다.



---

#### Django admin 생성하기

먼저 관리자 사이트에 로그인 할 수 있는 user를 만들어야합니다.

`$ python manage.py createsuperuser`

username, password를 입력해줍니다. email은 건너뛰어도 됩니다.




`polls/admin.py`  에 다음을 추가 해 줍니다.


```python
from django.contrib import admin

from .models import Question

admin.site.register(Question)

```


runserver를 실행시킨 후 localhost:8000/admin에서 로그인 해 봅니다.
