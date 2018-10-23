---
layout: post
comments: true
title: '[JS] ES6(ES2015) Array Helper정리'
categories: [js]
tags: [javascript]
date: 2018-10-16
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## ES6 Array Helper

2015년에 javascripts는 큰 발전이 있었습니다. 바로 **ECMAScript2015**가 발표된것인데요! <br>ECMA라는 단체에서 기존의 결점을 보완한 표준 자바스크립트 버전을 매년 발표하고 업데이트하기로 정해서
6, 7 같은 넘버링보다는 연도를 붙이기로 정했다네요. 따라서 ES6는 EcmaScript2015 와 같은 말이랍니다. <br><br>오늘은 ES6의 **Array Helper**에 대해 정리하려고 합니다. 대부분의 메서드는 ECMAScript5.1에서도 사용할 수 있었고 실제로 ES6에서 추가된 것은 find하나입니다. 

(실은 reduce에 대해서 잘 몰라서 공부할겸 다 정리하는중..)
## forEach

**forEach** 메서드는 배열의 각 아이템을 iterator 객체로 반복합니다. 이때 배열의 반복은 array.length번 반복하면 됩니다.

```javascript
const arr = [1, 2, 3, 4];
arr.forEach(item => console.log(item));

// 1,2,3,4
```

## map

**map** 메서드는 forEach 메서드와 비슷하지만 중요한 다른점은 각각 배열 원소들에 대해서 전달받은 함수를 호출하고 그 결과를 모아서 새로운 배열을 만듭니다. <br>
이때 return이 없는 함수인 경우 원래 배열의 원소 갯수만큼 undefined로 채워진 배열이 만들어집니다.

```javascript
const arr = [{ name: 'James', age: 18 }, { name: 'Alice', age: 20 }];
const person_names = arr.map(person => person.name);
console.log(person_names);


//['James','Alice']
```

## filter

**filter** 메서드는 배열에서 내가 원하는 조건을 충족한 배월 원소들로만 추려 배열을 만들 수 있습니다.

```javascript
const fruits = [
  { name:'banana', color:'yellow' },
  { name:'watermelon', color:'red' },
  { name:'orange', color:'orange' }
 ]
 const filteredFruits = fruits.filter(fruit => fruit.color === ‘yellow’);
 console.log(filteredFruits)

//[{name:'banana',color:'yellow'}]
```

## find

**find** 메서드는 배열에서 내가 원하는 조건을 충족한 하나의 배열 원소를 배출합니다.
이때 조건에 부합한 원소가 여러개여도 가장 처음 찾은 원소를 배출하고 메서드는 종료됩니다.(깔끔)
<br>비슷한 메서드로 **findIndex**가 있는데 조건에 맞는 배열 원소의 index번호를 알려줍니다. 만약 조건에 일치하는게 없다면 -1을 리턴합니다.

```javascript
let x = [{ name: 'jay', age: 1 }, { name: 'hulu', age: 30 }];
const result = x.find(obj => obj.name === 'jay');
console.log(result);

//{ name:'jay', age:1 }
```


## some

**some** 메서드는 배열의 모든 원소들이 해당 조건에 부합하는지 검사합니다. 배열의 원소들에 대해서 함수를 실행하게 되고 하나라도 조건에 부합하면 true, 부합하지 않으면 false를 리턴합니다.

```javascript
const names = ['Alex', 'Jimmy', 'John'];
const result = names.every(name => name.length > 4);
console.log(result);

//false
```

## every

**every** 메서드는 배열의 모든 원소들이 해당 조건에 부합하는지 검사합니다. 배열의 원소들에 대해서 함수를 실행하게 되고 모두다 조건에 부합하면 true, 하나라도 부합하지 않으면 false를 리턴합니다.

```javascript
const names = ['Alex', 'Jimmy', 'John'];
const result = names.every(name => name.length > 4);
console.log(result);

//false
```

## helper's parameters

위에 나열한 helper들의 콜백함수는 전부 같은 인자를 가집니다. 
<br> **첫번째는 처리할 현재 요소,**
<br> **두번째는 처리할 현재 요소의 index,**
<br> **세번째는 helper를 호출한 원본 array입니다.**

이 인자들을 사용하여 무궁무진한 반복문 코드를 사용할 수 있겠죠?

## reduce

**reduce** 메서드는 가장 유연한 helper이며 가장 복잡합니다. <br>reduce는 쉽게 말해 배열의 각 요소에 대해 하나의 단일 값(single value)으로 줄이는 메서드 입니다.
<br>
따라서 위의 helper들의 콜백함수 인자에서 **accumulator인자** 한개가 추가됩니다.

<br> **첫번째는 accumulator로 누적값,**
<br> **두번째는 처리할 현재 요소,**
<br> **세번째는 처리할 현재 요소의 index,** 
<br> **네번째는 reduce를 호출한 원본 array입니다.**

배열안에 요소의 숫자를 모두 더하는 함수를 만들어 보겠습니다. 먼저 for을 사용한 반복문입니다.

```javascript
const numbers = [1, 2, 3, 4, 5];
let sum = 0;
for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}
console.log(sum);

//15
```

reduce의 첫번째 인자로 콜백함수, 두번째 인자로 initialValue를 지정해 줄 수 있습니다. 지금은 0이 initialValue가 되겠죠? 

```javascript
const sum = numbers.reduce((sum, numbers) => sum + numbers, 0);
console.log(sum);

//15
```
이 구문을 reduce를 사용한 코드입니다. 깔끔하게 위의 코드가 정리되었습니다.
<br>

조금 더 reduce를 활용해 보겠습니다. 데이터의 성별을 숫자합으로 줄인 코드입니다.

```javascript
let gender = [
  { type: '남자' },
  { type: '여자' },
  { type: '남자' },
  { type: '남자' },
  { type: '여자' }
];

const result = gender.reduce((genderCount, gender) => {
  genderCount[gender.type] = (genderCount[gender.type] || 0) + 1;
  console.log(genderCount);
  return genderCount;
}, {});

console.log(result);

//{남자: 3, 여자: 2}
```

## 정리

지금까지 Array Helper에 대하여 정리하였습니다.<br> for문을 사용하는 것보다 가독성이 높아진 것을 볼 수 있습니다.<br> 그러나 반복문의 남용은 성능저하를 일으킬 수 있기때문에 꼭 필요할때만 사용해 줍시다!