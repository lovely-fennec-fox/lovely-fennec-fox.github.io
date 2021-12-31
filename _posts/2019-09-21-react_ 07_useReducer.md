---
layout: post
title:  "React_ useReducer Hook"
author: fennec-fox
tags: React
img: post-8.jpg
category:  react
---

<br>

# useReducer Hook

<br>

- useState가 아닌 useReducer를 통해서 상태를 변화시켜줄 수 있다.

- `useState와 useReducer의 차이`

  - useState : ex) setValue(5) 와 같이 바뀔 값을 지정해주면 상태가 업데이트가 된다.

  - useReducer : ex) dispatch({ type: 'INCREMENT'}) 와 같이 액션객체를 통해 상태를 업데이트를 한다.

    ​                      상태 업데이트 로직을 컴포넌트 밖으로 분리가 가능하다.

- `reducer` : 상태를 업데이트 하는 함수

```react

// reducer는 다음과 같이 사용한다.

function reducer(state, action) { // 현재 상태와 액션객체를 받아와서 상태를 업데이트 한다.
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT': 
      return state - 1;
    default:   
   		return state;
  }
}

----------------------------------------------------------------

// useReducer 사용법
// useReducer(reducer함수, 기본 값)
// [number = 현재상태, dispatch = 액션을 발생시키는 함수]
const [number, dispatch] = useReducer(reducer, 0);


```

<br>

- `useReducer` 를 활용한 Counter 코드

```react

import React, { useReducer } from 'react'; //react에서 useState를 받아온다.


// 1. reducer 함수를 만든다.
function reducer(state, action){
    switch (action.type) {
        case 'INCREMENT':
          return state + 1;
        case 'DECREMENT': 
          return state - 1;
        default:   
            return state;
        // 아니면 throw new Error('Unhandled action');    
    }
}


function Counter() {

  	// 2. useReducer을 정의한다.
    // [현재상태, 액션을 발생하는 함수] 
    const [number, dispatch] = useReducer(reducer, 0);

  
  	// 3. dispatch의 상태를 정의한다.
    const onIncrease = () => {
        dispatch({
            type: 'INCREMENT'
        })
    };

    const onDecrease = () => {
        dispatch({
            type: 'DECREMENT'
        })
    };

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}> +1 </button>
            <button onClick={onDecrease}> -1 </button>
        </div>
    )
}

export default Counter;

```

<br>

- `useReducer` 를 활용한 예제

```react

import React, { useRef, useReducer, useMemo, useCallback } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
  inputs: {
    username: '',
    email: ''
  },
  users: [
    {
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com',
      active: true
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false
    }
  ]
};

function reducer(state, action) {
  switch (action.type) {
    case 'CHANGE_INPUT':
      return {
        ...state,
        inputs: {
          ...state.inputs,
          [action.name]: action.value
        }
      };
    case 'CREATE_USER':
      return {
        inputs: initialState.inputs,
        users: state.users.concat(action.user)
      };
    case 'TOGGLE_USER':
      return {
        ...state,
        users: state.users.map(user => 
          user.id === action.id
            ? { ...user, active: !user.active }
            : user
        )
      };
    case 'REMOVE_USER':
      return {
        ...state,
        users: state.users.filter(user => user.id !== action.id)
      }    
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const nextId = useRef(4);

  const { users } = state;
  const { username, email } = state.inputs;

  const onChange = useCallback(e => {
    const { name, value } = e.target;
    dispatch({
      type: 'CHANGE_INPUT',
      name,
      value
    });
  }, []);

  const onCreate = useCallback(() => {
    dispatch({
      type: 'CREATE_USER',
      user: {
        id: nextId.current,
        username,
        email
      }
    });
    nextId.current += 1;
  }, [username, email]);

  const onToggle = useCallback(id => {
    dispatch({
      type: 'TOGGLE_USER',
      id
    });
  }, []);

  const onRemove = useCallback(id => {
    dispatch({
      type: 'REMOVE_USER',
      id
    });
  }, []);

  const count = useMemo(() => countActiveUsers(users), [users]);

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList 
        users={users}
        onToggle={onToggle}
        onRemove={onRemove}
      />
      <div>활성사용자 수 : {count}</div>
    </>
  );
}

export default App;

```

<br>

<br>