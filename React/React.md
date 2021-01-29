# React

## React가 나오게 된 이유

1. 기존의 JS로 이벤트 핸들링을 할 경우 DOM의 특정 속성을 업데이트 해주어야 해 업데이트 규칙이 점차 복잡해짐
2. Ember, Backbone, AngularJs등의 프레임워크로 JS의 특정 값이 바뀌면 특정 DOM의 속서이 바뀌도록 연결하여 업데이트 작업을 간소화
3. React는 이와 다르게 상태가 변하면 상태에 따라 DOM을 업데이트하는 것이 아니라 아예 다 날려버리고 처음부터 모든 걸 새로 만듬
->  Virtual DOM(JS 객체이기 때문에 작동 성능이 훨씬 빠름)을 이용, 상태가 업데이트 되면 Virtual DOM에 렌더링 후 비교 알고리즘을 통해 업데이트를 어떻게 할 지 정함  

# State

동적인 데이터 작업을 할 때 사용

State는 변하는 데이터를 가지는 Object이다.

React Component에서 기본으로 제공하는 기능이므로 function component 말고 Class component에서 상속받아 사용 가능하다.

```
class App extends React.Component{
  state = {
    count : 0
  };

  render(){
    return <h1>The number is : {this.state.count}</h1>;  
  }
}
```

function 과는 다르게 Object이므로 this.state.데이터 형식으로 받아올 수 있다.

## State 값 변경

React는 JavaScript를 사용하므로 아래와 같이 작성할 수 있다.

```
class App extends React.Component{
  state = {
    count : 0
  };
  add = () => {
    console.log("add");
    this.state.count = 1;
  };
  minus = () => {
    console.log("minus");
    this.state.count = -1;
  }
  render(){
    return <div>
    <h1>The number is : {this.state.count}</h1>
    <button onClick = {this.add}>Add</button>
    <button onClick = {this.minus}>Minus</button>
    </div>;  
  }
}
```
하지만 이처럼 add나 minus를 통해 state를 직접 변경하려고 하면 error가 발생한다. 그 이유는 매번 state의 상태를 변경할 때 react는 render function을 호출해줘야 하기 때문이다.

```
add = () => {
  console.log("add");
  this.setState({count:this.state.count+1});
};
minus = () => {
  console.log("minus");
  this.setState({count:this.state.count-1});
}
```

따라서 우리는 setState()를 통해 state가 변경된 것을 알려주고 이를 통해 refresh를 해 render를 다시 호출할 수 있도록 해야한다.

위의 코드는 외부의 state에 의존하기 때문에 별로 좋은 코드는 아니다. 따라서, react에서는 현재 state의 상태를 제공하는 `current`라는 것을 제공한다. 이를 이용하면 아래와 같이 코드를 변경할 수 있다.

```
add = () => {
  console.log("add");
  this.setState(current => ({count: current.count+1}));
};
minus = () => {
  console.log("minus");
  this.setState(current => ({count: current.count-1}));
}
```

# React Life Cycle

Class Component에서 Component가 render 된 후 호출되는 다른 function들이 존재한다.