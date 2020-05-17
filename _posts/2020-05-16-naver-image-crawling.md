---
layout:     post
title:      "Hello 2020"
subtitle:   " \"Hello World, Hello Blog\""
date:       2020-05-16 12:00:00
author:     "Jusung"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
---

This post is about simple web crawling using beautiful soup.

I used this to crawl images of Korean celebrities to train a machine learning model to categorize which animals' image they match.

The code is below

```coq
from urllib.request import urlopen
from bs4 import BeautifulSoup as bs
from urllib.parse import quote_plus
from pathlib import Path

baseUrl = 'https://search.naver.com/search.naver?where=image&sm=tab_jum&query='
people = {'puppy': ['강다니엘', '백현', '박보검', '송중기'],
          'cat': ['황민현', '시우민', '이종석', '강동원', '이종석', '이준기'],
          'bear': ['마동석', '조진웅', '조세호', '안재홍'],
          'dinosaur': ['윤두준', '이민기', '육성재', '공유', '김우빈'],
          'rabbit': ['정국', '바비', '박지훈', '수호']}
Path("./img").mkdir(parents=True, exist_ok=True)
for k, v in people.items():
    Path("./img/" + k).mkdir(parents=True, exist_ok=True)
    for person in v:
        url = baseUrl + quote_plus(person)
        html = urlopen(url)
        soup = bs(html, "html.parser")
        img = soup.find_all(class_='_img', limit=50)
        Path("./img/" + k + '/' + person).mkdir(parents=True, exist_ok=True)
        n = 1
        for i in img:
            imgUrl = i['data-source']
            with urlopen(imgUrl) as f:
                with open('./img/' + k + '/' + person + '/' + person + ' ' + str(n)+'.jpg','wb') as h: # w - write b - binary
                    img = f.read()
                    h.write(img)
            n += 1
print('다운로드 완료')
```

This code generates directories and images from naver search result.

The limitations are that CSS selectors are used to crawl images, so it cannot be used if naver changes their CSS selectors in the future.

References:
Jocoding:
https://www.youtube.com/watch?v=ZTJjW7XuHIY&list=PLU9-uwewPMe2-vtJAgWB6SNhHcTjJDgEO

https://velog.io/@joygoround/%EC%A1%B0%EC%BD%94%EB%94%A9-%EC%99%84%EC%84%B1%ED%98%95-%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0-1