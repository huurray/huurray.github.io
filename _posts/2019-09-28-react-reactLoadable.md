---
layout: post
comments: true
title: '[REACT] Code Splitting을 위한 react-loadable'
categories: [react]
tags: [react]
date: 2018-09-28
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}

## Bundling?

<br>
대부분의 React 앱은 Webpack 이나 Browserify 와 같은 도구를 사용하여 "번들된"파일을 갖게 됩니다. <br>
단일 파일들을 병합하여 한번에 전체 앱을 로드하기 때문입니다.<br><br>

![image](/assets/img/post/reactLoadable/1.png){:.lead}
(크롬 개발자도구의 network 탭에서 bundle.js를 확인할 수 있어요.)

하지만 앱이 커지면 번들링 된 앱은 굉장히 무거워지기 때문에 Code Splitting 과정을 거쳐 앱을 로드시키는데 너무 많은 시간이 걸리게 하는걸 방지 할 수 있어요!
<br><br>
사용자가 현재 필요로하는 것만 lazy-load 라고 한다니 이름 참 잘 지었네요.


## react-loadable
<br>
react-loadable Code Splitting을 위한 라이브러리로 react 공식문서에서도 추천하고 있는 React-Friendly API입니다.<br>
(서버 로딩이 아닌 컴포넌트 로딩을 위한 도구입니다. )
<br><br>
저도 다음 프로젝트부터는 사용하기 위해서 공부를 하였기에 기초적인 부분을 소개하려 합니다.
<br>
기본적으로 처음 앱 로드가 intialize될 때 전체 앱을 로드하는 것을 막아주는 역할을 합니다.
<br>

```javascript
import Loadable from 'react-loadable';

const LoadableOtherComponent = Loadable({
  loader: () => import('./OtherComponent'),
  loading: () => <div>Loading...</div>
});

const MyComponent = () => <LoadableOtherComponent />;
```

어떤 기점을 가지고 코드를 분할하는 것은 프로젝트마다 다르겠지만 기본적으로 route구분으로 분할 때가 많습니다.
<br>
그럴땐 react-loadable 역시 route를 정의하는 상위 컴포넌트에서 한번에 정해주는 것이 좋습니다.

```javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Loadable from 'react-loadable';

const Loading = () => <div>Loading...</div>;

const Main = Loadable({
  loader: () => import('./routes/Main'),
  loading: Loading,
});

const About = Loadable({
  loader: () => import('./routes/About'),
  loading: Loading,
});

const App = () => (
  <Router>
    <Switch>
      <Route exact path="/" component={Main}/>
      <Route path="/about" component={About}/>
    </Switch>
  </Router>
);
```
![image](/assets/img/post/reactLoadable/2.png){:.lead}
(Code Spiltting을 통해 0.chunk.js가 생긴 것을 볼수 있습니다.)

## Issue

그러나 중요한것은 규모가 어느정도 되는 앱이라는 전제 조건에서 사용하는 것이 효율적이라는 점입니다.

![image](/assets/img/post/reactLoadable/3.png){:.lead}

react-loadable을 적용한 것과 적용하지 않은 CPA(create-react-app)을 비교해보면 번들되는 파일이 더 많아지는 것을 확인할 수 있습니다.
<br>
<br>
또한,
<br>
main page에서 사용되는 용량은 113+83KB(0.chunk.js+main.js)와 222KB(main.js)로,
<br>
단 20KB정도의 차이만 보이는 것을 볼 수 있습니다. 

<br>
이러한 잇슈에 대해서는 좀 더 토론할 여지가 있어보입니다.
