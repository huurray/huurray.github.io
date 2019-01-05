---
layout: post
comments: true
title: '[JS] Javascript는 도대체 어떻게 실행될까?'
categories: [js]
tags: [javascript]
date: 2019-01-05
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## 들어가기

**JavaScript**를 다루기 위해서 기본적인 문법과 API 사용법에 대해서 익히는 것도 중요하지만, 고급 개발자가 되려면 엔진단에서의 자바스크립트 동작원리도 알아야 한다고 생각하는데요.

복잡한 Javacript엔진의 정확한 이해도 중요하지만 이번에는 이 언어의 몇가지 핵심적인 내용만 간단히 추려서 적어 보겠습니다.

바로 **Call Stack, Event Loop, Non-Blocking**입니다.

## Call Stack

보통 자바스크립트는 "**싱글 쓰레드 기반**이고 콜백 큐를 사용한다" 라고 하는데요.

간단하게만 이야기해서 작업장이 하나이고 그 때문에 한 번에 한 작업만 할 수 있기 때문에 나머지 호출된 작업을 **줄(Queue)** 세우는 방식으로 일을 처리한다는 뜻!

또한, 좁은 통에 공을 넣으면 가장 마지막에 넣은 공을 먼저 빼야하듯이 쓰레드에 쌓인 스택 역시 가장 마지막에 들어온 스택이 가장 먼저 실행됩니다.
다시 말해, 실행된 함수 끝나면 또 스택의 가장 위쪽의 함수를 꺼내는 것이고 스택이 없을때까지 반복하는 것이죠!

예제 코드로 확인해 봅시다.

```javascript
function multiply(a, b) {
  return a * b;
}
function square(n) {
  return multiply(n, n);
}
function printSquare(n) {
  var squared = square(n);
  console.log(squared);
}
printSquare(4);
```

printSquare 함수가 실행되고 순차적으로 square, multiply 함수가 실행되고 마지막으로 콘솔이 찍히게 됩니다.

처리과정은 다음과 같습니다. gif짤로 만들어봤습니다.

![image](/assets/img/post/js-callStack/2.gif)

또, 호출 스택이 최대 크기가 되면 스택을 날려버린다고 표현하는데요. 이는 반복문 코드를 광범위하게 테스트하지 않고 실행했을 때 자주 발생합니다.

아래 코드를 보시면

```javascript
function foo() {
  foo();
}

foo();
```

엔진에서 이 코드를 실행할 때, foo()에 의해서 foo 함수가 호출됩니다. 그런데 여기서 foo 함수가 반복적으로 자신을 다시 호출하는 재귀 호출을 수행합니다. 그러면 매번 실행할 때마다 호출 스택에 foo() 가 쌓이게 됩니다.

아래와 같이 말이죠.

![image](/assets/img/post/js-callStack/1.png)

## Event Loop

그럼 **비동기 콜백(asynchronous callback)**은 어떻게 이뤄질까요?

다음 코드를 보겠습니다.

```javascript
console.log('Hi');

setTimeout(function() {
  console.log('there');
}, 5000);

console.log('JSconf');
```

이 코드는 먼저 'Hi'라는 콘솔이 찍히고, setTimeout이 실행되고 'JSconf'라는 콘솔이 찍히고 5초뒤에 'there'이라는 콘솔이 찍힐 겁니다.

그러면 setTimeout 함수가 아직 끝나지 않았는데 어떻게 'JSconf'라는 콘솔이 찍혔을까요??

우리가 이걸 동시에 할 수 있는 이유는 바로!! **브라우저가 단순 런타임 이상의 작업을 하기 때문입니다.**

![image](/assets/img/post/js-callStack/3.png)

자바스크립트는 한번에 하나의 일 밖에 할 수 없지만, **브라우저가 Web API같은 것들을 제공합니다.**
이들은 자바스크립트에서 호출 할 수 있는 쓰레드를 제공하죠.

Node.js 또한 이와같은 방법으로 실행되죠!

따라서 위의 코드는 실제로 이와같은 방식으로 진행됩니다.(gif짤입니다.)

![image](/assets/img/post/js-callStack/4.gif)

setTimeout이 실행되면 브라우저로 넘어가 타이머를 실행시키고 카운트 다운을 시작합니다.
이말은 setTimeout의 호출 자체는 완료되었고, **우리는 스택에서 지울 수 있게되는겁니다.**

또 카운트다운이 끝나면 해당 결과물은 **Task Queue**에 들어갑니다.

**Event Loop는 Call Stack과 Task Queue를 주시하며 스택이 비어있으면, Queue에 있는 콜백들을 순서대로 쌓아 주는 역할을 하는 것입니다.**

따라서 'JSconf'라는 콘솔이 먼저 실행되고 카운트 다운이 끝나면 'there'이 찍히는 것이죠!!

마지막으로 이 코드를 봅시다.

```javascript
console.log('start');

setTimeout(function() {
  console.log('inside');
}, 0);

console.log('end');
```

이 코드의 결과물이 'start', 'end', 'inside' 순으로 되는 것 역시 setTimeout이 브라우저단으로 넘어갔다가 Task Queue에서 다시 자바스크립트 엔진으로 돌아오기 떄문이겠죠??

## Non-Blocking

Non-Blocking은 I/O 방식 중 하나인데, I/O는 컴퓨터가 데이터를 주고받을 때의 **입출력 프로세스**를 말합니다.

앞서 이야기한 Event Loop를 잘 활용하면 오래걸리는 작업을 효율적으로 처리할 수 있습니다.

이방식이 바로! **Non-Blocking방식** 이라고 하죠!

Non-Blocking은 Blocking방식과 비교하며 이해하는게 가장 빠를겁니다.

![image](/assets/img/post/js-callStack/5.png)

위 사진은 비동기 함수(getUserSync)와 동기 함수(getUser)가 실행되는 프로세스를 도식화 한 것인데요.

**Blocking방식**은 한 함수가 프로세스가 시스템을 호출 하고나서 결과가 반환되기까지 다음 처리로 넘어가지 않고,
반대로 **Non-Blocking 방식**은 마치 멀티 쓰레드를 가진것처럼 시스템을 호출한 직후에 프로그램으로 제어가 다시 돌아와서 시스템 호출의 종료를 기다리지 않고 다음 처리로 넘어갈 수 있습니다.

## 마무리

Javascript가 뭐냐? 라고 물어볼 때 항상 등장했던 **싱글쓰레드, non-blocking방식, 콜스택, 비동기-동기 언어 등의 용어**에 대해 궁금증이 좀 풀리는 느낌입니다.

이 언어의 몇가지 특성을 명확하게 알고 짜는 코드와 그렇지 않은 코드가 다를거라고 생각합니다. 제 글을 보고 조금이라도 도움이 되셨으면 좋겠습니다~~

## 참고

[2014 JSconf 강의 중 일부](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
