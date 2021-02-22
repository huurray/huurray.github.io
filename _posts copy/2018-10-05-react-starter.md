---
layout: post
comments: true
title: '[REACT] Firebase를 탑재한 React Starter Kit 구현'
categories: [react]
tags: [react]
date: 2018-10-05
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

![image](/assets/img/post/react-starter/0.png)

## Firebase?

**Firebase**는 Google 에서 서버리스 클라우딩 서비스를 제공하는 대표적인 SaaS(Software as a Service)입니다. <br>
**서버리스**는 24 시간 꽉찬 구동이 필요없는 경우 필요한만큼 필요한 경우에만 서버 규모가 할당되어 결론적으로 요금을 절약 할 수 있고, 보안 등의
이유로 인기를 끌고 있습니다. <br>또한 저같은 초보 프론트엔드 개발자가 백엔드 서버를 구현하고 관리하는 것에 대한 부담을 줄여주어서
온전히 자신의 서비스 로직에만 집중할수 있게되는 것이 큰 장점입니다.<br>
<br>
또한 최근에 FaaS 인 Cloud Functions 가 연동되면서 더욱 활용가능한 서비스가 되었습니다.
<br>
**Firebase**를 이용해 Auth, 즉 로그인 기능을 구현하는것은 굉장히 쉽지만 [React-Firebaseui](https://github.com/firebase/firebaseui-web-react)는 그 쉬운걸 더 쉽게 만들어 줍니당.
이번에는 react-firebaseui 를 이용하도록 하겠습니다.

## What is in?

저는 본 프로젝트에

- React
- Redux
- Typescipt
- React-Router-V4
- Firebase
- React-Firebaseui
- Styled-components

를 탑재하여 SPA 앱을 구현하겠습니다. 각 항목에 대한 자세한 설명은 생략하겠습니다!

## Set

JavaScript 는 weakly typed 언어 이기때문에 변수가 숫자였다가 또 문자열이였다가 할 수 있죠.
때문에 타입스크립트를 사용해서 Type 을 지정해줘서 잠재적인 에러를 막을 수 있습니다.
<br>
또한, react 의 경우 PropTypes 를 대신해서 사용할 수 있기때문에 일석이조입니다!
<br><br>
저는 **create-react-app**을 사용하여 앱을 initialize 하겠습니다. 여기서 typescript 를 탑재하기 위해
뒤에 "--scripts-version=react-scripts-ts" 를 넣어주겠습니다.

> create-react-app firebase-react-starter --scripts-version=react-scripts-ts

index.css, App.css, App.test.js, logo.svg 등 쓸데 없는 파일을 지워주고 앞에서 사용하기로한 package 들을 받아줍니다!
package 의 type 을 알 수 없다고 나오면 @types/를 붙여 package 의 type 을 받아야합니다.

![image](/assets/img/post/react-starter/1.png)

## Process

먼저 React-router-v4 를 이용해 Main, sub 페이지를 구현하고 로그인을 위한 Signin 페이지를 넣었습니다.
<br>
각 컴포넌트는 페이지 별로 구분하고 Container 와 Presenter 를 구분해놨는데 이렇게 하면 state 와 동적함수와 스타일 코드를 구분해서 사용할 수 있는 장점이 있습니다.
<br>
![image](/assets/img/post/react-starter/2.png)
(저는 요즘에 이런식으로 많이 짭니다.)
<br><br>
Firebaseui 를 사용하기 때문에 로그인을 구현하는 것은 굉장히 쉽습니다. <StyledFirebaseAuth>을 그냥 사용하면 되는데 이때 옵션값으로 어떤 플랫폼과 연동할 것인지 선택할 수 있고 로그인 후 콜백함수를 지정해 줄 수도 있습니다. 물론 firebase 에서 프로젝트를 만들고 [Auth 관련 설정](https://console.firebase.google.com)을 해주셔야합니다. (이글에서는 생략하겠습니다.)

```javascript

// components/SignIn/presenter.tsx
import * as React from 'react';
import * as firebase from 'firebase';
import StyledFirebaseAuth from 'react-firebaseui/StyledFirebaseAuth';

interface Props {
  userActions: any;
  history: any;
}

export default class extends React.PureComponent<Props> {
  render() {
    const uiConfig = {
      signInFlow: 'popup',
      signInOptions: [
        // firebase.auth.EmailAuthProvider.PROVIDER_ID,
        // firebase.auth.PhoneAuthProvider.PROVIDER_ID,
        // firebase.auth.GoogleAuthProvider.PROVIDER_ID,
        // firebase.auth.TwitterAuthProvider.PROVIDER_ID,
        // firebase.auth.GithubAuthProvider.PROVIDER_ID,
        firebase.auth.FacebookAuthProvider.PROVIDER_ID
      ],
      callbacks: {
        signInSuccessWithAuthResult: data => {
          this.props.userActions.getUser({
            displayName: data.user.displayName,
            email: data.user.email,
            photoURL: data.user.photoURL
          });
          this.props.history.replace('/');
        }
      }
    };
    return (
      <div>
        <StyledFirebaseAuth
          uiConfig={uiConfig}
          firebaseAuth={firebase.auth()}
        />
      </div>
    );
  }
}
```




## Redux

로그인에서 콜백함수로 **getUser()**라는 리덕스 액션함수를 넣어주었는데 리덕스 스토어에 가져온 로그인 정보를 넣어주는 함수입니다.
<br>
redux 의 initialState 에는 유저정보를 저장하는 data 와 로그인 상태를 구분하는 isSignedIn 를 넣어 주었습니다. firebase 에는 firebase.auth().onAuthStateChanged()라는 로그인상태 체크 메서드가 있어 매번 사용해 줄 수도 있지만 상태 체크에 딜레이가 있기 때문에 처음 어플리케이션이 켜질때나 로그인 시, 로그아웃 시 등에 파악하고 리덕스에 저장하는 것이 더 효율적입니다.

```javascript

// redux/user/reducer.js

import { Action } from '../../types';
import { User } from '.';

interface State {
  isSignedIn: boolean;
  data: User;
}

const initialState = {
  isSignedIn: false,
  data: {
    displayName: '',
    email: '',
    photoURL: ''
  }
};

function reducer(state: State = initialState, action: Action) {
  switch (action.type) {
    case 'USER_GET':
      return (state = {
        ...state,
        isSignedIn: true,
        data: { ...state.data, ...action.payload }
      });
    case 'USER_LOGOUT':
      return (state = { ...state, ...initialState });
    default:
      return state;
  }
}

export default reducer;
```

## HOC

타 플랫폼 로그인을 통해 가져오는 정보는 한정적입니다. 자체 로그인을 사용하여도 회원가입에서 많은 정보를 요구하면 사용자의 접근성이 낮아지게 되죠. 그래서 요즘은 보통 로그인을 간단하게 하고 앱 내부에서 추가적인 회원정보를 받습니다. 이 때문에 회원정보를 저장할 때는 확장성을 고려해야 됩니다.
<br><br>
그래서 저는 HOC 형태로 확장가능한 것들을 넣어 주었습니다. withAuth 와 withStyles 는 App 컴포넌트를 감싸서 상위에서 전역적으로 실행되는 것을 도와주고, 코드를 따로 분리해서 확장이 편리하도록 하였습니다.

<br><br>
![image](/assets/img/post/react-starter/3.png)
[firebase-react-starter](https://github.com/huurray/firebase-react-starter) <- 전체 코드

