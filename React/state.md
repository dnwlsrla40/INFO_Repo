# State

컴포넌트에서 보여줘야 하는 내용이 사용자 인터랙션에 따라 바뀌어야 할 때 사용

리액트 16.8 이전 버전 까진 함수형 컴포넌트에서는 상태를 관리할 수 없었는데 16.8부터 Hooks라는 기능이 도입되면서 가능하게 되었다.

useState라는 함수가 리액트의 Hooks 중 하나이다.

 - 상태(State) : 컴포넌트의 동적인 값

## useState

```
적용 전

import React from 'react';

function Counter(){
    const onIncrease = () => {
        console.log('+1')
    }

    const onDecrease = () => {
        console.log('-1')
    }

    return(
        <div>
            <h1>0</h1>
            <button onClick={onIncrease}>+1</button>
            <button onClick={onDecrease}>-1</button>
        </div>
    );
}

export default Counter;
```

```
적용 후
import React, { useState } from 'react'; // 리액트 패키지에서 useState 함수를 불러옴

function Counter(){

    const [number, setNumber] = useState(0); // 사용 할 때 상태의 기본 값을 파라미터로 넣어서 호출(여기선 0으로 호출) return 값은 배열로 첫 번째 원소는 현재 상태, 두 번째 원소는 Setter 함수다.

    const onIncrease = () => {
        setNumber(number + 1);
    }

    const onDecrease = () => {
        setNumber(number - 1);
    }

    return(
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+1</button>
            <button onClick={onDecrease}>-1</button>
        </div>
    );
}

export default Counter;
```

## 함수형 업데이트

위의 코드는 Setter 함수를 사용할 때, 업데이트 하고 싶은 새로운 값을 파라미터로 넣어주고 있는데, 그 대신에 기존 값을 어떻게 업데이트 할 지에 대한 함수를 등록하는 방식으로도 값을 업데이트 할 수 있다.

```
import React, { useState } from 'react'; // 리액트 패키지에서 useState 함수를 불러옴

function Counter(){

    const [number, setNumber] = useState(0); // 사용 할 때 상태의 기본 값을 파라미터로 넣어서 호출(여기선 0으로 호출) return 값은 배열로 첫 번째 원소는 현재 상태, 두 번째 원소는 Setter 함수다.

    const onIncrease = () => {
        setNumber(prevNumber => prevNumber +1);
    }

    const onDecrease = () => {
        setNumber(prevNumber => prevNumber -1);
    }

    return(
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+1</button>
            <button onClick={onDecrease}>-1</button>
        </div>
    );
}

export default Counter;
```
onIncrease 와 onDecrease에서 setNumber를 사용 할 때 그 다음 상태를 파라미터로 넣어준 것이 아니라, 값을 업데이트 하는 함수를 파라미터로 넣어주었다.

함수형 업데이트는 주로 나중에 컴포넌트를 최적화 하게 될 때 사용하게 된다.

## 여러 개의 input 상태 관리

input의 개수가 여러 개가 됐을 때는, 단순히 useState를 여러번 사용하고 onChange도 여러개 만들어서 구현 할 수 있지만 좋지 않은 방법이다.

useState 에서 문자열이 아닌 객체 형태의 상태 관리를 해주고 input에 name을 설정해 어떤 이벤트가 발생 했는지 구분해주는 것이 좋다.

```
import React, { useState } from 'react';

function InputSample() {
    const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
    });

    const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출


    const onChange = (e) => {
        const { value, name } = e.target; // 우선 e.target 에서 name 과 value 추출
        setInputs({
            ...inputs, // 기존의 input 객체를 복사한 뒤
            [name]: value //  name 키를 가진 값을 value로 설정
        });
    };

    const onReset = () => {
        setInputs({
            name: '',
            nickname: '',
        })
    };

    return (
        <div>
            <input name="name" placeholder="이름" onChange={onChange} value={name}/>
            <input name="nickname" placeholder="닉네임" onChange={onChange} value={nickname}/>
            <button onClick={onReset}>초기화</button>
            <div>
                <b>값: </b>
                {name} ({nickname})
            </div>
        </div>
    );
}

export default InputSample;
```

React 상태에서 객체를 수정해야 할 때는,

```
inputs[name] = value;
```
이런 식으로 직접 수정하면 안되고, 새로운 객체를 만들어서 새로운 객체에 변화를 주고, 이를 상태로 사용해주어야 한다.

```
setInputs({
    ...inputs,
    [name]: value
});
```

위 코드에서 사용한 ... 문법은 spread 문법이다.

위와 같은 작업을, "불변성을 지킨다"라고 부른다. 불변성을 지켜주어야만 리액트 컴포넌트에서 상태가 없데이트가 됐음을 감지 할 수 있고 리렌더링이 진행된다.