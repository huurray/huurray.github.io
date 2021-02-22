---
layout: post
comments: true
title: '[DEVLOG] 나의 개발 블로그를 구글 검색어 노출하기'
categories: [devlog]
tags: [jekyll]
date: 2018-10-30
---
![headerimg](/assets/img/subcate/jekyll-head.png)
google 웹마스터 도구(Search console)을 사용하자!
{:.center}

## jekyll 블로그 글이 구글에서 검색되도록 만들기

jekyll로 만들어진 블로그는 구글에서 검색이 안된답니다!ㅜㅜ<br>
그래서 네이버의 웹마스터 도구처럼 구글도 **Search console**에 블로그를 등록해주어야 합니다.

(1). [구글 웹 마스터 도구 접속](https://search.google.com/search-console/about?hl=ko&utm_source=wmx&utm_medium=wmx-welcome)

(2). 나의 지킬 블로그 주소를 입력합니다.

![image](/assets/img/post/jekyll-googleSearch/0.png)

(3). 구글에서 제공하는 html을 다운로드합니다.

![image](/assets/img/post/jekyll-googleSearch/1.png)

(4). html을 루트 디렉토리에 올리고 커밋/ 푸쉬합니다. (네이버와 동일한 방식)

(5). **sitemap.xml** 파일을 루트 디렉토리에 작성합니다. 템플릿에 따라 저파일이 있을 수도 있는데 있으면 그냥 쓰고 없으면 검색해서 좋은 폼을 찾아 작성해줍니다.<br>
(_config.yml 파일 내의 url 부분에 **자신의 블로그 url**을 입력해야 sitemap.xml에서 site.url 부분을 사용 할 수 있어요!)

(6). 구글 웹마스터 도구에서 sitemaps메뉴를 선택후 주소를 입력합니다. (ex.https://example.github.io/sitemap.xml)

![image](/assets/img/post/jekyll-googleSearch/3.png)

