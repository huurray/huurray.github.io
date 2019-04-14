---
layout: post
comments: true
title: '[JS] 좋은 개발자의 코드작성법(Clean Code) - (동시성 및 기타)'
categories: [js]
tags: [javascript]
date: 2019-04-15
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## 들어가기

Clean Code관련하여 **JavaScript**의 변수와 함수에 대해 글을 올렸었는데요.
[좋은 개발자의 코드작성법(Clean Code) - (변수, 함수)](https://huurray.github.io/js/2019/04/21/js-cleanCode1/)

이번에는 원문에서의 동시성 및 기타사항에 대한 항목들을 옮겨 보려합니다.
(원문은 글 아래에 링크되어 있습니다.)

이글은 소프트웨어 방법론에 관한 책들 중 Robert C. Martin's의 책인<br />
**Clean Code**에 있는 내용을 JavaScript 언어에 적용시켜 적은 글입니다.

**(이것은 책을 쓴 개발자의 개인적인 의견입니다. 법칙이 아닙니다!)**

## 동시성(Concurrency)

### 1. Callback 대신 Promise를 사용하세요

Callback 지옥을 벗어나기위해 Promise개념을 사용합니다.

**Bad:**

```javascript
require('request-promise')
  .get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(response => {
    return require('fs-promise').writeFile('article.html', response)
  })
  .then(() => {
    console.log('File written')
  })
  .catch(err => {
    console.error(err)
  })
```

**Good:**

```javascript
require('request-promise')
  .get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(response => {
    return require('fs-promise').writeFile('article.html', response)
  })
  .then(() => {
    console.log('File written')
  })
  .catch(err => {
    console.error(err)
  })
```

### 2. Async/Await은 Promise보다 더욱 깔끔합니다

Promise도 Callback에 비해 정말 깔끔하지만 ES2017/ES8에선 async와 await이 있습니다.

이들은 Callback에대한 더욱 깔끔한 해결책을 줍니다.

오직 필요한 것은 함수앞에 async를 붙이는 것 뿐입니다. 그러면 함수를 논리적으로 연결하기위해 더이상 then을 쓰지 않아도 됩니다.

만약 당신이 ES2017/ES8 사용할 수 있다면 이것을 사용하세요!

**Bad:**

```javascript
require('request-promise')
  .get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(response => {
    return require('fs-promise').writeFile('article.html', response)
  })
  .then(() => {
    console.log('File written')
  })
  .catch(err => {
    console.error(err)
  })
```

**Good:**

```javascript
async function getCleanCodeArticle() {
  try {
    const response = await require('request-promise').get(
      'https://en.wikipedia.org/wiki/Robert_Cecil_Martin'
    )
    await require('fs-promise').writeFile('article.html', response)
    console.log('File written')
  } catch (err) {
    console.error(err)
  }
}
```

## 에러 처리(Error Handling)

에러를 뱉는다는 것은 좋은 것입니다! 즉, 프로그램에서 무언가가 잘못되었을 때 런타임에서 성공적으로 확인되면

현재 스택에서 함수 실행을 중단하고 (노드에서) 프로세스를 종료하고 스택 추적으로 콘솔에서 사용자에게 그 이유를 알려줍니다.

### 1. 단순히 에러를 확인만 하지마세요

단순히 에러를 확인하는 것만으로 그 에러가 해결되거나 대응 할 수 있게 되는 것은 아닙니다.

console.log를 통해 콘솔에 로그를 기록하는 것은 에러 로그를 잃어버리기 쉽기 때문에 좋은 방법이 아닙니다.

만약에 try/catch로 어떤 코드를 감쌌다면 그건 당신이 그 코드에 어떤 에러가 날지도 모르기 때문에 감싼 것이므로

그에대한 계획이 있거나 어떠한 장치를 해야합니다.

**Bad:**

```javascript
try {
  functionThatMightThrow()
} catch (error) {
  console.log(error)
}
```

**Good:**

```javascript
try {
  functionThatMightThrow()
} catch (error) {
  // 첫번째 방법은 console.error를 이용하는 것입니다. 이건 console.log보다 조금 더 알아채기 쉽습니다.
  console.error(error)
  // 다른 방법은 유저에게 알리는 방법입니다.
  notifyUserOfError(error)
  // 또 다른 방법은 서비스 자체에 에러를 기록하는 방법입니다.
  reportErrorToService(error)
  // 혹은 그 어떤 방법이 될 수 있습니다.
}
```

### 2. Promise가 실패된 것을 무시하지 마세요

**Bad:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data)
  })
  .catch(error => {
    console.log(error)
  })
```

**Good:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data)
  })
  .catch(error => {
    // 첫번째 방법은 console.error를 이용하는 것입니다. 이건 console.log보다 조금 더 알아채기 쉽습니다.
    console.error(error)
    // 다른 방법은 유저에게 알리는 방법입니다.
    notifyUserOfError(error)
    // 또 다른 방법은 서비스 자체에 에러를 기록하는 방법입니다.
    reportErrorToService(error)
    // 혹은 그 어떤 방법이 될 수 있습니다.
  })
```

## 포맷팅(Formatting)

### 1. 일관된 대소문자를 사용하세요

JavaScript에는 정해진 타입이 없기 때문에 대소문자를 구분하는 것으로 당신의 변수나 함수명 등에서 많은 것을 알 수 있습니다.

이 규칙 또한 주관적이기 때문에 당신이 팀이 선택한 규칙들을 따르세요 중요한건 항상 일관성 있게 사용해야 한다는 것입니다.

**Bad:**

```javascript
const DAYS_IN_WEEK = 7
const daysInMonth = 30

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude']
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles']

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good:**

```javascript
const DAYS_IN_WEEK = 7
const DAYS_IN_MONTH = 30

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude']
const artists = ['ACDC', 'Led Zeppelin', 'The Beatles']

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

### 2. 함수 호출자와 함수 피호출자는 가깝게 위치시키세요

우리는 코드를 읽을때 신문을 읽듯 위에서 아래로 읽기 때문에 코드를 작성 할 때도 읽을 때를 고려하여 작성 해야합니다.

**Bad:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers')
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager')
  }

  getPeerReviews() {
    const peers = this.lookupPeers()
    // ...
  }

  perfReview() {
    this.getPeerReviews()
    this.getManagerReview()
    this.getSelfReview()
  }

  getManagerReview() {
    const manager = this.lookupManager()
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(user)
review.perfReview()
```

**Good:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee
  }

  perfReview() {
    this.getPeerReviews()
    this.getManagerReview()
    this.getSelfReview()
  }

  getPeerReviews() {
    const peers = this.lookupPeers()
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers')
  }

  getManagerReview() {
    const manager = this.lookupManager()
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager')
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee)
review.perfReview()
```

## 주석(Comments)

### 1. 주석은 단지 그 로직이 복잡하다는 것을 말 할 뿐입니다

주석을 다는것은 사과해야할 일이며 필수적인 것이 아닙니다. 좋은 코드는 코드 자체로 말합니다.

**Bad:**

```javascript
function hashIt(data) {
  // 이건 해쉬입니다.
  let hash = 0

  // lengh는 data의 길이입니다.
  const length = data.length

  // 데이터의 문자열 개수만큼 반복문을 실행합니다.
  for (let i = 0; i < length; i++) {
    // 문자열 코드를 얻습니다.
    const char = data.charCodeAt(i)
    // 해쉬를 만듭니다.
    hash = (hash << 5) - hash + char
    // 32-bit 정수로 바꿉니다.
    hash &= hash
  }
}
```

**Good:**

```javascript
function hashIt(data) {
  let hash = 0
  const length = data.length

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i)
    hash = (hash << 5) - hash + char

    // 32-bit 정수로 바꿉니다.
    hash &= hash
  }
}
```

### 2. 주석으로 된 코드를 남기지 마세요

버전 관리 도구(Git)가 존재하기 때문에 코드를 주석으로 남길 이유가 없습니다.

**Bad:**

```javascript
doStuff()
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good:**

```javascript
doStuff()
```

### 3. 코드 기록을 주석으로 남기지 마세요

버전 관리 도구(Git)가 존재하기 때문에 코드를 주석으로 남길 이유가 없습니다.

**Bad:**

```javascript
/**
 * 2016-12-20: 모나드 제거했음, 이해는 되지 않음 (RM)
 * 2016-10-01: 모나드 쓰는 로직 개선 (JP)
 * 2016-02-03: 타입체킹 하는부분 제거 (LI)
 * 2015-03-14: 버그 수정 (JR)
 */
function combine(a, b) {
  return a + b
}
```

**Good:**

```javascript
function combine(a, b) {
  return a + b
}
```

## 원문

[ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
