---
layout: post
comments: true
title: '[JS] Logical Operator(논리연산자) &&, ||'
categories: [js]
tags: [javascript]
date: 2019-04-01
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## 들어가기

조건문의 조건을 설정할때 우리는 종종 And 나 Or 이라는 것이 필요할 때가 있죠.

그럴때 **Javascript**에서 사용하는 연산자는 바로 &&, \|\| 이며 Logical **Operator(논리연산자)**라 불립니다.

이 글에서 사용하면 코드를 정말 간단하게 할 수 있는 논리연산자를 다루어 보겠습니다.

## 기본 원리

기본 원리를 예시문과 같이 정리하면 다음과 같습니다.

- **A && B**

A를 true로 변환할 수 있는 경우 B을 반환하고, 그렇지 않으면 A를 반환합니다.

- **A \|\| B**

A을 true로 변환할 수 있으면 A을 반환하고, 그렇지 않으면 B를 반환합니다.

참고로 false로 표현될 수 있는 표현의 예는 다음과 같습니다.

1. null
2. NaN
3. 0
4. 빈 문자열 ("", '', ``)
5. undefined

**객체나 배열 데이터의 경우 존재하면 반드시 True, 존재하지 않으면 undefined이니 False가 되겠죠??**

## 사용 예제

실제 코드를 작성하다보면 이러한 논리연산자를 사용하여 코드를 확 줄일 수가 있습니다.

```javascript
function myProfile(name) { ​
  if (!name) {
    name = "Junhyeok Heo";
  } else {
    name = name;
  }
}
```

위의 코드에서는 조건문을 사용하여 name파라미터가 없을때를 처리해주는 상황인데요.

다음과 같은 논리 연산자를 사용하여 코드를 축약할 수 있습니다.

```javascript
function myProfile(name) {
  name = name || 'Junhyeok Heo'
}
```

\|\|앞의 **name값이 존재하지 않다면** null이거나 undefined 값,

즉 **false**가 오기때문에 \|\| 뒤에 명시한 'Junhyeok Heo'가 오는 로직입니다!

비슷한 예제로 &&를 활용한 코드를 보겠습니다.

```javascript
function isAdult(age) {
  if (age && age > 18) {
    return true;
  } ​else {
    return false;
  }
}

// 논리 연산자를 사용하여 코드를 refactoring하면

function isAdult(age) {
  return age && age > 18;
}
```

## 심화 예제

논리 연산자를 2개이상 사용하면 조금 복잡하지만 역시 간단한 코드를 만들 수 있습니다.

```javascript
if (userName) {
  logIn(userName)
} else {
  signUp()
}

// 논리 연산자를 사용하여 코드를 refactoring하면

userName && logIn(userName)) || signUp();
```

위의 코드는 userName이 true이면 userName을 parameter로 이용한 logIn function을 call하고,

userName이 false이면 signUp function을 call하는 예제입니다.

```javascript
var userID;
​if (userName && userName.loggedIn) {
  userID = userName.id;
}
​else {
  userID = null;
}

// 논리 연산자를 사용하여 코드를 refactoring하면

var userID = userName && userName.loggedIn && userName.id
```

위의 코드는 순서대로 userName을 체크, true이면 userName.loggedIn을 체크, true이라면 userName.id 를 userID에 초기화합니다.

만약 userName이 false이라면 null을 userID에 초기화합니다.

## 참고

[12 Simple (Yet Powerful) JavaScript Tips](http://javascriptissexy.com/12-simple-yet-powerful-javascript-tips/)<br>
[MDN Logical_Operators](<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/%EB%85%BC%EB%A6%AC_%EC%97%B0%EC%82%B0%EC%9E%90(Logical_Operators)>)
