## JSX

자바스크립트의 확장 문법으로 xml과 매우 비슷하게 생겼다.

```
var a = {
  <div>
    <h1>Awesome <b>React</b></h1>
  </div>
}
```

JSX 형식으로 작성된 코드는 나중에 코드가 번들링되면서 *babel-loader* 를 사용하여 자바스크립트로 변환한다.

```
var a = React.createElement(
  "div",
  null,
  React.createElement(
    "h1",
    null,
    "Awesome",
    React.createElement(
      "b",
      null,
      "React"
      )
    )
  );
```

> JSX도 ES6 문법인가?

JSX는 리액트용이기 때문에 공식 자바스크립트 문법은 아니다. babel이 이를 변환해 주기는 하지만, 바벨에서 여러 문법을 지원할 수 있도록 **preset** 이란 것을 설정한다. (ES6에는 babel-preset-es2015를 적용)

---

#### JSX의 장점

- 보기 쉽고 익숙하다 (위의 두 예제 코드 참고)
- 오류 검사: JSX에 오류가 있다면, babel이 코드를 변환하는 과정에서 이를 감지해 낸다
- 더 높은 활용도: div나 span 같은 HTML 태그 뿐만 아니라, 앞으로 만들 컴포넌트도 작성 가능


```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

**Q. ReactDom.render(...)의 역할?**

컴포넌트를 페이지에 렌더링 하는 역할을 하며,
react-dom 모듈을 불러와 사용할 수 있다.

첫번째 파라미터는 페이지에 렌더링할 내용을 JSX 형태로 작성하고,
두번째 파라미터는 해당 JSX를 렌더링할 document 내부 요소를 설정한다.

---

#### JSX 문법

> 감싸인 요소

컴포넌트에 여러 요소가 있다면 부모 요소 하나로 꼭 감싸야 한다.

Virtual DOM에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 **컴포넌트 내부는 DOM 트리 구조 하나여야 한다** 는 규칙을 따르기 때문이다.

```
render() {
  return (
      <div>
        <h1>리액트 안녕!</h1>
        <h2>당신은 어썸한가요?</h2>
      </div>
    )
}
```

div 태그로 감싸지 않으면 아래와 같은 오류가 난다

```
Failed o compile.

Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag.
```

혹은 Fragment 컴포넌트로 감싸고 여러 요소를 렌더링하는 것도 가능하다.

```
import React, { Component, Fragment } from 'react';

class App extends Component {
  render() {
    return (
      <Fragment>
        <h1>리액트 안녕!</h1>
        <h2>당신은 어썸한가요?</h2>
      </Fragment>
      )
  }
}
```

> 자바스크립트 표현

JSX 안에서는 자바스크립트 표현식을 쓸 수 있다.

```
import React, { Component } from 'react';

class App extends Component {
  render() {
    const text = '당신은 어썸한가요?';
    const condition = true;

    return (
      <Fragment>
        <h1>리액트 안녕!</h1>
        <h2>{text}</h2>
        {
          condition ? '참' : '거짓'
        }
      </Fragment>
    );
  }
}

export default App;
```

**JSX 내부의 자바스크립트 표현식에선 if문을 사용할 수 없다**

조건에 따라 다른 것을 렌더링해야 할 때에는,
- JSX 밖에서 if문을 사용하여 작업하거나
- {} 안에 조건부 (삼항) 연산자를 사용한다
