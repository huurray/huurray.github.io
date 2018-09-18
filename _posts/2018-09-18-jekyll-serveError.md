---
layout: post
comments: true
title: '[DEVLOG] Jekyll Serve Error <Invalid CP949 character "￦xE2">'
categories: [devlog]
tags: [jekyll]
date: 2018-09-18
---
![headerimg](/assets/img/subcate/jekyll-head.png)
Window에서 Jekyll CP949 에러 해결 방법
{:.center}


## 윈도우에서 지킬 serve 시 발생하는 CP949 에러!

![image](/assets/img/post/jekyll-serveError/1.jpg){:.lead}

지킬에 서버를 가동할 시 자주 발생하는 에러코드입니다.

윈도우에서 SASS를 사용하는 패키지 인코딩이 원인이 되는데요.
여러가지 방법이 있지만 제가 자주 쓰는 회피방법이 있어서 알려드립니다!

## 방법은?

>chcp 65001
{:.lead}

![image](/assets/img/post/jekyll-serveError/2.jpg){:.lead}
![image](/assets/img/post/jekyll-serveError/3.jpg){:.lead}

이렇게 65001 언어셋을 설정하고 서버를 가동하면 잘 작동됩니다!


[Jekyll 문서(Windows))](https://github.com/juthilo/run-jekyll-on-windows){:target="_blank"} 여기에 Ruby2.0 기준으로 잘 설명한 git이 있어서 링크 남깁니다.
