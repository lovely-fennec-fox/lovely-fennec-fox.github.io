---
layout: post
title:  "React_ Context API를 사용한 전역 값 관리"
author: fennec-fox
tags: React
img: post-8.jpg
category:  blog
---

<br>

# Context API를 사용한 전역 값 관리

<br>

```react

// Context 사용법

import React, {createContext, useContext } from 'react';

const MyContext = createContext('defaultValue');

function Child() {
    const text = useContext(MyContext); // createContext 한 값을 바로 불러올 수 있다.
}

function Parent() {
    return <Child />
}

function GrandParent() {
    return ( // MyContext.provider로 보낼 값을 감싸준다. 보낼 값은 value 값에 넣어준다.
        <MyContext.Provider value='GOOD'> 
            <Parent />
        </MyContext.Provider>
    )
}

export default GrandParent;

```

<br>

- Context 활용예제

```react

// App.js

import React, { useRef, useReducer, useMemo, useCallback, createContext } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';
import useInputs from './useInputs';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
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

export const UserDispatch = createContext(null); // export를 사용해 createContext내보내기


function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const [form, onChange, reset] = useInputs({
    username: '',
    email: ''
  });
  const { username, email } = form;
  const nextId = useRef(4);

  const { users } = state;


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
    reset();
  }, [username, email, reset]);


  const count = useMemo(() => countActiveUsers(users), [users]);

  return ( // Provider를 사용하여 dispatch값을 손자 컴포넌트에 전달한다.
    <UserDispatch.Provider value={dispatch}> 
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList 
        users={users}
      />
      <div>활성사용자 수 : {count}</div>
    </UserDispatch.Provider>
  );
}

export default App;

-------------------------------------------------------------------------------------
  
// UserList.js
  
import React, { useContext } from 'react';
import { UserDispatch } from './App'; // export로 내보낸 값 받아오기

const User = React.memo(function User({ user }) {
    const { username, email, id, active } = user;
    const dispatch = useContext(UserDispatch); // 받아온 값을 useContext로 사용할 수 있다.

    return (
        <div>
            <b 
                style={
                    color: active ? 'green' : 'black',
                    cursor: 'pointer'
                }
                onClick={ // dispatch 활용
                    () => dispatch({
                        type: 'TOGGLE_USER',
                        id
                    })
                }
            >
            {username}
            </b>
            &nbsp; 
            <span>({email})</span>
            <button onClick={() => dispatch({
                type: 'REMOVE_USER',
                id
            })}>삭제</button>
        </div>
    );
})

function UserList({ users }) {
    return (
        <div> 
            {
                users.map(
                    user => (
                        <User 
                            user={user} 
                            key={user.id} 
                        />
                    )
                )
            }
        </div>
    );
}

export default React.memo(UserList);  

```

<br>

<br>

