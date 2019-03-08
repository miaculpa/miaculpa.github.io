---
layout: post
title:  "django로 인스타그램만들기 01 - pipenv로 의존성 관리, 프로젝트 기본 설정"
subtitle:   ""
categories: django
tags: insta
comments: true
---

### 인스타그램 만들기

장고로 인스타그램을 만들어 봅시다. 이 프로젝트에서는 pipenv를 이용해 의존성을 관리하도록 하겠습니다.


`pipenv --python 3.6.5` 가상환경을 설정해 줍니다.


`$ pipenv install django` 장고를 설치해 줍니다.


`vi Pipfile` and `vi Pipfile.lock` 버전 확인

`pipenv shell` 다음 명령어로 가상환경을 활성화 시킵니다.



프로젝트의 기본 세팅을 다음과 같이 해줍니다.
```
instagram/
  pipenv --python 3.6.5
  pipenv install django
  git init
  vim gitignore
  django-admin startproject config
  mv config app
```  


pycharm에서 interpreter를 설정해 줍니다.   

`/home/{username}/.virtualenvs/instagram-{randomvalue}/bin/python`


pycharm에서 alt+F12로 터미널창을 열 수 있습니다.


manage.py가 있는 디렉토리에서 runserver를 실행시켜 봅니다.
