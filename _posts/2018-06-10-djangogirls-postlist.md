---
layout: post
title:  "[DjangoGirls] - post list에서 동적으로 Post목록 보여주기"
subtitle:   ""
categories: django
tags: djangogirls
comments: true
---


> context에 'posts'키로 전달한 QuesrySet을 순회하며 div요소를 반복해서 생성해준다.


```python
result = '글 목록<br>'
-    for post in Post.objects.all():
-        result += '{}<br>'.format(post.title)
-    return HttpResponse(result)
-    # Post instance에서 title속성에 접근가능
-    # HttpResponse에
-    #
-    # 글 목록<br>
-    # - 격전 참여시..<br>
-    # - 부정행위..<br>
-    # - PBE...<br>
-    #
-    # 위 텍스트를 넣어서 리턴
```

```python
def post_list(request):
  posts = Post.objects.all()
  context = {
    'posts' : posts,
  }
  # render는 주어진 인수를 사용해서
  # 1번째 인수: HttpRequest인스턴스
  # 2번째 인수: 문자열(TEMPLATE['DIRS']을 기준으로 탐색할 템플릿 파일의 경로)
  #  3번째 인수: 템플릿을 렌더링할때 사용할 객체 모음
  # return render(request, 'blog/post_list.html', context)
   return render(
      request=request,
      template_name='blog/post_list.html',
      context=context,
  )

```


```python
{% for post in posts %}
		<div>
			<p>published: {{ post.published_date }}</p>
			<h2>{{ post.title }}</h2>
			<p>{{ post.text }}</p>
		</div>
	{% endfor %}

```
