Date : 230510  
Learning Scope: 1장 21 ~ 27
<br><br><br><br>

# 21. 커스텀 Hooks 만들기
반복되는 로직을 재사용하고자 하는 경우가 있다.

<br>

### 1. 이를 해결하기 위한 고전 방법 2 가지
    1. Higher-order Components (HoC)
      : 파라미터로 컴포넌트를 받아와서 그리는 방식
    2. render props

<br>

### 2. Custom Hook
컴포넌트 트리에 새 컴포넌트를 추가하지 않고 `로직만을 분리`  
          
    컴포넌트 트리를 통해 전달되어 생성하는 것이 아닌,
    각 컴포넌트에서 독립적으로 state를 생성 할 수 있다. 

#### 언제 사용? `반복되는 로직을 재사용할 때`
#### 반환되는 값이나 기능의 상태는? `각 컴포넌트의 독립된 상태`

<br>

### 3. 사용법
* 📁 hooks 등등 적당한 위치의 폴더에 생성
* `use~` prefix를 붙인다.
* Hooks의 API를 사용하여 원하는 기능을 구현하고 컴포넌트에서 사용할 값들을 반환한다.  


1. Custom Hook 만들기

    > useInputs.js
    ```js
    import { useState, useCallback } from 'react';

    const userInputs(initialForm) {
      const [form, setForm] = useState(initialForm);

      const onChange = useCallback(e => {
        const { name, value } = e.target;
        setForm(form => ({ ...form, [name]: value }));
      }, []);

      const reset => useCallback(() => setForm(initialForm), [initialForm]);

      return [ form, onChange, reset ]; 
    }

    export default userInputs;

    ```
    > 👀 키워드
    >> 1. `use~` prefix  
    >> 2. 내장훅 사용
    >> 3. `[]` 혹은 `{}` 리턴한다.
  
<br>

2. Custom Hook 사용하기

    > App.js
    ```js
      ...
      
      import useInputs from './hooks/useInputs';

      ...

      function App() {
        const [{ username, email }, onChange, reset] = useInputs({
          username: '',
          email: ''
        });
      }

      export default App;
    ```
    > 👀 키워드
    >> 1. 최상위 수준에서 Hook을 호출한다.
    >> 2. Hook API를 생성하였다. 

<br>

## Hook 규칙
1. `최상위 수준`에서만 Hook을 호출한다.
    * 반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하면 안 된다. 
    * 리액트의 최상위에서 호출해야 컴포넌트가 랜더링 될 떄 동일 순서로 Hook이 호출되는 것을 보장.
2. 오직 React 함수 내에서 Hook을 호출한다.
    * 일반적인 JS 함수에서 호출하면 안 된다.


<br><br><br><br>

# 22. `Context API` 
여러 컴포넌트를 거쳐 깊은 곳에 위치한 컴포넌트에게 `state`, `function` 를 전달해줘야 하는 상황이 생긴다.  
이때 상태 및 함수 전달 시 중간다리 역할만을 하는 컴포넌트들이 생기는데, 이는 뎁스가 깊어지면 복잡하고 번거로워진다.  
이러한 문제는 `ContextAPI`와 `dispatch`를 사용하여 해결할 수 있다.  


## Context API
```
* 컴포넌트 안에서 `전역적`으로 데이터를 공유한다. 
* 일반적으로 부모와 자식간 props를 날려 state를 변화시키는 것과 달리,   
   context api는 컴포넌트 간 간격이 없다.  
```

### 1. Context API가 다루는 값  
`값`, `상태`, `함수`, `인스턴스`, `DOM` 모두 가능하다.  

### 2. 사용법      
#### `React.createContext` 
  * `createContext()`의 param에는 Context의 기본값이 설정된다.  
  * `export`를 해주어야 사용 가능.
```js
import React from 'react';
export const MyStore = React.createContext(defaultValue);

// `defaultValue` param은 트리 안에서 적절한 provider를 찾지 못 할 때 쓰이는 값.  
```

  <br><br>

#### `Context.Provider`
  * 정의한 context를 하위 컴포넌트에게 전달하는 역할.  
  * provider를 전달하는 변수는 꼭 value를 사용해야 한다.  
  * provider > provider 하위 배치 가능하며, 이 경우 하위 provider가 우선시  
  * provider하위에 context를 가진 component는 provider의 value로 가진 state가 변화할 때마다, 전부 re-render된다.

```js
  <MyStore.Provider value={this.state}>
    <subComponent1 />
    <subComponent2 />
  </MyStore.Provider>

  // value 값을 설정해줘야 한다.
  // this.state를 어디서든 조회하여 사용할 수 있다.
```

<br><br>

#### `Context.Consumer` 
* context 변화를 구독하는 컴포넌트
* context의 자식은 함수(컴포넌트)여야 한다.  
* 이 함수(컴포넌트)가 가지는 context값은 가장 가까운 provider의 값.  
* 상위 provider가 없다면, `createContext()`에서 정의한 `defaultValue`를 갖는다.

```js
  <MyContext.Consumer>
    {value => /* context 값을 이용한 렌더링 */}
  </MyContext.Consumer>
```

### 3. 여러 컴포넌트 구독하기
위 방법은 하나의 context만 구독 가능하다.

여러 컴포넌트를 구독하려면, Consumer 내부에 또 다른 Consumer를 정의하여 context를 내려준다.
```js
<ThemeContext.Consumer>
  {theme => (
    <UserContext.Consumer>
      {user => <ProfilePage user={user} theme={theme} />}
    </UserContext.Consumer>
  )}

</ThemeContext.Consumer>

```


<br><br><br><br>


# 23. Immer 를 사용한 더 쉬운 불변성 관리
리액트에서 배열이나 객체 업데이트 시
- 직접 수정 X
- 불변성을 지켜야 한다.


```js
// 객체

// BAD
const obj = {
  a: 1,
  b: 2
};
obj.b = 3; 

// GOOD
const obj = {
  a: 1,
  b: 2
};

const nextObj = { // 새로운 객체를 만든다.
  ...obj,
  b: 3
}
```

```js
// 배열

// BAD
// push, splice, 직접 n 번째 항목 수정 

// GOOD
// concat, filter, map 사용
```

`immer` 라이브러리 사용법에 따라 사용한다.  
그러나, 성능적으로는 `immer`를 사용하지 않은 코드가 조금 더 빠르다는 점.  
* immer는 js의 Proxy를 사용하는데 구형 브라우저나 react-native에서는 지원되지 않고, 비슷한 ES5의 fallback사용. 꽤나 느리기에 성능저하.  


(실제로 React State 규칙이 있는데, 이를 무시하고 사용할 수 있도록 해주는 라이브러리이기에 비추한다고 함.)


<br><br><br><br>


# 24. 클래스형 컴포넌트  
* 함수형 컴포넌트 + Hooks로는 할 수 없는 작업 2가지가 있음
* 그 외, 엣날에 만들어진 라이브러리의 경우 Hooks지원이 안되거나,
* react-native 관련 라우터 라이브러리인 `react-native-navigation` 의 경우에도 클래스형 컴포넌트를 어쩔 수 없이 써야 하는 일이 종종 존재함.

  ```js

  import React, { Component } from 'react';

  class Hello exnteds Component {
    render() {
      const { color, name, isSpecial } = this.props;

      return (
        <div style={{ color }}>
          {isSpecial && <b>*</b>}
          안녕하세요 {name}
        </div>
      );
    }
  }

  Hello.defaultProps = {
    name: 'NoName'
  };

  export default Hello;

  ```
  > 👀 
  >> 1. `render()` 메서드 필수
  >> 2. `this.` 로 props 조회  
  >> 3. defaultProps 사용시 컴포넌트 내부에서 사용하려면 클래스 내부에서 `static` 키워드와 함께 선언 가능.  
  
  <br>

  ```js
  // class Hello 내부
  static defaultProps = {
    name: 'NoName'
  }
  ```



### 메서드와 컴포넌트 인스턴스의 관계가 끊기는 경우
해결 방법 3가지
1. 클래스의 생성자 메서드에서 bind작업을 해줌.
    ```js
    class Counter extends Component {
      constructor(props) {
        super(props); // Counter class가 컴포넌트로서 작동할 수 있도록 Component쪽 생성자 함수를 먼저 실행 
        this.handleIncrease = this.handleIncrease.bind(this);
      }

      // 함수 정의
      handleIncrease() {
        // ..
      }

      render() {
        return (
          <div>
            <button onClick={this.handleIncrease}>+1</button>
          </div>
        );
      }
    }

    export default Counter;
    ```

2. 화살표 문법 사용  
`class-properties` 문법 (정식 js 문법 아님)
보통 CRA로 만든 프로젝트에서 커스텀 메서드를 만들 때 자주 사용.  
    ```js
    class Counter extends Component {
      handleIncrease = () => {
        //...
      };

      render() {
        return (
          <div>
            <button onClick={this.handleIncrease}>+1</button>
          </div>
        );
      }
    }
    export default Counter;
    ```

3. onClick에 새로운 함수 전달  
렌더링 할 때 마다 함수가 새로 만들어져서 최적화할 때 까다로우므로 비추.
    ```js
    return (
      <div>
        <button onClick={() => this.handleIncrease()}>+1</button>
      </div>
    );
    ```

<br>


### 상태 선언하기
클래스형 컴포넌트의 `state`는 무조건 객체형태
```js
constructor(props) {
  super(props);
  this.state = {
    counter: 0
  };
}

// CRA: `class-properties` 문법 적용되어 있다면,
constructor(props) {
  state = {
    counter: 0
  };
}
```

### 상태 업데이트하기
- this.setState 사용한다.  
- {} 객체 형태로 업데이트 한다.  
```js
class Counter extends Component {
  state = {
    counter: 0
  }

  handleIncrease = () => {
    this.setState({
      counter: this.state.counter + 1
    });
  };
...
}
```

### 상태 업데이트하기(2) - 함수형 업데이트
한 함수에서 `setState`를 여러번에 걸쳐서 해야 되는 경우 유용하다.  

```js
// 업데이트 시 객체
// 결과 -> 1씩 증가
handleIncrease = () => {
  this.setState({
    counter: this.state.counter + 1
  });
  this.setState({
    counter: this.state.counter + 1
  });
};
```
```js
// 업데이트 시 함수형 업데이트
// 결과 -> 2씩 증가
handleIncrease = () => {
  this.setState(state => ({
    counter: state.counter + 1
  }));
  this.setState(state => ({
    counter: state.counter + 1
  }));
};
```
> 객체 업데이트에서 2가 아닌 1씩 증가하는 이유는, 상태가 바로 바뀌는 것이 아니기 때문이다.  
상태를 바꾸는 함수가 아닌, 특정 상태로 바꿔달라고 요청하는 함수이기 때문.


```js
// 상태가 
// setState의 두 번째 파라미터에 콜백함수
// 
handleIncrease = () => {
  this.setState(
    {
      counter: this.state.counter + 1
    },
    () => {
      console.log(this.state.counter);
    }
  );
};
```

<br><br><br><br>


# 25. LifeCycle Method
## 생명주기 메서드
1. 컴포넌트가 브라우저에 나타나고
2. 업데이트 되고
3. 사라지게 될 때 
호출 되는 메서드

useEffect와 비슷하나 커버하지 않는 기능들이 있음.  

## 1. Mount
- `constructor` : 컴포넌트 생성자 매서드. 컴포넌트가 만들어지면 가장 먼저 실행됨.
- `getDerivedStateFromProps` : props 로 받아온 것을 state 에 넣어주고 싶을 때 사용
- `render` : 렌더링 함수
- `componentDidMount` : 첫번째 렌더링을 마치고 나면 호출되는 메서드. 외부 라이브러리 연동이나 axios, fetch 요청, DOM 수정

## 2. Update
- `getDerivedStateFromProps` : 컴포넌트의 props 나 state 가 바뀌었을때도 이 메서드가 호출
- `shouldComponentUpdate` : 컴포넌트가 리렌더링 할지 말지를 결정하는 메서드
- `render`
- `getSnapshotBeforeUpdate` : 컴포넌트에 변화가 일어나기 직전의 DOM 상태를 가져와서 특정 값을 반환하면 그 다음 발생하게 되는 `componentDidUpdate` 함수에서 받아와서 사용을 할 수 있다.
- `componentDidUpdate` : 렌더링이 마치고, 화면에 변화가 모두 반영되고 난 뒤 호출되는 메서드 3번째 파라미터로 getSnapshotBeforeUpdate 에서 반환한 값을 조회 할 수 있음.


## 3. UnMount
- `componentWillUnmount` : 컴포넌트가 화면에서 사라지기 직전에 호출된다. web api를 제거하거나 외부 라이브러리 dispose기능이 있다면 여기서 호출

<br><br><br><br>


# 26. componentDidCatch 로 에러 잡아내기 / Sentry 연동
리액트 애플리케이션에서 발생하는 에러를 처리하는 방법

<br><br><br><br>


# 27. 리액트 개발 할 때 사용하면 편리한 도구들 - Prettier, ESLint, Snippet
검색해서.. 잘 적용하도록..



