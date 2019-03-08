---
layout: post
title:  "[DjangoGirls] - setting"
subtitle:   "setting"
categories: django
tags: djangogirls
comments: true
---



### 가상환경(virtual envirionment) 설정

```
~/projects/
    django/
      djangogirls-tutorial/
        git init
        .gitignore(python, django, macos, linux, git, pycharm+all)
        pyenv virtualenv 3.6.5 fc-djangogirls
        pyenv local fc-djangogirls

        Pycharm interpreter설정
        linux ~/.pyenv/versions/fc-djangogirls/bin/python
        mac   /usr/local/var/pyenv/versions/fc-djangogirls/bin/python

```

### 장고 설치하기

`command line`

```
$ pip install django~=1.11.0
```

### 장고 프로젝트 시작

```
$ django-admin startproject mysite
```

장고가 동작하기 위해서는 특별한 구조가 필요한데 그걸 장고가 해준다
django-admin startproject mysite
> mysite라는 이름의 프로젝트를 실행하겠다.

```
djangogirls-tutorial/
  mysite/               < Django프로젝트 관련 코드 컨테이너 폴더
    manage.py
    mysite/             <이 Django프로젝트의 설정 관련 패키지
      settings.py
```

`mysite` 란 폴더가 두개라 헷갈리기 때문에
1. Django코드 컨테이너 폴더의 이름을 app으로 변경
  (seach for reference와 Serch in comments and strings 체크 해제)
2. Django프로젝트 설정 패키지의 이름을 config로 변경
  (reference와 comments and string 체크)

```
djangogirls-tutorial/     < 이 프로젝트의 컨테이너 폴더(Root폴더)
  app/                    < Django프로젝트 관련 코드 컨테이너 폴더
    config/               < Django프로젝트의 설정 관련 패키지
      settings.py
    .gitignore            < Django프로젝트 (애플리케이션)코드와 관계없지만
    .git/                   프로젝트를 위해 필요한 파일/폴더
    requirements.txt
```

### 모듈 가져오기

pycharm은 현재 프로젝트 구조에서 가장 상위폴더를 파이썬 source의 root로 인식한다

```
project/   <- python실행
    project_module1.py
    project_module2.py
    pac1/
        __init__.py
        pac1_module.py
        pac2/
            __init__.py
            pac2_module.py
            pac2_module2.py

            pac3/
                __init__.py


## python을 root에서 시작했을 때
# project_module1을 가져올때
import project_module1

# pac1패키지 내의 pac1_module을 가져올 때
from pac1 import pac1_module

# pac2패키지 내의 pac2_module을 가져올 때
from pac1.pac2 import pac2_module

## 하위 패키지에서의 동작
# pac2_module.py에서 pac2_module2.py를 가져오고 싶을 경우
(pac2_module.py의 내용)
from         . import pac2_module2
from pac1.pac2 import pac2_module2

# pac2_module.py에서 pac1_module.py를 가져오고 싶을 경우
from   .. import pac1_module
from pac1 import pac1_module

## python을 pac1에서 시작했을 때
# project_module1을 가져오려면
불가능
```

```
pip list

pip freeze  # Django, pytz version알려줌

pip freeze > requirements.txt # Django, pytz version이 기록됨
# 10년전 프로젝트여도 가상환경을 requirements.txt대로 설정하면 돌릴 수 있게됨
```






정리
> Django코드 컨테이너 폴더명을 app으로 변경
Django설정 패키지명을 config로 변경
.gitignore를 작성
requrements.txt를 작성


---
`reference`
- [장고걸스](https://tutorial.djangogirls.org/ko/django/)
