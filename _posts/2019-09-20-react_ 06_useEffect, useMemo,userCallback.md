---
layout: post
title:  "React_ useEffect Hook"
author: fennec-fox
tags: React
img: post-8.jpg
category: react
---

<br>

# useEffect Hook

- 컴포넌트가 생성되었을 때, 혹은 컴포넌트가 삭제되었을 때 실행 값을 줄 수 있다.
- 컴포넌트의 props 등이 변경되어 업데이트 되거나, 업데이트 되기 전에 실행 값을 줄 수 있다.
- 리랜더링 될 때마다 실행 값을 줄 수 있다.

```react
import React, { useEffect } from 'react';

useEffect(() => {
  console.log('컴포넌트가 화면에 나타남'); // 업데이트가 있을 때, 실행된다.
  return () => {
    console.log('컴포넌트가 화면에서 사라짐'); // 업데이트를 하기 전 상태를 반환함
  }
}, []); // [ ] 는 기본 값

```

<br>

# useMemo Hook

- 성능을 효과적으로 만드는데 사용한다.
- 특정 값이 바뀌었을 때만, 특정 함수를 실행하게 한다.

```react
import React, { useRef, useState, useMemo } from 'react';

function countActiveUsers(users){
  console.log('활성 사용자 수를 세는중 ..');
  return users.filter(user => user.active).length;
}

// useMemo(실행함수, 기본 값)
// 기본 값인 users가 바뀔 때만 countActiveUsers함수를 호출한다.
const count = useMemo(() => countActiveUsers(users), [users]);

```

<br>

# useCallback Hook

- 함수를 재사용할 수 있도록 해주는데 사용한다.

```react
import React, { useRef, useState, useMemo, useCallback } from 'react'; // useCallback 
import UserList from './UserList';
import CreateUser from './CreateUser';

function countActiveUsers(users){
  console.log('활성 사용자 수를 세는중 ..');
  return users.filter(user => user.active).length;
}

function App() {

  const [inputs, setInputs] = useState({
    username: '',
    email: ''
  });
  const { username, email } = inputs;
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  }, [inputs]);
  const [users, setUsers] = useState([
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
  ]);

  const nextId = useRef(4); 

  const onCreate = useCallback(() => { // 함수를 사용 할 때, useCallback로 감싸준다. 
    const user = {
      id: nextId.current,
      username,
      email,
    }
    setUsers(users.concat(user));
    setInputs({
      username: '',
      email: ''
    });
    nextId.current += 1;
  }, [username, email, users]); // 사용한 변수들을 파라미터로 넣어준다.

  const onRemove = useCallback(id => { // 함수를 사용 할 때, useCallback로 감싸준다. 
    setUsers(users.filter(user => user.id !== id));
  }, [users]);

  const onToggle = useCallback(id => { // 함수를 사용 할 때, useCallback로 감싸준다. 
    setUsers(users.map(
      user => user.id === id
      ? { ...user, active: !user.active}
      : user
    ))
  }, [users]);

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
        onRemove={onRemove}
        onToggle={onToggle}
      />
      <div> 활성 사용자 수: {count} </div>
    </>
  );
}

export default App;

```

<br>

# React.memo

- 컴포넌트 리렌더링 방지를 하기 위해서 사용한다. 

```react

import React from 'react';

function CreateUser({ username, email, onChange, onCreate }) {
    return (
        <div>
            <input 
                name="username" 
                placeholder="계정명" 
                onChange={onChange} 
                value={username} 
            />
            <input
                name="email" 
                placeholder="이메일" 
                onChange={onChange} 
                value={email} 
            />
            <button onClick={onCreate}> 등록 </button>
        </div>
    )
}

export default React.memo(CreateUser); // React.memo 함수로 감싸주면, props의 값이 변경되었을 때만 리렌더링을 한다.

```

<br>

- 다음과 같이 감싸줄 수도 있다.

```react

import React, { useEffect } from 'react';

// React.memo로 컴포넌트를 감싸주고, 변수로 받는다.
const User = React.memo(function User({ user, onRemove, onToggle }) { 
    const { username, email, id, active } = user;

    return (
        <div>
            <b
                style={
                    color: active ? 'green' : 'black',
                    cursor: 'pointer'
                }
                onClick={
                    () => onToggle(id)
                }
            >
            {username}
            </b>
            &nbsp; 
            <span>({email})</span>
            <button onClick={() => onRemove(id)}>삭제</button>
        </div>
    );
})

function UserList({ users, onRemove, onToggle }) {
    return (
        <div> 
            {
                users.map(
                    user => (
                        <User 
                            user={user} 
                            key={user.id} 
                            onRemove={onRemove} 
                            onToggle={onToggle}
                        />
                    )
                )
            }
        </div>
    );
}

export default React.memo(UserList); // React.memo

```

<br>

- 최적화를 위해 디펜던시에서 Users값을 빼주고, callback함수를 만들어준다.

```react

import React, { useRef, useState, useMemo, useCallback } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

function countActiveUsers(users){
  console.log('활성 사용자 수를 세는중 ..');
  return users.filter(user => user.active).length;
}

function App() {

  const [inputs, setInputs] = useState({
    username: '',
    email: ''
  });
  const { username, email } = inputs;
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  }, [inputs]);
  const [users, setUsers] = useState([
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
  ]);

  const nextId = useRef(4); 

  const onCreate = useCallback(() => {
    const user = {
      id: nextId.current,
      username,
      email,
    }
    setUsers(users => users.concat(user)); // callback함수를 사용하면 기존의 값을 불러오기 때문에 의존값에서 users를 빼줄 수 있다.
    setInputs({
      username: '',
      email: ''
    });
    nextId.current += 1;
  }, [username, email]);  // username과 email이 바뀔 때에만 리렌더링 되고, 그러지 않으면 계속 재사용된다.

  const onRemove = useCallback(id => { 
    setUsers(users => users.filter(user => user.id !== id)); // users를 callback로-
  }, []);

  const onToggle = useCallback(id => {
    setUsers(users => users.map( // users를 callback로-
      user => user.id === id
      ? { ...user, active: !user.active}
      : user
    ))
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
        onRemove={onRemove}
        onToggle={onToggle}
      />
      <div> 활성 사용자 수: {count} </div>
    </>
  );
}

export default App;

```

<br>

- 최적화를 하는 다른 방법

```react

import React, { useEffect } from 'react';

const User = React.memo(function User({ user, onRemove, onToggle }) {
    const { username, email, id, active } = user;

    return (
        <div>
            <b 
                style={
                    color: active ? 'green' : 'black',
                    cursor: 'pointer'
                }
                onClick={
                    () => onToggle(id)
                }
            >
            {username}
            </b>
            &nbsp; 
            <span>({email})</span>
            <button onClick={() => onRemove(id)}>삭제</button>
        </div>
    );
})

function UserList({ users, onRemove, onToggle }) {
    return (
        <div> 
            {
                users.map(
                    user => (
                        <User 
                            user={user} 
                            key={user.id} 
                            onRemove={onRemove} 
                            onToggle={onToggle}
                        />
                    )
                )
            }
        </div>
    );
}

// nextProps.Users === prevProps.Users 면, 리렌더링 하지 않게다는 의미
// 다른 값 곧  onRemove, onToggle가 현재 컴포넌트에 영향을 주고 있지 않을 때만 가능하다
// 그렇지 않았을 때는 심각한 오류를 낼 수도 있다.
export default React.memo(UserList, (prevProps, nextProps) => nextProps.Users === prevProps.Users);

```

<br>

<br>

