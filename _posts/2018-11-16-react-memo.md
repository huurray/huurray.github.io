---
layout: post
comments: true
title: '[REACT] React.memo와 React.lazy'
categories: [react]
tags: [react]
date: 2018-11-16
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}


<br>
오늘은 React 16.6버전의 New Features! memo와 lazy에 대해서 공부해 보고자 합니다.

## React.memo

***React.memo***는 기본적으로 ***HoC***입니다. PureComponent와 같이 동일한 입력값이 발생할때
호출값을 저장하고 재실행 되지 않게끔하게해서 ***최적화***를 도와준다고 합니다!

어떻게? 얕은 비교(shallow compare)를 통해서 props값에 변화가 있을때만! ***리렌더링***합니다.

```javascript
const MyComponent = React.memo(function MyComponent(props) {
  /* render using props */
});
```

또한, ***두번째 인수***로 비교함수를 지정해 줄수도 있습니다!

```javascript
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
export default React.memo(MyComponent, areEqual);
```

이 방법은 성능 최적화방법으로만 존재합니다. 
그렇지만 render가 버그를 유발할 수 있으므로 렌더링을 "방지"하기 위해 의존하지는 말라는 공식문서의 글을 보았습니다!(***shouldComponentUpdate 대용은 안된다!***)

## React.lazy

저번에 블로그에 ***code splitting***을 위해 react-loadable을 사용하는 방법을 소개해드렸었는데요!
비슷한 방식으로 사용할수 있는 메서드가 react에 내장되었습니다. 바로 ***lazy***와 ***Suspense***입니다!

***중요한 점***
<br>
그러나!!! 공식문서에서는 아직 서버사이드 앱에서의 code splitting을 여전히 React Loadable을 추천합니다.

```javascript
const LazyCount = React.lazy(() => import("./LazyCount");
function MyComponent() {
  return (
    <div>
       <LazyCount />
    </div>
  );
};
```

사용방법이 loadable과 비슷하죠? 그러나 위에 코드를 입력하면 에러가 발생하는데, <br>
LazyCount를 동적으로 가져오기 전에 MyComponent가 렌더 되기 때문입니다. <br>
Suspense를 이용해서 그전을 처리할 수 있는 대체를 정해줘야합니다. 

```javascript
const LazyCount = React.lazy(() => import("./LazyCount");
function MyComponent() {
  return (
    <div>
      <React.Suspense fallback={<div>loading...</div>}>
        <LazyCount/>
      </React.Suspense>
    </div>
  );
};
```


##참고

[react 공식문서 react.memo](https://reactjs.org/docs/react-api.html#reactmemo)<br>
[react 공식문서 react.lazy](https://reactjs.org/docs/code-splitting.html#reactlazy)