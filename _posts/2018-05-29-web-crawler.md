---
layout: post
title:  "[Crawler] - 웹툰 크롤링1 "
subtitle:   "크롤링1"
categories: web
tags: crawler
comments: true
---
## 죽음에 관하여 크롤링

### 1. requests
>HTML받아와서 html변수에 문자열을할당   
  os.path.exists(경로)의 결과는 파일이 존재하는지, 존재하지 않는지 여부를 True/False로 반환해준다     
  os.path.exists()의 결과를 분기로   
   파일이 존재하면 -> open()을 사용해 가져온 파일 객체를 read()한 결과 문자를 html변수에 할당   
   존재하지 않으면 -> 1. requests.get()의 결과인 response의 text속성값을 html변수에 할당     
               -> 2. response의 text속성값을 'data/episode_list.html'파일에 저장  



```python
import requests
import os
from bs4 import BeautifulSoup

# HTML파일을 저장하거나 불러올 경로
file_path = 'data/episode_list.html'
# HTTP요청을 보낼 주소
url_episode_list = 'http://comic.naver.com/webtoon/list.nhn'
# HTTP요청시 전달할 GET Parameters
params = {
    'titleId': 703845,
}

#HTML파일이 로컬에 저장되어 있는지 검사
if os.path.exists(file_path):
  #저장되어 있다면, 해당 파일을 읽어서 html변수에 할당
  html = open('data/episode_list.html', 'rt').read()

else:
  #저장되어 있지 않다면, requests를 사용해 HTTP GET요청
  response = requests.get('http://comic.naver.com/webtoon/list.nhn', params)
  #요청 객체의 text속성값을 html변수에 할당
  html = response.text
  open('data/episode_list.html', 'wt').write(response.text)

```

### 2. 제목, 저자, 웹툰정보 탐색하기
> html변수를 사용해 soup변수에 BeautifulSoup객체를 생성   
 soup객체에서   
    - 제목: 죽음에 관하여 (재)   
    - 작가: 시니/혀노   
    - 설명: 삶과 죽음의 경계선, 그 곳엔 누가 있을까.   
  의 내용을 가져와 title, author, description변수에 할당   

```python
soup = BeautifulSoup(html. 'lxml')

#div.detail > h2 (제목, 작가)의
#0번째 자식: 제목 텍스트, 1번째 자식: 작가정보
#tag로 문자열을 가져올 때는 get_text()
h2_title = soup.select_one('div.detail > h2')
title = h2_title.contents[0].strip()
author = h2_title.contents[1].get_text(strip=True)
#div.detail > p (설명)
description = soup.select_one('div.detail>p').get_text(strip=True)

print(title)
print(author)
print(description)
```

### 3. 에피소드 정보 목록 가져오기
>url_thumbnail: 썸네일 URL   
 title: 제목   
 rating: 별점   
 crated_date: 등록일   
 no: 에피소드 상세페이지의 고유 번호   
 각 에피소드들은 하나의 dict데이터   
 모든 에피소드들을 list에 넣는다   

```python
#table.viewList>tbody>tr>img
table = soup.select_one('table.viewList')
#print(table.prettify()) 예쁘게 볼 수 있음

#select_one과 select차이는 하나냐 여러개냐 후자는 list반환
#table내의 모든 tr요소 검색.
tr_list = table.select('tr')

#첫 번째 tr은 thead의 tr 이므로 제외.
for index, tr in enumerate(tr_list[1:]):
    #print(tr.attrs)그 요소의 속성
    #에피소드에 해당하는 tr은 클래스가 없으므로
    #현재 순회중인 tr요소가 클래스 속성값을 가진다면 continue
    if tr.get('class'):
        continue
    #print('=={}==\n{}|n'.format(index, tr))
    #현재 tr의 첫번째 요소 td요소의 하위 img태그의 'src'속성값.
    url_thumbnail = tr.select_one('td:nth-of-type(1) img').get('src')

    #현재 tr의 첫번째 요소 td요소의 자식 a태그의 'href' 속성값.

    url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
    #url_dict = dict(parse.parse_qsl(parse.urlsplit(url_detail).query))
    query_string = parse.urlsplit(url_detail).query
    query_dict = parse.parse_qs(query_string)
    no = query_dict['no'][0]

    #현재 tr의 두번째 요소의 td요소의 자식 a요소의 내용.
    title = tr.select_one('td:nth-of-type(2) > a').get_text(strip=True)
    #현재 tr의 세번째 요소의 td요소의 하위 srong태그의 내용.
    rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
    #현재 tr의 내번째 td요소의 내용
    created_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)

    print(url_thumbnail)
    print(url_detail)
    print(no)
    print(title)
    print(rating)
    print(created_date)
```
