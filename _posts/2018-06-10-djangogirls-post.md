---
layout: post
title:  "[DjangoGirls] - TEMPLATES DIR설정, ORM실습"
subtitle:   "Django urls, view, template"
categories: django
tags: djangogirls
comments: true
---

### TEMPLATES_DIR 설정


settings.py에 `TEMPLATES_DIR = '<경로>'` 를 정해주면
이전 포스트에서처럼 매번 경로를 계산해 주지 않아도된다.


```python
TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')
TEMPLATES = [
  'DIRS' :[
      # Django가 render, render_to_string등의 함수로
      # 템플릿을 불러올 때 기준이 되는 폴더 목록
      TEMPLATES_DIR,
  ]
]
```

```python
def post_list(request):
  # 경로에 해당하는 HTML파일을 문자열로 로드해줌
  # html = render_to_string('blog/post_list.html')
  #  return = HttpResponse(html)
  return render('blog/post_list.html')
```

```
정리
Django

view function:
  html = render_to_string(path)
  # path: template_dir/path
  return HttpResponse()

templates_dir
  template files

settings.py
TEMPLATES = {
  'DIRS': [template_dir]
}
#settings.py에 있는 template_dir를 기준으로 해서 path에서 data를 찾아온다.
```

### 장고 ORM과 쿼리셋(QuerySets)
> 쿼리셋은 전달받은 모델의 객체 목록이다. 쿼리셋은 데이터베이스로부터 데이터를 읽고, 필터를 걸거나 정렬을 할 수 있다.

`command-line`에서 아래 명령 입력

`pip install ipython`
`python manage.py shell`

여기에서 데이터베이스를 탐색해보자.

```python
#post모델을 blog.models에서 불러온다.
from blog.models import Post

# 데이터베이스에 접근할 땐 objects를 씀
post_list = Post.objects.all()
for post in Post.objects.all():
    print(type(post))
```

데이터베이스에 새 글 객체를 저장하는 방법에 대해 알아보자.

```python
from django.contrib.auth.models import User

me = User.objects.get(username='ahyeon')

Post.objects.create(
  author=me,
  title='New Post',
  text='New Post text',
  )
```




### 필터링하기

```python
#글들 중 제목에 way라는 글자가 들어간 글들만 보여줌
Post.objects.filter(title__contains='way')

```

### 정렬하기
```python
# 제목 오름차순
for post in Post.objects.order_by('title'):

```
