---
layout: post
title:  "[DjangoGirls] - introduction"
subtitle:   "introduction"
categories: django
tags: djangogirls
comments: true
---

## 장고란?
>Django는 파이썬으로 만들어진 무료 오픈소스 웹 애플리케이션 프레임워크이다.

> 애플리케이션 프레임워크는 프로그래밍에서 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임이다. 재사용할 수 있는 수많은 코드를 프레임워크로 통합함으로써 개발자가 새로운 애플리케이션을 위한 표준 코드를 다시 작성하지 않아도 같이 사용된다.



## 서버에 웹 사이트를 요청하는 과정
```
Browser -> (request) -> Server -> Django application
-> urlresolver
    ->    view1 -> (process) -> (response) -> Server -> Browser
        view2
        view3


# 사용자가 브라우저를 통해 서버컴퓨터로 요청을 보낸다.
# 요청이 장고 애플리케이션으로 전달된다.
# 장고 urlresolver는 그 요청을 분류하고 관련된 함수(view)에 넘겨준다
# view함수에서 요청을 처리한 후 다시 사용자의 브라우저에 돌려준다.
```

`reference`

- [장고걸스](https://tutorial.djangogirls.org/ko/django/)
- [참고 : 위키백과](https://ko.wikipedia.org/wiki/%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC) -애플리케이션 프레임워크
