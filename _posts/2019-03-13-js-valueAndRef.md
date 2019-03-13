---
layout: post
comments: true
title: '[JS] Value Types and Reference Types 비교'
categories: [js]
tags: [javascript]
date: 2019-03-13
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## 들어가기

**JavaScript**의 데이터에는 **5가지 타입**의 value(_Boolean, null, undefined, String, 그리고 Number_)와
**3가지 타입**의 reference(_Array, Function, 그리고 Object_)가 존재합니다!

두가지 데이터 개념은 처리과정에서 확실하게 구분되는데요. 어떤 차이점이 있는지 알아보도록 합니다.

## Value Type VS Reference Type

먼저 **Value** 값들을 변수로 정의헤 보겠습니다.

```javascript
var x = 10
var y = 'abc'
var z = null
```

x라는 변수는 10이며, y라는 변수는 'abc', z라는 변수는 null이 되었습니다.

```javascript
var x = 10
var y = x

x = 50

console.log(y)
```

그러면 value타입의 데이터들을 위와 같이 처리하면 y값은 어떻게 될까요?

50이 될까요? **아닙니다!!** y는 여전히 10인데요.
이것이 reference타입과의 가장 큰 차이점입니다.

바로 이 value타입의 데이터들은 처리과정에서 반드시 **COPY**, 즉 복사되서 가져가지기 떄문에 원데이터에 영향이 없는것입니다!

```javascript
var array = ['apple', 'banana']
var newArray = array

array.push('mango')

console.log(array)
//['apple', 'banana', 'mango']
```

같은 방식으로 **reference타입** 데이터인 **Array**의 처리 결과를 볼까요?
원데이터가 변한 것을 볼수 있습니다. 말그대로 **참조**(reference)하기 때문입니다!

## == and ===

위 개념을 숙지하고 있지않으면 비교? 연산자를 할때 굉장히 혼동이 될 수 있는데 예를들면,

```javascript
var arrRef = [’Hi!’];

var arrRef2 = arrRef;

console.log(arrRef === arrRef2); // -> true

var arr1 = ['Hi!'];

var arr2 = ['Hi!'];

console.log(arr1 === arr2); // -> false
```

이런경우들 떄문이죠! 값이 같아보이지만 참조가 아닌 다른 배열이라면 **다르다**는것을 알 수 있습니다! 객채도 마찬가지겠죠?

## 참고

[Explaining Value vs. Reference in Javascript](https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0)
[Primitive Types & Reference Types in JavaScript](https://gist.github.com/branneman/7fb06d8a74d7e6d4cbcf75c50fec599c)
