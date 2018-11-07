---
layout: post
comments: true
title: '[REACT] React Hooks 업데이트!'
categories: [react]
tags: [react]
date: 2018-11-07
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}

## Hooks

**Hooks**는 조만간 react에서 도입 예정인 기술로 **React v16.7.0-alpha** 버전에서 사용가능합니다.(현재 16.6)<br>
**함수형 컴포넌트**에서도 상태 관리 및 클래스형 컴포넌트에서만 할 수 있는 다른 작업들을 구현 할 수 있게 해주는 기능입니다.<br>
또한 customize를 통해서 **모듈화**가 가능하고 context, ref 등 api 연동이 쉬워져 앞으로 hooks의 많은 사용이 예상되고 있죠!!<br>

## Try to Use

예제 프로젝트를 하나 만들어서 다음 기능들을 사용해보죠!

1. useState
2. useEffect
3. useContext

> create-react-app react-hooks <br>
> cd react-hooks <br>
> yarn add react@next react-dom@next

## useState

```javascript
import { useState } from 'react';

function Example() {
  //count라는 스테이트와 setCount라는 setter함수
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**useState** 를 호출하면 배열을 반환하는데, 이 배열의 첫번째 원소는 **현재 상태 값**과, 두번째 원소는 이 값을 설정해주는 **setter함수**입니다.<br>
또 인자로 들어가는 값은 i**nitial state**값이 됩니다. 지금은 0!

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

class를 사용하면 이런 코드가 됩니다. 비교해보세요!

## useEffect

```javascript
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  //componentDidMount와 componentDidUpdate 역할을 합니다.
  useEffect(() => {
    console.log(`You clicked ${count} times`);
  }, [count]);
  //[count]는 componentWillUnmount역할을 합니다.

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**useEffect**는 componentDidMount, componentDidUpdate, 그리고 componentWillUnmount를 합쳐놓은 기능이라고 볼 수 있습니다!<br>
**useEffect**의 첫번쨰 인자로 들어가는 **side effects**는 컴포넌트가 마운트 되거나 리렌더링이 마치고 나서 실행됩니다.<br>
그리고 return 값으로 특성 함수가 들어가면 그함수를 중지하는 역할을 해줍니다!<br>
또한 두번째 인자에 배열값을 넣으면 그 값을 주시하며 **변화가 있을때만** 실행합니다.

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    console.log(`You clicked ${this.state.count} times`);
  }

  componentDidUpdate() {
    console.log(`You clicked ${this.state.count} times`);
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

class를 사용하면 이런 코드가 됩니다. 비교해보세요!

## Custom Hook 

**useState**와 **useEffect**을 활용하여 다양한 작업을 할 수 있는데요! 재사용되는 코드를 **모듈화**해서 사용하는것도 한 방법이겠죠!<br>
axios를 활용한 예제를 만들어 보겠습니다!

```javascript
//src/hooks/useRequest.js
import { useEffect, useState } from 'react';
import axios from 'axios';

function useRequest(url) {
  // loading, response, error 값을 다루는 hooks
  const [loading, setLoading] = useState(false);
  const [response, setResponse] = useState(null);

  const func = () => {
    console.log("함수를 내보내다.")
  }

  // 렌더링 될 때, 그리고 url 이 바뀔때만 실행됨
  useEffect(
    async () => {
      try {
        setLoading(true);
        const res = await axios.get(url);
        setResponse(res);
      } catch (e) {
        console.log(e)
      }
      setLoading(false);
    },
    [url]
  );

  return [response, loading, func]; // 현재 값들을 배열로 반환
}

export default useRequest;
```

입력된 url을 httpRequest요청으로 처리해주는 모듈입니다!

```javascript
//src/App.js
import React, { useState } from 'react';
import useRequest from './hooks/useRequest';

const App = () => {
  const [searchValue, setSearchValue] = useState('');
  const [id, setId] = useState('huurray');
  const [response, loading, func] = useRequest(`https://api.github.com/users/${id}`);

  if (loading) {
    return <div>로딩중..</div>;
  }

  //가져온 함수 사용
  func();

  // 처음 렌더링에서는 response가 없으므로 null 리턴
  if (!response) return null;

  const { data } = response;

  return (
    <div>
      <input type="text" onChange={e => setSearchValue(e.target.value)} />
      <button onClick={() => setId(searchValue)}>Search</button>
      {data && (
        <div>
          <h1>{data.name}</h1>
          <img src={data.avatar_url} alt="avartar_url" />
        </div>
      )}
    </div>
  );
};

export default App;
```

**useRequest**를 모듈러 불러와 사용하면 **url이 바뀌였을때 리렌더링**되었을때 배열에 넣어진 값들을 다시 배출합니다!<br>
HOC 나 Render Props 로 할 수 있는 것들을, Hook 으로도 할 수 있게 되었습니다. <br><br>
**React Hooks**가 정식으로 릴리즈 되면 엄청난 격변이 일어날것같은데요!! 기대가 됩니다!!

## useContext

**useContext**는 context의 사용을 아주 깔끔하게 해줍니다. 바로 **consumer역할**을 하면서 말이죠!<br>
저번에 작성한 'contextAPI 알아보기'에서 예시로 만들었던 카운터를 만들어 보겠습니다.

```javascript
//src/context/Example.js
import React, { useState, createContext } from 'react';
import Counter from '../components/Counter';

export const Context = createContext();

function Example() {
  const [number, setNumber] = useState(0);

  const actions = {
    plusOne: () => {
      setNumber(number + 1)
    }
  }

  const value = { number, actions };
  console.log(value)
  return (
    <div>
      <Context.Provider value={value}>
        <Counter />
      </Context.Provider>
    </div>
  );
}

export default Example;
```

context를 만드는 것은 **state를 명시하는 방식**과 **State값을 바꾸는 방식(this.setState)**말고는 변한게 없습니다. <br>
child 컴포넌트로 넣어준 Counter를 만들어보겠습니다.

```javascript
//src/components/Counter.js
import React, { useContext } from 'react';
import { Context } from '../contexts/Example';

function Counter() {
  const ExampleCtx = useContext(Context);
    
  return (
    <div>
      <div>Counter is {ExampleCtx.number}</div>
      <button onClick={ExampleCtx.actions.plusOne}>+1</button>

    </div>
  );
}

export default Counter;
```

context를 사용할때는 **useContext**를 써서 불러오는 방식으로 사용합니다.<br>
위에서 **value값**으로 내보냈던 것들을 ExampleCtx객체에 담아 사용했습니다.


## 참고

[react hooks 공식문서](https://reactjs.org/docs/hooks-intro.html)<br>

[velopert님 react hooks알아보기](https://velog.io/@velopert/react-hooks)