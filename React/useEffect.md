# useEffect

React에서는 useEffect Hook을 이용하여 컴포넌트가 mount 됐을 때(처음 나타났을 때), unmount(사라질 때), 업데이트 될 때(특정 props가 바뀔 때) 특정 작업을 처리하도록 할 수 있다.

## 마운트 / 언마운트

마운트 시 하는 작업들
 - props 로 받은 값을 컴포넌트의 로컬 상태로 설정
 - 외부 API 요청(REST API 등)
 - 라이브러리 사용(D3, Video.js 등...)
 - setInterval을 통한 반복잡업 혹은 setTimeout을 통한 작업 예약

언마운트 시 하는 작업들
 - setInterval, setTimeout을 사용하여 등록한 작업 clear(clearInterval, clearTimeout)
 - 라이브러리 인스턴스 제거

useEffect

 - 첫 번째 파라미터 : 함수
 - 두 번째 파라미터 : 의존값이 들어있는 배열(=deps), 빈 경우 처음 나타날때에만 함수 호출
 - return : cleanup 함수, useEffect에 대한 뒷정리(deps가 빈 경우 컴포넌트가 사라질 때 cleanup 함수 호출)
```
UserList.js

import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');
    return () => {
      console.log('컴포넌트가 화면에서 사라짐');
    };
  }, []);
  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black'
        }}
        onClick={() => onToggle(user.id)}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map(user => (
        <User
          user={user}
          key={user.id}
          onRemove={onRemove}
          onToggle={onToggle}
        />
      ))}
    </div>
  );
}

export default UserList;
```

## deps에 특정 값 넣기

deps에 특정 값을 넣게 되면, 컴포넌트가 처음 마운트 될 때, 지정한 값이 바뀔 때, 언마운트 될 때, 값이 바뀌기 직전 모든 경우 다 호출된다.

useEffect 안에서 사용하는 상태나, props가 있다면 useEffect의 deps에 넣어주어야 한다.

deps 파라미터를 생략하면, 컴포넌트가 리렌더링 될 때마다 호출이 된다.
