---
layout: post
title:  "django로 인스타그램만들기 05 - 세션과 쿠키"
subtitle:   ""
categories: django
tags: insta
comments: true
---

reference
- [https://docs.djangoproject.com/en/2.0/topics/auth/](https://docs.djangoproject.com/en/2.0/topics/auth/) : Django docs -  User authentication

- [https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4) : 위키백과 - HTTP쿠키

---


```
세션


브라우저 > 로그인요청(username/password) > 서버
  > authentication(인증)
      주어진 username/password에 해당하는 유저가 있는지 검사
  > 인증에 성공하면, 그 사용자에 해당하는 '특정값'을 DB에 저장(세션값을 저장한다)
  > '특정값'을 HTTP response에 Set-Cookie헤더로 담아 전송
      > 브라우저는 response를 받고, Set-Cookie헤더에 담긴 내용을 쿠키저장공간에 저장

> 이후 브라우저 > 서버로 가는 모든 요청에 쿠키저장공간에 있는 특정값을 함께 보냄
      > 서버는 받은 request에 특정값이 있는지 검사, 특정 유저에 매칭이되면 같은 유저가 요청을 보냈다고 간주

```


Http요청은 불연속적입니다. 하지만 우리가 login을 했을 때 login이 유지 됩니다.  

이것은 위의 과정이 장고 내부에 암시적으로 설정되어 있기 때문입니다.
이것을 세션을 유지한다라고도 말합니다.    

브라우저에서는 그 세션값을 저장하는 방법이 쿠키입니다.

admin page에서 `개발자도구 > application > cookies > sessionid`와

db의 `django_sessions` 를 비교하면 같은 값이 하나 있는 것을 확인 할 수 있습니다.
