---
layout: post
title:  "django로 인스타그램만들기 08 - signup 구현"
subtitle:   ""
categories: django
tags: insta
comments: true
---

이번 포스트에서는 회원가입기능을 담당하는 함수를 만들어 봅시다.

request가 아무 오류 없이 왔다고 했을 때는 다음과 같이 유저를 생성해 주면 될 것입니다.

```python
from django.contrib.auth import authenticate, login, logout, get_user_model
from django.shortcuts import render, redirect

User = get_user_model()

#...

def signup(request):
    username = request.POST['username']
    password = request.POST['password']

    user = User.objects.created_user(
        username=username,
        password=password,
    )
```



User모델이 필요할 때는 `get_user_model()` 내장 함수 이용합니다.

User클래스 자체를 가져올때는 `get_user_model`, Foreignkey에 User모델을 지정할때는 `settings.AUTH_USER_MODEL` 를 사용합니다.

user모델을 커스터마이징할 수 있기 때문에 양쪽을 대비할 수 있는 get_user_model을 가져옵니다.

get_user_model은 지정한 user가 있으면 그것을, 아니면 기본 user를 가져옵니다.

---

## #user가 존재하는지 검사하기

이제 request가 POST가 아닌 경우 다시 회원가입 페이지로 보내고, POST요청일 경우 회원가입이 되게 함수를 바꿔줍니다.

그리고 exists를 사용해서 유저가 존재하는지 검사할 수 있도록 해줍니다.
exists()는 쿼리셋에 사용하는 함수입니다.

```python
def signup(request):
    if request.method == 'POST':
        # exists를 사용해서 유저가 이미 존재하면 signup으로 다시 redirect
        # 존재하지 않는 경우에만 아래 로직 실행
        username = request.POST['username']
        password = request.POST['password']
        if User.objects.filter(username=username).exists():
            return redirect('members:signup')
        user = User.objects.created_user(
            username=username,
            password=password,
        )
        login(request, user)
        return redirect('index')
    return render(request, 'members/signup.html')
```


{% raw %}

`templates/base.html`

```html
{% extends 'base.html' %}

{% block content %}
<div>
    {% if errors %}
        <ul>
        {% for error in errors %}
            <li>{{ error }}</li>
        {% endfor %}
        </ul>
    {% endif %}

    <form action="" method="post">
        {% csrf_token %}
        <div><label>Username<input type="text" name="username"></label></div>
        <div><label>Password<input type="password" name="password"></label></div>
        <div><label>Email<input type="text" name="email"></label></div>
        <button type="submit">회원가입</button>
    </form>
</div>
{% endblock %}
```

---

### 오류메세지 출력하기

위의 코드에서 user가 이미 존재하면 바로 redirect가 되는데 사용자입장에서는 오류를 보지 못하는 문제가 있습니다.

redirect는 브라우저 단에서 이루어지기 때문에 오류메세지 츌력하지 못합니다.

단순 redirect가 아니라, render를 사용해서 context에 오류메세지를 담아 전달 할 수 있도록 함수를 바꿔줍니다.

password가 일치하지 않는 경우에도 오류메세지가 출력될 수 있도록 해줍니다.

```python
def signup(request):
    context = {
        'errors': [],
    }
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        password2 = request.POST['password2']
        email = request.POST['email']
        # exists를 사용해서 유저가 이미 존재하면 signup으로 다시 redirect

        # 단순 redirect가 아니라, render를 사용
        # render에 context를 전달
        #   context로 사용할 dict객체의
        #       'errors'키에 List를 할당하고, 해당 리스트에
        #       '유저가 이미 존재함' 문자열을 추가해서 전달

        context['username'] = username
        context['email'] = email

        # form에서 전송한 데이터들이 올바른지 검사
        if User.objects.filter(username=username).exists():
            context['errors'].append('유저가 이미 존재함')
        # password, password2를 검사
        if password != password2:
            context['errors'].append('비밀번호가 일치하지 않음')
        # errors가 존재하면 render
        if not context['errors']:
            user = User.objects.created_user(
                username=username,
                password=password,
            )
            login(request, user)
            return redirect('index')
    return render(request, 'members/signup.html', context)
```


템플릿에서는 전달받은 errors를 순회하며 에러메시지를 출력하도록 해주고, 사용자 입장에서 쳤던문자를 다시 치지 않아도 되게 `value`를 지정해줍니다.


```html
{% extends 'base.html' %}

{% block content %}
<div>
    {% if errors %}
        <ul>
        {% for error in errors %}
            <li>{{ error }}</li>
        {% endfor %}
        </ul>
    {% endif %}

    <form action="" method="post">
        {% csrf_token %}
        <div><label>Username<input type="text" name="username" value="{{ username }}"></label></div>
        <div><label>Password<input type="password" name="password"></label></div>
        <div><label>PasswordCheck<input type="password2" name="password2"></label></div>
        <div><label>Email<input type="text" name="email" value="{{ email }}"></label></div>
        <button type="submit">회원가입</button>
    </form>
</div>
{% endblock %}
```



---


위의 오류 뿐 아니라 사용자가 전부 값을 채우지 않았을 경우에도 오류를 출력해 줘야 합니다.

하지만 밑에처럼 코드를 작성하면 비슷하게 반복되서 예쁘지 않습니다.

```
if not username:
    context['errors'].append('아이디를 입력하세요')

if not email:
    context['errors'].append('이메일을 입력하세요')

if not password:
    context['errors'].append('비밀번호를 입력하세요')

```

locals()함수를 사용해서 코드를 리팩토링해니다.

우선 print(locals())를 해보면 다음과 같은 결과가 출력됩니다.

```shell
{'email': 'd', 'password2': 'c', 'password': 'b', 'username': 'a', 'context': {'errors': []}, 'request': <WSGIRequest: POST '/members/signup/'>}
```

필요없는 정보들도 있기 때문에 반드시 필요한 내용만 따로 required_fields에 담아봅니다.

```python
# print(locals())
# 반드시 내용이 채워져야 하는 form의 필드 (위 변수명)
required_fields = ['username', 'email', 'password', 'password2']
for field_name in required_fields:
    print('field_name:', field_name)
    print('locals().[field_name]:', locals()[field_name])
```

그리고 다시 요청을 보내보면 다음과 같이 잘 나오는 것을 확인할 수 있습니다.

```shell
field_name: username
locals().[field_name]: a
field_name: email
locals().[field_name]: d
field_name: password
locals().[field_name]:
field_name: password2
locals().[field_name]: c
```

오류를 한글로 출력할 수 있도록 바꿔줍니다.

```python
required_fields = {
    'username': {
        'verbose_name': '아이디',
    },
    'email': {
        'verbose_name': '이메일',
    },
    'password': {
        'verbose_name': '비밀번호',
    },
    'password2': {
        'verbose_name': '비밀번호 확인',
    }
}
for field_name in required_fields.keys():
    if not locals()[field_name]:
        context['errors'].append('{}를(을) 채워주세요'.format(field_name))

```

드디어 signup view함수를 완성했습니다. 밑에가 완성된 코드입니다.


```python
from django.contrib.auth import authenticate, login, logout, get_user_model
from django.shortcuts import render, redirect

# User클래스 자체를 가져올때는 get_user_model()
# ForeignKey에 User모델을 지정할때는 settings.AUTH_USER_MODEL
User = get_user_model()


def signup(request):
    context = {
        'errors': [],
    }
    if request.method == 'POST':
        # 입력되지 않은 필드에 대한 오류를 추가
        username = request.POST['username']
        password = request.POST['password']
        password2 = request.POST['password2']
        email = request.POST['email']

        # print(locals())
        # 반드시 내용이 채워져야 하는 form의 필드 (위 변수명)

        # required_fields = ['username', 'email', 'password', 'password2']
        # for field_name in required_fields:
        #     print('field_name:', field_name)
        #     print('locals().[field_name]:', locals()[field_name])
        #     if not locals()[field_name]:
        #         context['errors'].append('{}를(을) 채워주세요'.format(field_name))

        required_fields = {
            'username': {
                'verbose_name': '아이디',
            },
            'email': {
                'verbose_name': '이메일',
            },
            'password': {
                'verbose_name': '비밀번호',
            },
            'password2': {
                'verbose_name': '비밀번호 확인',
            }
        }
        for field_name in required_fields.keys():
            if not locals()[field_name]:
                context['errors'].append('{}를(을) 채워주세요'.format(field_name))


        # 입력데이터 채워넣기
        context['username'] = username
        context['email'] = email

        # form에서 전송한 데이터들이 올바른지 검사
        if User.objects.filter(username=username).exists():
            context['errors'].append('유저가 이미 존재함')
        # password, password2를 검사
        if password != password2:
            context['errors'].append('비밀번호가 일치하지 않음')
        # errors가 존재하면 render


        if not context['errors']:
            user = User.objects.created_user(
                username=username,
                password=password,
            )
            login(request, user)
            return redirect('index')

    return render(request, 'members/signup.html', context)



```

웹사이트를 만들 때 이런 form이 들어가는 과정을 자주 쓰기 때문에 장고가 form class라고 하는 기능을 제공해줍니다.
다음 포스트에서 form class를 다루도록 하겠습니다.


{% endraw %}
