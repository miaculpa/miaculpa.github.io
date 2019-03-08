---
layout: post
title:  "[Crawler] - 웹툰 크롤링2"
subtitle:   "크롤링2"
categories: web
tags: crawler
comments: true
---

### 4. 에피소드를 클래스로 구현


```python
import os
from urllib import parse
import requests
from bs4 import BeautifulSoup

#에피소드를 클래스로 구현
class Episode:
    def __init__(self, webtoon_id, no, url_thumbnail, title, rating, created_date):
        self.webtoon_id = webtoon_id
        self.no = no
        self.url_thumbnail = url_thumbnail
        self.title = title
        self.rating = rating
        self.created_date = created_date

#실제 에피소드 페이지의 URL을 리턴
    @property
    def url(self):
        url =  ' http://comic.naver.com/webtoon/detail.nhn?'

        params = {
            'titleId': self.webtoon_id,
            'no': self.no,
        }

        episode_url = url + parse.urlencode(params)
        print(episode_url)
        return episode_url


def webtoon_crawler(webtoon_id):
    """
    webtoon_id를 입력받아서
    :param webtoon_id:
    :return:
    """
    file_path = 'data/episode_list_{webtoon_id}.html'.format(webtoon_id = webtoon_id)
    if os.path.exists('file_path'):
        # 저장되어 있다면, 해당 파일을 읽어서 html변수에 할당 html=open().read()로 쓸수있
        f = open(file_path, 'rt')
        html = f.read()

    else:
        # 저장되어 있지 않다면, requests를 사용해 HTTP GET요
        payload = {'titleId': webtoon_id}
        response = requests.get('http://comic.naver.com/webtoon/list.nhn', params=payload)
        with open(file_path, 'wt') as f:
            f.write(response.text)
            html = response.text


    soup = BeautifulSoup(html, 'lxml')
    h2_title = soup.select_one('div.detail > h2')
    title = h2_title.contents[0].strip()
    author = h2_title.contents[1].get_text(strip=True)
    description = soup.select_one('div.detail > p').get_text(strip=True)


    #웹툰 크롤링을 통해 얻게된 정보를 딕셔너리 형태로 return
    info = dict()
    info['title'] = title
    info['author'] = author
    info['description'] = description
    return info


def episode_crawler(webtoon_id):
    """
    webtoon_id를 입력받아서 title, no, created_date 등등 어떤 정보를 가져오는 크롤러
    :param webtoon_id:
    :return:
    """
    file_path = 'data/episode_list_{webtoon_id}.html'.format(webtoon_id=webtoon_id)
    url_episode_list = 'http://comic.naver.com/webtoon/list.nhn'
    # HTTP요청시 전달할 GET Parameters
    params = {
        'titleId': 703845,
    }
    if os.path.exists(file_path):
        # 저장되어 있다면, 해당 파일을 읽어서 html변수에 할당
        html = open(file_path, 'rt').read()

    # BeautifulSoup클래스형 객체 생성 및 soup변수에 할당
    soup = BeautifulSoup(html, 'lxml')

    table = soup.select_one('table.viewList')
    tr_list = table.select('tr')

    #list를 return하기 위해 선언
    #for문을 다 실행하면 episode_lists에는 Episode인스턴스가 들어가 있음
    episode_list = list()

    for index, tr in enumerate(tr_list[1:]):

        if tr.get('class'):
            continue

        url_thumbnail = tr.select_one('td:nth-of-type(1) img').get('src')


        url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')

        query_string = parse.urlsplit(url_detail).query
        query_dict = parse.parse_qs(query_string)
        no = query_dict['no'][0]
        title = tr.select_one('td:nth-of-type(2) > a').get_text(strip=True)
        rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
        created_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)

        # print(url_thumbnail)
        # #print(url_detail)
        # print(title)
        # print(rating)
        # print(created_date)
        # print(no)

        #매 에피소드 크롤링한 결과를 episode클래스를 new_episode 인스턴스로 생성
        #new_episode = Episode 인스턴스(객체)

        new_episode = Episode(
            webtoon_id = webtoon_id,
            no = no,
            url_thumbnail = url_thumbnail,
            title = title,
            rating = rating,
            created_date = created_date,
        )
        #episode_list에 Episode인스턴스를 추
        episode_list.append(new_episode)
        #print(episode_list)

    return episode_list

ep = episode_crawler(703845)

# for item in ep:
#     print(item.no, item.title)

class Webtoon:
    def __init__(self, webtoon_id):
        self.webtoon_id = webtoon_id
        info = webtoon_crawler(webtoon_id)
        print(info)
        self.title = info['title']
        self.author = info['author']
        self.description = info['description']
        self.episode_list = list()

    def update(self):
        """
        update함수를 실행하면 해당 webtoon_id에 따른 에피소드 정보들을 episode 인스턴스로 저장
        :return:
        """
        result = episode_crawler(self.webtoon_id)
        #print(result)
        self.episode_list = result


if __name__ == '__main__':
    webtoon1 = Webtoon(703845)
    print(webtoon1.title)
    print(webtoon1.episode_list)
    webtoon1.update()
    print(webtoon1.episode_list)

    for episode in webtoon1.episode_list:
        print(episode.url)

```
