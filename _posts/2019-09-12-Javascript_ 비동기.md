---
layout: post
title:  "Javascript_ 비동기"
author: fennec-fox
tags: Javascript
category:  blog
---

<br>


# Promise

- 사용법

```javascript
// 성공할 때 resolve, 실패할 때 reject

const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('result');
  }, 1000);
});

// myPromise 함수 이후 실행될 함수
myPromise.then(result => {
  console.log(result);
});

const myPromise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error());
  }, 1000);
});

// myPromise2 함수 이후 실행될 함수
myPromise2
  .then(result => {
    console.log(result);
  })
  .catch(e => {
    console.log(e);
  });

```

<br>

- 활용

```javascript

function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    console.log('result: ', n);
    return increaseAndPrint(n);
  })
  .catch(e => {
    console.error(e);
  });


// 1
// 2
// 3
// 4
// ValueIsFiveError

------------------------------------------------------- 조금 더 간단히

function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(increaseAndPrint) // 파라미터는 자동으로 넘어온다. 
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .catch(e => {
    console.error(e);
  });

```

<br>

- Promise의 단점 
  - 어디에서 에러가 나는지 찾기가 어렵다. 
  - `.then`으로 계속 이어져서 분기를 나누기 어렵다. 

<br>

# async / await (ES6)

- 사용법

```javascript

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function process() {
  console.log('안녕하세요!');
  await sleep(1000);
  console.log('반갑습니다!');
}

process()

// 안녕하세요! 
// (1초 후) 반갑습니다! 
```

<br>

- `async의 return값은 promise 이다.`

```javascript

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function process() {
  console.log('안녕하세요!');
  await sleep(1000);
  console.log('반갑습니다!');
  return true;
}

process().then(value => {
  console.log(value); // promise의 리턴 값을 받아온다.
});

// 안녕하세요! 
// 반갑습니다! 
// true

```

<br>

- `async에서 에러를 잡을 때는 try, catch를 사용한다.`

```javascript

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function makeError() {
  await sleep(1000);
  const error = new Error();
  throw error; // 에러를 발생시킬 때는 throw를 쓴다.
}

async function process() {
  try {
    await makeError();
  } catch (e) {
    console.error(e); // 1초 후 error를 발생시킨다.
  }
}

process();

```

<br>

<br>

## 비동기 함수 async / await 사용하여 구현

```javascript

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000);
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500);
  return '토끼';
};

const getTurttle = async () => {
  await sleep(3000);
  return '거북이';
};

async function process() {
  const dog = await getDog();
  console.log(dog);
  const rabbit = await getRabbit();
  console.log(rabbit);
  const turtle = await getTurttle();
  console.log(turtle);
}

process();

```

<br>

### 모든 promise 실행할 때_ promise.all

- 한 개의 promise에서만 에러가 나도 전체적으로 에러가 발생한다.

```javascript

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000); // 1초
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500); // 0.5초
  return '토끼';
};

const getTurtle = async () => {
  await sleep(3000); // 3초
  return '거북이';
};

async function process() {
  // 3개의 함수의 return값을 results에 리스트로 들어가는데 값이 나오는데 3초가 걸린다. 
	const results = await Promise.all([getDog(), getRabbit(), getTurtle()]);
  console.log(results);
}

process();


------------------------------------------------- 다음과 같은 방법으로 나눠줄 수도 있다.

async function process() {
  // 3개의 함수의 return값을 results에 리스트로 들어가는데 값이 나오는데 3초가 걸린다. 
	const [dog, rabbit, turtle] = await Promise.all([getDog(), getRabbit(), getTurtle()]);
  console.log(dog);
  console.log(rabbit);
  console.log(turtle);
}

```

<br>

### Promise중 제일 먼저 실행된 것만 필요할 때_ promise.race

```javascript

async function process() {
	const first = await Promise.race([getDog(), getRabbit(), getTurtle()]);
  console.log(first);
}

// 토끼

```

<br>

<br>

