---
layout: post
comments: true
title: '[REACT] Firebase를 탑재한 React Starter Kit - 01'
categories: [react]
tags: [react]
date: 2018-10-01
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}

## Firebase-React-Starter

<br>
웹 페이지 개발을 시작할 때 로그인이나 라우터, 리덕스 연결 등은 항상 반복되는 일입니다. <br>
반복은 싫어요ㅠ 그래서 기본적인 Starter Kit을 만들어서 <br>
사용하고, 업데이트 하면 어떨까 하는생각을 하자마자 실행에 옮겨봤습니다. <br>
오늘은 그 과정을 간략하게 기록하고자 합니다.<br><br>
[firebase-react-starter](https://github.com/huurray/firebase-react-starter) <- 전체 코드

## Firebase?

**Firebase**는 Google에서 서버리스 클라우딩 서비스를 제공하는 대표적인 SaaS(Software as a Service)입니다. <br>
**서버리스**는 24시간 꽉찬 구동이 필요없는 경우 필요한만큼 필요한 경우에만 서버 규모가 할당되어 결론적으로 요금을 절약 할 수 있고, 보안 등의
이유로 인기를 끌고 있습니다. <br>또한 저같은 초보 프론트엔드 개발자가 백엔드 서버를 구현하고 관리하는 것에 대한 부담을 줄여주어서 
온전히 자신의 서비스 로직에만 집중할수 있게되는 것이 큰 장점입니다.<br>
<br>
**Firebase**는 최근에 FaaS인 Cloud Functions가 연동되면서 더욱 활용가능한 서비스가 되었습니다.
<br>
Firebase를 이용해 Auth, 즉 로그인 기능을 구현하는것은 굉장히 쉽습니다.
그러나 [React-Firebaseui](https://github.com/firebase/firebaseui-web-react)는 그 쉬운걸 더 쉽게 만들어 줍니다.
이번에는 react-firebaseui를 이용하도록 하겠습니다.

## What is in?

저는 본 프로젝트에
* React
* Redux
* Typescipt
* React-Router-V4
* Firebase
* React-Firebaseui
* Styled-components

를 탑재하여 SPA앱을 구현하겠습니다. 각 항목에 대한 자세한 설명은 생략하겠습니다!

## Set

JavaScript 는 weakly typed 언어 이기때문에 변수가 숫자였다가 또 문자열이였다가 할 수 있죠.
때문에 타입스크립트를 사용해서 Type을 지정해줘서 잠재적인 에러를 막을 수 있습니다.
<br>
또한, react의 경우 PropTypes를 대신해서 사용할 수 있기때문에 일석이조입니다!
<br><br>
저는 **create-react-app**을 사용하여 앱을 initialize 하겠습니다. 여기서 typescript를 탑재하기 위해
뒤에 "--scripts-version=react-scripts-ts" 를 넣어주겠습니다.

> create-react-app firebase-react-starter --scripts-version=react-scripts-ts

index.css, App.css, App.test.js, logo.svg 등 쓸데 없는 파일을 지워주고 앞에서 사용하기로한 package들을 받아줍니다!
package의 type을 알 수 없다고 나오면 @types/를 붙여 package의 type을 받아주세요.

![image](/assets/img/post/react-starter/1.png){:.lead}


## File Tree

![image](/assets/img/post/react-starter/2.png){:.lead}

먼저 React-router-v4를 이용해 Main, sub페이지를 구현하고 로그인을 위한 Signin페이지를 넣었습니다.
<br>
Hoc를 이용하여 처음 어플리케이션이 Initialize될때 유저 정보를 받아 올 수 있도록 하고(withAuth), 앱 전체에 styled-components의 theme을 사용할 수 있도록 하였습니다.(withStyle)
<br>
그리고 데이터는 redux의 store에서 일괄 관리합니다.
<br><br>

세부적인 코드 작성은 [REACT] Firebase를 탑재한 React Starter Kit - 02에 소개하겠습니다.
<br>


