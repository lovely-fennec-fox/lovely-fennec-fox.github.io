---
layout: post
title:  "React_ 조건부 렌더링"
author: fennec-fox
tags: React
img: post-8.jpg
category:  react
---

<br>

# 조건부 렌더링

<br>

```react

// 부모 컴포넌트

function App() {

  return (
    <Wrapper> 
      {/* true는 javascript의 값이기에 괄호 안에 넣어야 한다. */}
      <Hello name="react" color="red" isSpecial={true}/> 
      <Hello name="react"/>
    </Wrapper>  
  );
}

-----------------------------------------------
  
// 자식 컴포넌트  

function Hello({ color, name, isSpecial }) {
    return(
        <div style={{color}}>
         		{/* 삼항연산자를 사용하여 *를 넣어주는 코드 */}
            {isSpecial ? <b>*</b> : null}
            안녕하세요 {name}
        </div>
    ) 
}

---------------------------- 다음과 같이 && 연산자로 바뀌면 더 깔끔한 코드가 된다.

// 자식 컴포넌트

function Hello({ color, name, isSpecial }) {
    return(
        <div style={{color}}>
        		{/* 논리연산자를 사용하여 *를 넣어주는 코드 */}
            {isSpecial && <b>*</b>}
            안녕하세요 {name}
        </div>
    ) 
}

```

<br>

# useState

<br>

```react

// useState 사용법
// count 코드

import React, { useState } from 'react'; //react에서 useState를 받아온다.

function Counter() {
    const [number, setNumber] =useState(0); // number: 초기 값인 0이 들어간다. setNumber: 업데이트 된 값이 들어간다.

    const onIncrease = () => {
        setNumber(number + 1);
    }

    const onDecrease = () => {
        setNumber(number - 1);
    }

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}> +1 </button>
            <button onClick={onDecrease}> -1 </button>
        </div>
    )
}

export default Counter;


------------------------------------------ 

import React, { useState } from 'react';

function Counter() {
    const [number, setNumber] =useState(0); 

    const onIncrease = () => {
        setNumber(prevNumber => prevNumber + 1); // setNumber에 number값이 들어오므로 prevNumber라는 함수를 넣어서 업데이트를 어떻게 진행할지 보여줄 수도 있다.
    }

    const onDecrease = () => {
        setNumber(prevNumber => prevNumber - 1);
    }

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

<br>

