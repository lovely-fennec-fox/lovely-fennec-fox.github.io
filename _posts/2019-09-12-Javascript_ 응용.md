---
layout: post
title:  "Javascript_ 응용"
author: fennec-fox
tags: Javascript
category:  blog
---

<br>


## 삼항연산자

```javascript

// condition이 true이면 true반환, false면 false반환
condition ? true : false

```

<예시>

```javascript

const array = [1, 2];
let text = array.length === 0 
	? '배열이 비어있습니다.' 
	: '배열이 비어있지 않습니다.'

console.log(text); // 배열이 비어있지 않습니다.

```

<br>

- 중첩 삼항연산자(잘 사용하지 않음)

```javascript

const condition1 = false;
const condition2 = false;

const value = condition1
	? 'A'
	: condition2
		? 'B'
		: 'C'

console.log(value); // 'C'

```

<br>

## Truthy 와 Falsy

```javascript

// Falsy 한 값 (false는 아니지만, false를 뜻하는 값)
1. undefined
2. null
3. 0
4. ''
5. NaN 

// Truthy 한 값
- 위의 5개 빼고 모든 값

```

<br>

## 단축 평가 논리 계산법

```javascript

// [ && 연산 ]

// 앞의 값이 true이면 뒤의 값을 리턴함
console.log(true && 'hello');  // hello
// 앞의 값이 false이면 뒤의 값은 보지 않고 앞의 것을 리턴함
console.log(false && 'hello'); // false


// [ || 연산 ]

// 앞의 값이 true이면 뒤의 값은 보지 않고 앞의 것을 리턴함
console.log(true || 'hello');
// 앞의 값이 false이면 뒤의 값을 리턴함 
console.log(false || 'hello');

```

<br>

## 함수의 기본 파라미터

```javascript

// 파라미터 값이 들어오지 않았을 때 에러를 내지 않도록 하는 것

function calculate(r) {
  const radius = r || 1; // 파라미터 값이 들어오지 않았을 때, 1이 return됨
  return Math.PI * radius * radius;
}

const area = calculate(); // 파라미터 값을 주지 않음
console.log(area)

----------------------------------------

// ES6 문법
function calculate(r = 1) {
  return Math.PI * r * r;
}

const area = calculate(); // 파라미터 값을 주지 않음
console.log(area)
```

<br>

## 조건문 더 스마트하게 쓰기

```javascript

// 조건문
function isAnimal(text) {
  return (
    text === '고양이' || text === '개' || text === '거북이' || text === '너구리'
  );
}

console.log(isAnimal('개')); // true
console.log(isAnimal('노트북')); // false

---------------------------------------------------
  
// includes 명령어 사용 
function isAnimal(name) {
  const animals = ['고양이', '개', '거북이', '너구리'];
  return animals.includes(name);
}

console.log(isAnimal('개')); // true
console.log(isAnimal('노트북')); // false  

```

<br>

## 비구조화 할당

```javascript

const object = { a: 1 };

const { a, b = 2 } = object; // 비구조화 할 때, 초기 값을 지정해 줄 수 있다. 

console.log(a); // 1
console.log(b); // 2

--------------------------------------------------

const animal = {
  name: '멍멍이',
  type: '개'
};

const { name: nickname } = animal // 새로운 이름으로 지정해 줄 수도 있다.
console.log(nickname); // 멍멍이

---------------------------------------------------
 
const array = [1];
const [one, two = 2] = array; // 배열도 다음과 같이 비구조화 할당을 할 수 있고, 초기값을 지정할 수 있다.

console.log(one); // 1
console.log(two); // 2 

----------------------------------------------------

const deepObject = {
  state: {
    information: {
      name: 'velopert',
      languages: ['korean', 'english', 'chinese']
    }
  },
  value: 5
};

const { name, languages } = deepObject.state.information; // 다음과 같이 추출해줄 수 있다.
const { value } = deepObject;

const extracted = { // 그리고 한꺼번에 할당해서 줄 수 있다. 
  name,
  languages,
  value
};

console.log(extracted); // {name: "velopert", languages: Array[3], value: 5}
  
```

<br>

## spread 와 rest

- `spread `: 값을 퍼트리는 역할

```javascript

// spread 연산자  '...'

const slime = {
  name: '슬라임'
};

const cuteSlime = {
  ...slime, // name: '슬라임' 과 같다.
  attribute: 'cute'
};

const purpleCuteSlime = {
  ...cuteSlime, // name: '슬라임', attribute: 'cute'
  color: 'purple'
};

```

<br>

- `rest` : 남은 값을 담는 변수 

```javascript

// rest 연산

const numbers = [0, 1, 2, 3, 4, 5, 6];

const [one, ...rest] = numbers; // 배열에서 rest는 맨 마지막에 와야 한다. 앞에 오면 에러남 

console.log(one); // 0 
console.log(rest); // [1, 2, 3, 4, 5, 6];

----------------------------------------------

// 파라미터 값으로도 쓸 수있다. 
function sum(...rest) {
  return rest.reduce((acc, current) => acc + current, 0);
}

const result = sum(1, 2, 3, 4, 5, 6);
console.log(result); // 21

```

<br>

## hoisting

```javascript

myFunction(); // 함수가 선언되기 전에 호출했어도 정상적으로 작동한다. 

function myFunction() {
  console.log('hello world!');
}

// hello world

```

- 위와 같이 잘 작동하는 이유는 자바스크립트 엔진이 위 코드를 해석하는 과정에서 다음의 코드와 같이 받아들이기 때문입니다.

```javascript

function myFunction() {
  console.log('hello world!');
}

myFunction();

```

- 이런 현상을 `hoisting` 이라고 한다.  
- 자바스크립트의 특징으로 잘 작동하기는 하나, 파악하기 어려운 코드가 되기 때문에 피해야 하는 코드이다. 
- `const` 와 `let`은 hoisting이 발생하지 않는다. 

<br>

