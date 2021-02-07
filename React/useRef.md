# useRef

react에서 컴포넌트의 특정 DOM을 선택해야 할 때, ref를 사용해야 한다.

함수형 컴포넌트에서는 이를 위해 `useRef`라는 Hook 함수를 사용한다.

```
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: ''
  });
  const nameInput = useRef();

  const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

  const onChange = e => {
    const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
    setInputs({
      ...inputs, // 기존의 input 객체를 복사한 뒤
      [name]: value // name 키를 가진 값을 value 로 설정
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: ''
    });
    nameInput.current.focus();
  };

  return (
    <div>
      <input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}
      />
      <input
        name="nickname"
        placeholder="닉네임"
        onChange={onChange}
        value={nickname}
      />
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

위의 코드는 초기화 버튼을 클릭했을 때 이름 input에 포커스가 잡히도록 `useRef`를 사용한 것이다.

추가로 `useRef`는 컴포넌트 안에서 조회 및 수정 할 수 있는 변수를 관리할 때에도 사용할 수 있다.

중요한 점은 `useRef`로 관리하는 변수는 값이 바뀐다고 해서 컴포넌트가 리렌더링 되지 않는다는 것이다. 

즉, 리액트 컴포넌트에서 상태(State)는 상태를 바꾸는 함수를 호출하고 나서 그 다음 렌더링 이후로 업데이트 된 상태를 조회 할 수 있는 반면, `useRef`로 관리하고 있는 변수는 설정 후 바로 조회 할 수 있다는 것이다.

따라서 이 변수를 사용하여 다음과 같은 값을 관리 할 수 있다.

 - `setTimeout`, `setInterval`을 통해서 만들어진 `id`
 - 외부 라이브러리를 사용하여 생성된 인스턴스
 - scroll 위치