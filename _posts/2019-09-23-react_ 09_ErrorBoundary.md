---
layout: post
title:  "React_ ErrorBoundary"
author: fennec-fox
tags: React
img: post-8.jpg
category:  react
---

<br>

# 에러가 발생하는 상황

```react

// App.js 

import React from 'react';
import User from './User';

function App() {
  const user = {
    id: 1,
    username: 'velopert'
  };
 // return <User user={user} />;
 // 위와 같이 user값을 props로 넘겨주지 않으면 에러가 발생하게 된다. 
  return <User />;
}

export default App;

----------------------------------------------------------

// user.js

import React from 'react';

function User({ user }) {
  return (
    <div>
      <div>
        <b>ID</b>: {user.id}
      </div>
      <div>
        <b>Username:</b> {user.username}
      </div>
    </div>
  );
}

export default User;


```

<br>

# 에러처리

```react

// ErrorBoundary.js

import React, { Component } from 'react';

class ErrorBoundary extends Component {
  state = {
    error: false
  };

  componentDidCatch(error, info) {
    console.log('에러가 발생했습니다.');
    console.log({
      error,
      info
    });
    this.setState({
      error: true
    });
  }

  render() { // 에러가 발생하면 에러발생을 return하고, 그렇지 않으면 자녀컴포넌트를 그대로 return해준다.
    if (this.state.error) { 
      return <h1>에러 발생!</h1>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;

----------------------------------------------------------------------------

// App.js

import React from 'react';
import User from './User';
import ErrorBoundary from './ErrorBoundary';

function App() {
  const user = {
    id: 1,
    username: 'velopert'
  };
  return ( // ErrorBoundary로 실행코드를 감싸준다.
    <ErrorBoundary>
      <User />
    </ErrorBoundary>
  );
}

export default App;

```

<br>

<br>

