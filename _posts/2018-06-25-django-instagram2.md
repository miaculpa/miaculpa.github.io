---
layout: post
title:  "django로 인스타그램만들기 02 - Post클래스 추가, MEDIA_URL연결"
subtitle:   ""
categories: django
tags: insta
comments: true
---
reference
- [https://docs.djangoproject.com/en/2.0/howto/static-files/](https://docs.djangoproject.com/en/2.0/howto/static-files/) : django docs - Mamaging static files

---

### Posts 애플리케이션 만들기

`python manage.py startapp posts`


`instagram/app/posts/models.py` 에서 Post클래스를 만들어 봅시다.


```
# posts앱의 class Post의 field
#   author
#   photo (ImageField)
#   content(Text)
#   created_at (DateTime)
```



```python
from django.conf import settings
from django.db import models


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    # ImageField를 ctrl+클릭하면 원본소스 볼 수 있습니다
    # imagefield는 filefield를 상속받은 걸 확인할 수 있습니다
    # filefield와 imagefield는 어디로 올라갈지 정해줘야 합니다 > upload_to
    photo = models.ImageField(upload_to='post', blank=True)
    content = models.TextField(blank=True)
    # auto_now_add는 처음 create될 때, auto_now는 저장될 때마다 즉 수정한 시간을 기록할 때 사용합니다
    created_at = models.DateTimeField(auto_now_add=True)

```

ImageField를 사용하기 위해서는 Pillow가 설치되어 있어야 합니다.
> PIL(python imaging library)는 이미지 파일을 열고 조작하고 저장하는 기능을 제공하는 라이브러리입니다.
그런데 2011년 이후 commit이 중단 되면서 그 후 개발하기 시작한 것이 pillow입니다.

[pillow docs 참고](https://pillow.readthedocs.io/en/5.1.x/) - how to install pillow


`$ pipenv install pillow` 로 설치해 줍니다.

`$ pipenv graph` 로 설치된 것들을 확인할 수 있습니다.


`config/settings.py`의 INSTALLED_APPS에 `'Posts.apps.PostsConfig',`를 추가해 줍니다.

 그 후 makemigrations, migrate를 해줍니다.



 ---

 그 다음 post를 admin에 등록하고
 superuser를 생성,
 로그인해서 post를 하나 추가해 봅니다.

`posts/admin.py`

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

관리자 페이지(localhost:8000/admin)에서 post를 하나 추가해보면
post라는 폴더가 생기면서 이미지 파일이 들어가는 걸 확인할 수 있습니다.

우리가 IMAGE_FILED에서 upload_to에 'post'를 지정을 해주었기 때문입니다.



그런데 이 폴더이름을 바꿀 수 있을까요?


[https://docs.djangoproject.com/en/2.0/howto/static-files/](https://docs.djangoproject.com/en/2.0/howto/static-files/) : django docs - Mamaging static files


### 개발 중 정적 파일 제공

개발하는 중에 'django.contrib.staticfiles'를 사용하면 DEBUG를 True로 설정했을 때 runserver가 자동으로 static file을 제공하는 작업을 자동으로 수행해줍니다.


`config.settings.py`에 들어가면 아래에 STATIC_URL이 정의되어 있는 걸 확인할 수 있습니다.

```
STATIC_URL = '/static/'
```

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```
static()위치인자로 앞에는 urlpatterns를 반환하고, document에서 file을 찾아서 return해 주는 view를 반환합니다. 이것을 암시적으로 해줍니다.

하지만 user가 업로드하는 파일은 명시적으로 적어줘야 합니다.

---
#### User가 업로드 한 파일 제공

django.views.static.serve() view함수를 이용해 MEDIA_ROOT로부터 미디어파일을 업로드할 수 있습니다.

app 폴더 안쪽에 media폴더를 새로 만들어 줍니다.
MEDIA_ROOT는 어디에 저장될지 실제 경로를 나타내고,
MEDIA_URL은 사용자가 올린 파일들을 어떤 url을 기준으로 upload를 할 것인지를 나타냅니다.
post폴더가 아무데나 생기는데 우리는 특정 경로에 생기게 만들고 싶습니다.

`config.settings.py`에서 BASE_DIR를 기준으로
MEDIA_ROOT를 지정해주고 runserver를 실행해서 확인해 봅니다.


그 후 user-uploaded file에 접근할 URL접두어를 지정해줍니다.


```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

print('BASE_DIR:', BASE_DIR)
print('MEDIA_ROOT: ', MEDIA_ROOT)

# ...

# 정적파일에 접근할 URL접두어
STATIC_URL = '/static/'

# User-uploaded file을 접근할 URL접두어
MEDIA_URL = '/media/'
```

하지만 관리자페이지의 post에서 이미지 파일을 클릭해보면 404에러가 발생합니다.

/media/가 왔을 때 어디로 이동할 지 정해주지 않아서 입니다. 그러므로 view function을 추가해 줍니다.


```python
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
    # path('media/<str:path>/', 특정view_function),
] + static(
    prefix='/media/',
    document_root=settings.MEDIA_ROOT,
    )

```

static 함수를 ctrl+클릭 해보면 static이 무엇을 return해주는지 알 수 있습니다.

```python
def static(prefix, view=serve, **kwargs):
    """
    Return a URL pattern for serving files in debug mode.

    from django.conf import settings
    from django.conf.urls.static import static

    urlpatterns = [
        # ... the rest of your URLconf goes here ...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    """
    if not prefix:
        raise ImproperlyConfigured("Empty static prefix not permitted")
    elif not settings.DEBUG or '://' in prefix:
        # No-op if not in debug mode or a non-local prefix.
        return []
    # static함수가 return 해주는 data type은 list입니다.
    # lstrip은 왼쪽이 '/'로 시작할 때 그것을 없애주는 함수 입니다.
    return [
        re_path(r'^%s(?P<path>.*)$' % re.escape(prefix.lstrip('/')), view, kwargs=kwargs),
    ]

```

media폴더는 git에 포함이 안되는데
.gitignore를 보면 media폴더가 기본적으로 포함된 것을 확인 할 수 있습니다.


> git commit    
>Post클래스 추가, MEDIA_URL연결       
>MEDIA_ROOT는 실제 파일이 업로드 되는 기준 경로        
>MEDIA_URL은 사용자가 업로드한 파일을 제공할 URL  
>config.urls에 추가한 static()은 MEDIA_URL로의 request에 대해    
       MEDIA_ROOT에서 찾은 파일을 response로 돌려줌     
