---
layout: post
comments: true
title: '[JS] 좋은 개발자의 코드작성법(Clean Code) - (변수, 함수)'
categories: [js]
tags: [javascript]
date: 2019-04-14
---

![headerimg](/assets/img/subcate/javascript.gif)
`javaScript`를 공부하고 기록하자!
{:.center}

## 들어가기

**JavaScript** 코드는 어떻게 작성하냐에 따라 다른 개발자가 읽기 쉽고,<br />
재사용 가능하며 리팩토링 가능하게끔 작성하도록 도와줍니다.

좋은 개발자가 되기 위해서는 코드가 정상적으로 작동하는 것과 더불어,<br />
**Clean Code**를 작성하는 것도 중요한 능력이 필요합니다.

저도 동료 개발자와 협업하기 때문에 이 주제로 항상 토론하고 많이 배우고 있죠.

그래서 오늘은 github에서 이와 관련된 좋은 글을 찾아서 공유해 보려고 합니다! ㅎㅎ<br />
(원문은 글 아래에 링크되어 있습니다.)

이글은 소프트웨어 방법론에 관한 책들 중 Robert C. Martin's의 책인<br />
**Clean Code**에 있는 내용을 JavaScript 언어에 적용시켜 적은 글입니다.

**(이것은 책을 쓴 개발자의 개인적인 의견입니다. 법칙이 아닙니다!)**

## 변수(Variables)

### 1. 의미있고 발음하기 쉬운 변수 이름을 사용하세요

**Bad:**

```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD')
```

**Good:**

```javascript
const currentDate = moment().format('YYYY/MM/DD')
```

### 2. 동일한 유형의 변수에 동일한 어휘를 사용하세요

**Bad:**

```javascript
getUserInfo()
getClientData()
getCustomerRecord()
```

**Good:**

```javascript
getUser()
```

### 3. 검색가능한 이름을 사용하세요

**Bad:**

```javascript
// 대체 86400000 무엇을 의미하는 걸까요?
setTimeout(blastOff, 86400000)
```

**Good:**

```javascript
// 대문자로 `const` 전역 변수를 선언하세요
const MILLISECONDS_IN_A_DAY = 86400000
setTimeout(blastOff, MILLISECONDS_IN_A_DAY)
```

### 4. 의도를 나타내는 변수들을 사용하세요

**Bad:**

```javascript
const address = 'One Infinite Loop, Cupertino 95014'
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
)
```

**Good:**

```javascript
const address = 'One Infinite Loop, Cupertino 95014'
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/
const [, city, zipCode] = address.match(cityZipCodeRegex) || []
saveCityZipCode(city, zipCode)
```

### 5. 자신만 알아볼 수 있는 작명을 피하세요

**Bad:**

```javascript
const locations = ['서울', '인천', '수원']
locations.forEach(l => {
  doStuff()
  doSomeOtherStuff()
  // ...
  // ...
  // ...
  // 잠깐, `l`은 또 뭘까요?
  dispatch(l)
})
```

**Good:**

```javascript
const locations = ['서울', '인천', '수원']
locations.forEach(location => {
  doStuff()
  doSomeOtherStuff()
  // ...
  // ...
  // ...
  dispatch(location)
})
```

### 6. 문맥상 필요없는 것들을 쓰지 마세요

**Bad:**

```javascript
const Car = {
  carMake: 'BMW',
  carModel: 'M3',
  carColor: '파란색'
}

function paintCar(car) {
  car.carColor = '빨간색'
}
```

**Good:**

```javascript
const Car = {
  make: 'BMW',
  model: 'M3',
  color: '파란색'
}

function paintCar(car) {
  car.color = '빨간색'
}
```

### 7. 기본 매개변수가 short circuiting 트릭이나 조건문 보다 깔끔합니다

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.'
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}
```

## 함수(Functions)

### 1. 함수 인자는 2개 이하가 이상적입니다

매개변수의 개수를 제한 하는 것은 함수 테스팅을 쉽게 만들어 주기 때문에 중요합니다.

만약 매개변수가 3개 이상일 경우엔 테스트 해야하는 경우의 수가 많아지고 각기 다른 인수들로 여러 사례들을 테스트 해야합니다.

이경우 es6의 비구조화(destructuring) 구문을 명확하게 할 수 있습니다.

**Bad:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Good:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
})
```

### 2. 함수는 하나의 행동만 해야합니다

**Bad:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client)
    if (clientRecord.isActive()) {
      email(client)
    }
  })
}
```

**Good:**

```javascript
function emailClients(clients) {
  clients.filter(isClientActive).forEach(email)
}

function isClientActive(client) {
  const clientRecord = database.lookup(client)
  return clientRecord.isActive()
}
```

### 3. 함수명은 함수가 무엇을 하는지 알 수 있어야 합니다

**Bad:**

```javascript
function AddToDate(date, month) {
  // ...
}

const date = new Date()

// 뭘 추가하는 건지 이름만 보고 알아내기 힘듭니다.
AddToDate(date, 1)
```

**Good:**

```javascript
function AddMonthToDate(date, month) {
  // ...
}

const date = new Date()
AddMonthToDate(date, 1)
```

### 4. 함수는 단일 행동을 추상화 해야합니다

추상화된 이름이 여러 의미를 내포하고 있다면 그 함수는 너무 많은 일을 하게끔 설계된 것입니다.

함수들을 나누어서 재사용가능하고 테스트하기 쉽게 만드세요.

**Bad:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ]

  const statements = code.split(' ')
  const tokens = []
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    })
  })

  const ast = []
  tokens.forEach(token => {
    // lex...
  })

  ast.forEach(node => {
    // parse...
  })
}
```

**Good:**

```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ]

  const statements = code.split(' ')
  const tokens = []
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */)
    })
  })

  return tokens
}

function lexer(tokens) {
  const ast = []
  tokens.forEach(token => {
    ast.push(/* ... */)
  })

  return ast
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code)
  const ast = lexer(tokens)
  ast.forEach(node => {
    // parse...
  })
}
```

### 5. 중복된 코드를 작성하지 마세요

중복된 코드를 작성하지 않기위해 최선을 다하세요.

중복된 코드가 있다는 것은 어떤 로직을 수정해야 할 일이 생겼을 때 수정 해야할 코드가 한 곳 이상이라는 것을 뜻합니다.

**Bad:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developers => {
    const expectedSalary = developer.calculateExpectedSalary()
    const experience = developer.getExperience()
    const githubLink = developer.getGithubLink()
    const data = {
      expectedSalary,
      experience,
      githubLink
    }

    render(data)
  })
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary()
    const experience = manager.getExperience()
    const portfolio = manager.getMBAProjects()
    const data = {
      expectedSalary,
      experience,
      portfolio
    }

    render(data)
  })
}
```

**Good:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary()
    const experience = employee.getExperience()

    let portfolio = employee.getGithubLink()

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects()
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    }

    render(data)
  })
}
```

### 6. Object.assign을 사용해 기본 객체를 만드세요

**Bad:**

```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
}

function createMenu(config) {
  config.title = config.title || 'Foo'
  config.body = config.body || 'Bar'
  config.buttonText = config.buttonText || 'Baz'
  config.cancellable =
    config.cancellable === undefined ? config.cancellable : true
}

createMenu(menuConfig)
```

**Good:**

```javascript
const menuConfig = {
  title: 'Order',
  // 유저가 'body' key의 value를 정하지 않았다.
  buttonText: 'Send',
  cancellable: true
}

function createMenu(config) {
  config = Object.assign(
    {
      title: 'Foo',
      body: 'Bar',
      buttonText: 'Baz',
      cancellable: true
    },
    config
  )

  // config는 이제 다음과 동일합니다: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig)
```

### 7. 사이드 이펙트를 피하세요 (1)

어떠한 구조체도 없이 객체 사이의 상태를 공유하거나, 무엇이든 쓸 수 있는 변경 가능한 데이터 유형을 사용하거나,

같은 사이드 이펙트를 만들어내는 것을 여러개 만들거나하면 안됩니다.

**Bad:**

```javascript
// 아래 함수에 의해 참조되는 전역 변수입니다.
// 이 전역 변수를 사용하는 또 하나의 함수가 있다고 생각해보세요. 이제 이 변수는 배열이 될 것이고, 프로그램을 망가뜨리겠죠.
let name = 'Ryan McDermott'

function splitIntoFirstAndLastName() {
  name = name.split(' ')
}

splitIntoFirstAndLastName()

console.log(name) // ['Ryan', 'McDermott'];
```

**Good:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
}

createMenu(menuConfig)
```

### 8. 사이드 이펙트를 피하세요 (2)

자바스크립트에서는 기본타입 자료형은 값을 전달하고 객체와 배열은 참조를 전달합니다.

따라서 가장 좋은 방법은 배열을 복제하여 수정하고 복제본을 반환하는 것입니다.

**Bad:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() })
}
```

**Good:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }]
}
```

### 9. 전역 함수를 사용하지 마세요

전역 환경을 사용하는 것은 JavaScript에서 나쁜 관행입니다.

왜냐하면 다른 라이브러리들과의 충돌이 일어날 수 있고, 당신의 API를 쓰는 유저들은 운영환경에서 예외가 발생하기 전까지는 문제를 인지하지 못할 것이기 때문입니다.

예제를 하나 생각해봅시다. JavaScript의 네이티브 Array 메소드를 확장하여 두 배열 간의 차이를 보여줄 수있는 diff 메소드를 사용하려면 어떻게 해야할까요?

새로운 함수를 Array.prototype에 쓸 수도 있지만, 똑같은 일을 시도한 다른 라이브러리와 충돌 할 수 있습니다.

다른 라이브러리가 diff 메소드를 사용하여 첫번째 요소와 마지막 요소의 차이점을 찾으면 어떻게 될까요?

이것이 그냥 ES2015/ES6의 classes를 사용해서 전역 Array를 상속해버리는 것이 훨씬 더 나은 이유입니다.

**Bad:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray)
  return this.filter(elem => !hash.has(elem))
}
```

**Good:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray)
    return this.filter(elem => !hash.has(elem))
  }
}
```

### 10. 명령형 프로그래밍보다 함수형 프로그래밍을 지향하세요

**Bad:**

```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
]

let totalOutput = 0

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode
}
```

**Good:**

```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
]

const totalOutput = programmerOutput
  .map(programmer => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE)
```

### 11. 조건문을 캡슐화 하세요

**Bad:**

```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Good:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode)
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

### 12. 부정조건문을 사용하지 마세요

**Bad:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

### 13. 과도한 최적화를 지양하세요

최신 브라우저들은 런타임에 많은 최적화 작업을 수행합니다.

대부분 당신이 코드를 최적화 하는 것은 시간낭비일 가능성이 많습니다.

**Bad:**

```javascript
// 오래된 브라우저의 경우 캐시되지 않은 `list.length`를 통한 반복문은 높은 코스트를 가졌습니다.
// 그 이유는 `list.length`를 매번 계산해야만 했기 때문인데, 최신 브라우저에서는 이것이 최적화 되었습니다.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

### 14. 죽은 코드를 지우세요

**Bad:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule
inventoryTracker('apples', req, 'www.inventory-awesome.io')
```

**Good:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule
inventoryTracker('apples', req, 'www.inventory-awesome.io')
```

## 원문

[ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
