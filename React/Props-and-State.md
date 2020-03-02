> Props

- 부모 컴포넌트가 자식 컴포넌트에게 주는 값
- 자식 컴포넌트는 props를 받아오기만하고, 받아온 props를 직접 수정할 수 없다

```
import React, { Component } from 'react';
import MyName from './MyName';

class App extends Component {
  render() {
    return (
      <MyName name="Kate" />
      )
  }
}
```


```
import React, { Component } from 'react';

class MyName extends Component {
  render () {
    return (
      <div>
        Hello {this.props.name}!
      </div>
      )
  }
}

export default MyName;
```

output:
`Hello React!`

실수로 props를 빠뜨리거나, 일부러 props를 비우는 상황을 위해서는 **defaultProps** 를 사용한다.

```
... <MyName />
```

```
import React, { Component } from 'React';

class MyName extends Component {
  // #1
  static defaultProps = {
    name : 'Tony'
  }

  render() {
    return (
      <div>
        Hello {this.props.name}!
      </div>
      )
  }
}

// #2
MyName.defaultProps = {
  name : 'Tony'
};

export default MyName;
```


> State

- JavaScript Object로서, User event를 저장하고 반응하는데 이용
- 컴포넌트 기반의 각 클래스는 그 자체의 state object를 가진다
** 함수형 컴포넌트는 state를 가지지 않고, 클래스 기반 컴포넌트만 스테이트를 가진다!
- 컴포넌트 내부에서 선언하며 내부에서 setState를 통해 값을 변경할 수 있다
- State를 사용하기 전에 state object initialization를 해야한다

```
import React, { Component } from 'react';

class Counter extends Component {

  constructor(props) {
    super(props); // 부모 클래스인 Component에 정의된 constructor 메소드를 호출

    this.state = {
      number: 0
    }
  }

  handleIncrease = () => {
    console.log(this);
      this.setState({
        number: this.state.number + 1
      })
  }

  handleDecrease = () => {
      this.setState({
        number: this.state.number - 1
      })
  }

  render() {
    return (
        <div>
          <h1>카운터</h1>
          <div>값 : {this.state.number}</div>
          <button onClick={this.handleIncrease}>+</button>
          <button onClick={this.handleDecrease}>-</button>
        </div>
    )
  }
}

export default Counter;
```
