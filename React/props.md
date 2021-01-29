# Props

props는 properties의 줄임말로 어떠한 값을 컴포넌트에게 전달해줘야 할 때 사용한다.

 - 전달 형태 : 객체
-  조회 : 파라미터를 통한 조회

## 기본 사용법

APP 컴포넌트에서 Hello 컴포넌트를 사용 할 때 name이라는 값을 전달해주고 싶다면 이렇게 코드를 작성한다.

```
App.js

import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" />
  );
}

export default App;
```

```
Hello.js

import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>
}

export default Hello;
```

이렇게 컴포넌트에게 전달되는 props는 파라미터를 통하여 조회 할 수 있다. props는 객체 형태로 전달된다.

## 여러개의 props, 비구조화 할당

```
App.js

import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" />
  );
}

export default App;
```

```
Hello.js

import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>
}

export default Hello;
```

이처럼 여러 개의 props를 보낼 수 있다.

위 코드에서 보면 props 내부의 값을 조회 할 때마다 `props.`을 입력하고 있는데 함수의 파라미터에서 "비구조화 할당(혹은 '구조 분해' 라고도 함)" 문법을 사용하면 조금 더 코드를 간결하게 작성 할 수 있다.

```
Hello.js

import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

export default Hello;
```

## defaultProps

컴포넌트에 props를 지정하지 않았을 때 기본적으로 사용할 값을 설정하고 싶다면 컴포넌트에 `defaultProps` 라는 값을 설정하면 된다.

```
Hello.js

import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```

```
App.js

import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <>
      <Hello name="react" color="red"/>
      <Hello color="pink"/>
    </>
  );
}

export default App;
```

![defaultProps](/picture/defaultProps.PNG)

## props.children

컴포넌트 태그 사이에 넣은 값을 조회하고 싶을 땐, props.children을 조회하면 된다.

```
Wrapper.js

import React from 'react';

function Wrapper() {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>

    </div>
  )
}

export default Wrapper;
```

```
App.js

import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red"/>
      <Hello color="pink"/>
    </Wrapper>
  );
}

export default App;
```

이렇게 App.js를 통해 Wrapper 태그 내부에 Hello 컴포넌트 두 개를 넣었는데 브라우저를 확인하면 아래와 같이 Hello 컴포넌트들은 보여지지 않는다.

![props.children 적용 전](/picture/Wrapper.PNG)

내부의 Hello 컴포넌트 내용을 보이게 하려면 Wrapper 컴포넌트에서 props.children을 렌더링 해주어야 한다.

```
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>
      {children}
    </div>
  )
}

export default Wrapper;
```

![props.children 적용 후](/picture/props.children.PNG)

## PropTypes  

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