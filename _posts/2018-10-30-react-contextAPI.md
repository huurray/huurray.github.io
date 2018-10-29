---
layout: post
comments: true
title: '[REACT] React Context API 알아보기'
categories: [react]
tags: [react]
date: 2018-10-30
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}

## Context API

**react 16.3버전**이 릴리즈되면서 **Context API**가 대폭 새로워졌습니다.
전역적인 데이터를 관리하기 위한 Context가 이전에는 사용하기 불편했다고 합니다.
그래서 Redux, react-router, styled-components도 이 **Context API**를 기반으로 구현되어 사용된거죠.

그런데! Context API가 일반적으로 사용하기 **쉽게 바뀌었다는 것!!**
지금부터 한번 알아봅시다.

## Context 사용하기

**Context**는 createContext라는 함수를 사용해서 만들며, 이 함수를 호출하면 **Provider**와 **Consumer**라는 컴포넌트들이 반환됩니다.<br>
Provider는 Context 에서 사용 할 값을 설정할 때 사용되고, Consumer 는 나중에 우리가 설정한 값을 불러와야 할 때 사용됩니다.

**예제를 만들어봅시다!**
숫자를 1씩 더하는 예제를 만들어보겠습니다. 숫자는 전역 context안의 **state**에 저장하고, 
<br>1씩 더하는 메서드 또한 context안의 **actions** 객체에 메서드를 만들어 사용하겠습니다.

```javascript
// src/context/counter
import React, { Component, createContext } from 'react';

const Context = createContext();
const { Provider, Consumer: CounterConsumer } = Context;

// Provider 에서 state 를 사용하기 위해서 컴포넌트를 새로 만들어줍니다.
class CounterProvider extends Component {
  constructor(props) {
    super(props);

    this.state = {
      value: 0
    };
    // 여기서 actions 라는 객체는 우리가 임의로 설정하는 객체입니다.
    // 나중에 변화를 일으키는 함수들을 전달해줄때, 함수 하나하나 일일히 전달하는 것이 아니라,
    // 객체 하나로 한꺼번에 전달하기 위함입니다.
    this.actions = {
      plusOne: () => {
        this.setState(prevState => ({ value: prevState.value + 1 }));
      }
    };
  }

  render() {
    const { state, actions } = this;
    // Provider 내에서 사용할 값은, "value" 라고 부릅니다.
    // 현재 컴포넌트의 state 와 actions 객체를 넣은 객체를 만들어서,
    // Provider 의 value 값으로 사용하겠습니다.
    const value = { state, actions };

    return <Provider value={value}>{this.props.children}</Provider>;
  }
}

// 내보내줍니다.
export { CounterProvider, CounterConsumer };
```
```javascript
//App.js
import React from 'react';
import { CounterProvider } from './contexts/counter';
import Calculator from './components/Calculator';

const App = () => {
  return (
    <CounterProvider>
      <Calculator />
    </CounterProvider>
  );
};

export default App;
```

먼저 Context Provider를 만들어주고 상위 App.js를 **Provider**로 감싸줍니다.


```javascript
// src/components/Calculator
import React, { Component } from 'react';
import { CounterConsumer } from '../contexts/counter';

class Calulator extends Component {
  render() {
    return (
      <div>
        값: {this.props.value}
        <button onClick={this.props.plusOne}>더하기</button>
      </div>
    );
  }
}

const CalculatorContainer = () => (
  <CounterConsumer>
    {
      ({state, actions}) => (
        <Calulator 
          value={state.value}
          plusOne={actions.plusOne}
        />
      )
    }
  </CounterConsumer>
)

export default CalculatorContainer;
```

Context Provider 내부에 있는 메소드를 사용하기위해서 컨테이너 컴포넌트를 추가적으로 만들어서 props로 필요한 값을 전달하는 방식으로 구현해주었습니다.

![image](/assets/img/post/react-contextAPI/0.png)

버튼을 누루면 1씩 증가하는 모습을 볼 수 있습니다.

## HOC를 접목한 활용법

방금 제가 Container를 만들 때 사용했던 것을 쉽게 재사용 할 수 있도록 **HOC** 를 사용한다면 짱짱 편리해질 수 있습니다.<br>
Provider를 만들때 **HOC**도 함께 만들어 내보내 주겠습니다.

```javascript
// src/context/counter
import React, { Component, createContext } from 'react';

const Context = createContext();
const { Provider, Consumer: CounterConsumer } = Context;

// Provider 에서 state 를 사용하기 위해서 컴포넌트를 새로 만들어줍니다.
class CounterProvider extends Component {
  constructor(props) {
    super(props);

    this.state = {
      value: 0
    };
    // 여기서 actions 라는 객체는 우리가 임의로 설정하는 객체입니다.
    // 나중에 변화를 일으키는 함수들을 전달해줄때, 함수 하나하나 일일히 전달하는 것이 아니라,
    // 객체 하나로 한꺼번에 전달하기 위함입니다.
    this.actions = {
      plusOne: () => {
        this.setState(prevState => ({ value: prevState.value + 1 }));
      }
    };
  }

  render() {
    const { state, actions } = this;
    // Provider 내에서 사용할 값은, "value" 라고 부릅니다.
    // 현재 컴포넌트의 state 와 actions 객체를 넣은 객체를 만들어서,
    // Provider 의 value 값으로 사용하겠습니다.
    const value = { state, actions };

    return <Provider value={value}>{this.props.children}</Provider>;
  }
}

// :: HoC 를 사용
function withCounter(WrappedComponent) {
  return function UseCounterProvider(props) {
    return (
      <CounterConsumer>
        {
          ({ state, actions }) => (
            <WrappedComponent
              value={state.value}
              plusOne={actions.plusOne}
            />
          )
        }
      </CounterConsumer>
    )
  }
}
// 내보내줍니다.
export { CounterProvider, CounterConsumer, withCounter };
```

그리고 **HOC**만 사용해주고 props로 받아오면! 더욱 깔끔한 코드가 되었습니다.

```javascript
// src/components/Calculator
import React, { Component } from 'react';
import { withCounter } from '../contexts/counter';

class Calulator extends Component {
  render() {
    return (
      <div>
        값: {this.props.value}
        <button onClick={this.props.plusOne}>더하기</button>
      </div>
    );
  }
}

export default withCounter(Calulator);
```

## 참고

[react contextAPI 공식문서](https://reactjs.org/docs/context.html)<br>
[velopert 개발자님 블로그 contextAPI 파헤치기](https://velopert.com/3606)