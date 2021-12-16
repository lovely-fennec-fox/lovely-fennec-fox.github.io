---
layout: post
title:  "React_ Input값 처리하기"
author: fennec-fox
tags: React
img: post-8.jpg
category:  blog
---

<br>

# Input 값 처리하기

<br>

```react

import React, { useState } from 'react';

function InputSample() {

    const [text, setText] = useState('');
    const onChange = (e) => { // input값을 가져와서 처리하는 로직
        setText(e.target.value);
    }
    
    const onReset = () => {
        setText('');
    }

    return (    
        <div>
            <input onChange={onChange} value={text}/>
            <button onClick={onReset}> 초기화 </button>
            <div>
                <b> 값 : </b>
                {text}
            </div>
        </div>
    )
}

export default InputSample;


```

<br>

# 여러개의 input 상태 관리하기

<br>

```react

import React, { useState } from 'react';

function InputSample() {
    const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
    });

    const { name, nickname } = inputs;
    const onChange = (e) => {
        const { name, value } = e.target;

        // const nextInputs = {
        //     ...inputs,     // 객체의 값을 변경할 때는 기존의 값을 불러온 다음 변경시켜야 한다.  
        //     [name]: value, // name으로 들어오는지, nickname으로 들어오는지 키 값을 변수로 받음 
        // };
        // setInputs(nextInputs); // 새로운 값을 세팅해줌
      
				// 조금 더 간단하게 나타내면 다음과 같다.
        setInputs({
            ...inputs,
            [name]: value,
        });        
    }
    
    const onReset = () => {
        setInputs({
            name: '',
            nickname: ''
        });        
    }

    return (    
        <div>
            <input 
                name="name" 
                placeholder="이름" 
                onChange={onChange} 
                value={name}
            />
            <input 
                name="nickname" 
                placeholder="닉네임" 
                onChange={onChange} 
                value={nickname}
            />
            <button onClick={onReset}> 초기화 </button>
            <div>
                <b> 값 : </b>
                {name} ({nickname})
            </div>
        </div>
    )
}

export default InputSample;

```

<br>

# useRef

- 특정 DOM을 컨트롤을 하기 위해 사용한다.

```react

import React, { useState, useRef } from 'react';

function InputSample() {

    const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
    });

    const nameInput = useRef(); // currnet라는 명령어를 가짐
    const { name, nickname } = inputs;
    const onChange = (e) => {
        const { name, value } = e.target;
        setInputs({
            ...inputs,
            [name]: value,
        }); 
        
        nameInput.current.focus(); //current 후, dom의 기능을 선택할 수 있다. 그 중에 focus를 선택함       
    }
    
    const onReset = () => {
        setInputs({
            name: '',
            nickname: ''
        });        
    }

    return (    
        <div>
            <input 
                name="name" 
                placeholder="이름" 
                onChange={onChange} 
                value={name}
                ref={nameInput} // useRef객체를 ref값으로 넣어준다.
            />
            <input 
                name="nickname" 
                placeholder="닉네임" 
                onChange={onChange} 
                value={nickname}
            />
            <button onClick={onReset}> 초기화 </button>
            <div>
                <b> 값 : </b>
                {name} ({nickname})
            </div>
        </div>
    )
}

export default InputSample;

```

