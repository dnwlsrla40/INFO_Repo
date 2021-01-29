# JSX

JavaScript와 HTML이 합쳐진 구조로 유일하게 react가 제공하는 독단적인 기능

# PropTypes

client가 요청하는 Prop의 Type, Parameter등을 확인하는 라이브러리

```
import PropTypes from 'prop-types'; // Prop가 전송하는 데이터 check

Food.propTypes = {
  // 이름을 무조건 propTypes로 지어야함
  // guide로 제공하는 option 확인 가능
  // isRequired : 필수 요소
  name : PropTypes.string.isRequired,
  picture : PropTypes.string.isRequired,
  rating : PropTypes.number.isRequired 
}
```
라고 사용하면, name,picture,rating이 요소로 필요하고 각각의 Type을 확인하여 없거나 틀리면 브라우저 console창에 warning 발생

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