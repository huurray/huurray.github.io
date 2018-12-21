---
layout: post
comments: true
title: '[DEVLOG] 인터넷 익스플로러 IE 이미지 깨짐현상'
categories: [devlog]
tags: [css]
date: 2018-12-20
---
![headerimg](/assets/img/subcate/css.png)
css관련 리포트 및 의견 정리
{:.center}

## IE 환경에서 발생하는 이미지 계단현상!

IE가 빨리 없어졌으면 좋겠지만 아직도 IE 크로스브라우징을 원하시는 클라이언트분들이 많죠..ㅠㅠ
오늘은 항상 궁금했던 IE의 이미지 깨짐현상에 대해 알아보았습니다.

![image](/assets/img/post/css-imgBugInIE/1.png)

크롬 브라우져에서는 꺠끗한 화질이 지만 IE에서는 저런 이미지 계단현상이 일어나는 것을 확인 할 수 있습니다.

## 해결방법

결론적으로 이 버그는 원본 이미지를 50% 이상 축소하여 표시할 경우 발생합니다.

아래 절차를 따르면 발생하는 현상을 직접 보실 수 있습니다.

1. [w3schools.com 예제](https://www.w3schools.com/code/tryit.asp?filename=FM8XMWR0LREF)

2. 페이지 로딩 후 좌측 상단의 Run 클릭

3. 우측 Result 창에 이미지 계단 현상 확인

보시면 원본 이미지는 1500px * 1500px 의 이미지를 180px * 180px 로 축소하여 표시하는 예제입니다.

좌측창의 코드 중 style 속성에서 width 와 height 를 증가시켜보면

원본 이미지의 절반 크기인 750px * 750px 부터 계단 현상이 사라지기 시작합니다.

***즉, 표현하고자하는 이미지의 웹에서의 크기를 생각해서 원본 이미지를 수정해야 해결됩니다.***

![image](/assets/img/post/css-imgBugInIE/2.png)

저는 이런 아날로그 적인 해결방법밖에 찾지 못했지만 ㅠㅠ

코드로서 해결할 수 있는 방법을 아시는분은 댓글 부탁드립니다!!





