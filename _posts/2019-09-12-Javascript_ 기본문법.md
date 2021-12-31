---
layout: post
title:  "Javascript_ 기본문법"
author: fennec-fox
tags: Javascript
category:  javascript
---

<br>

# 기본문법

<br>

## 선언

1. `let` : 변수 선언 명령어
2. `const` : 상수 선언 명령어_ 선언 후 재선언을 할 수 없다. 
3. `var` : 똑같은 이름으로 여러번 선언할 수 있어서 예전에 사용하고, 지금은 사용하지 않는 변수 선언 명령어  

```javascript
// 블록이 다를 경우, 같은 이름으로 선언 할 수 있다. 

const a = 1;
if ( a + 1 === 2 ) {
  const a = 2;
  console.log("if문 안의 a :" , a)
}

console.log("if문 밖의 a :" , a)

// if문 안의 a : 2
// if문 안의 a : 1 
```

<br>

## 데이터 타입

1. 문자열 : `' '`, `" "` 둘의 차이는 없으나 대부분 ` ' '` 을 많이 쓴다.
2. `null` , `undefined` 의 차이 : ` null`은 `없다.` 라는 뜻을 의미하고, `undefiend` 는 `아직 정해지지 않았다.`를 뜻한다.

<br>

## 연산자

1. 산술연산자 : `a++` , `++a` 도 가능하다.
2. 논리연산자 : 연산순서는 `not`, `and`, `or` 순서로 연산된다.
3. 비교연산자 : `===` 는 `equals`를 의미함. `==`은 타입을 검사하지 않기 때문에, 실제로는 다른 값도 같게 나올 수 있다. 기본적으로 `===`으로 비교하는 것이 좋다.

<br>

## `switch case` 조건문 사용법

```javascript

const device = 'iphone';

switch (device) {
  case 'iphone':
    console.log('아이폰!');
    break;
  case 'ipad':
    console.log('아이패드!');
    break;
  default : // case중에 해당되지 않는 것들이 이곳으로 들어옴
    console.log('모르겠네요..');    
}

// 아이폰!

```

<br>

## ES6

- ECMAScript6를 의미하며, 2015년에 도입되어 ES2015라고도 불리운다.
- 2019년도에는 ES10 버전이 나왔다.

<br>

1. 문자열 안에 변수 넣기

```javascript

let name = 'AA';

console.log(`Hello, ${name}!`); // ${ } 안에 변수를 넣어준다.

// Hello, AA!

```

<br>

2. 화살표 함수

```javascript

const add = (a, b) => {
  return a + b;
}

const sum = add(1, 2);
console.log(sum);

// 3
```

- `function` 키워드와 `=>` 키워드의 차이점 : `this` 의 값이 다르다.

<br>

## 객체의 비구조화 할당(객체구조분해)

- 객체에서 key의 value값 얻기

```javascript

const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const { name } = ironMan;
console.log(name);

// 토니 스타크

```

<br>

- 기본 객체 사용법

```javascript

const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const captainAmerica = {
  name: '스티븐 로저스',
  actor: '크리스 에반스',
  alias: '캡틴 아메리카'
};

function print(hero) {
  const text = `${hero.alias}(${hero.name}) 역할을 맡은 배우는 ${hero.actor} 입니다.`;
  console.log(text);
}

print(ironMan);
print(captainAmerica);

// 아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
// 캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.

```

<br>

- 객체 구조 분해 사용

```javascript

const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const captainAmerica = {
  name: '스티븐 로저스',
  actor: '크리스 에반스',
  alias: '캡틴 아메리카'
};

function print(hero) {
  const { alias, name, actor } = hero; // 객체를 분해한다. 
  const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`; // 객체의 key값만 주면 value를 얻을 수 있다.
  console.log(text);
}

print(ironMan);
print(captainAmerica);

// 아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
// 캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.

```

<br>

- 파라미터 단계에서 객체 비구조화 할당하기

```javascript

const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const captainAmerica = {
  name: '스티븐 로저스',
  actor: '크리스 에반스',
  alias: '캡틴 아메리카'
};

function print({ alias, name, actor }) { // 파라미터에서 비구조화 할당
  const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`;
  console.log(text);
}

print(ironMan);
print(captainAmerica);

// 아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
// 캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.

```

<br>

## 객체 안에 함수 넣기

1. 기본

```javascript

const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say: function say() {
    console.log(this.sound); // 여기에서 this는 this가 속해있는 dog 객체를 의미한다. 
  }
};

dog.say();

// 멍멍!

```

2. function 이름을 없애도 key값으로 실행가능

```javascript

const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say: function() {   // function 이름을 없애도 key값으로 실행가능 
    console.log(this.sound);
  }
};

dog.say();

// 멍멍!

```

3. function 이름만 주어도 실행가능

```javascript

const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say() {   // function 이름만 주어도 실행가능하다.
    console.log(this.sound);
  }
};

dog.say();

// 멍멍!

```

4. 화살표 함수 : ` 실행이 되지 않는다. this가 가르키는 것이 달라지기 때문이다.`

```javascript

const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say: () => {   // 화살표 함수로 선언했을 때는 this의 값을 찾지 못한다.
    console.log(this)
    console.log(this.sound);
  }
};

dog.say();

// undefined
// TypeError

```

<br>

## this는 해당 객체에서만 사용 가능하다.

```javascript

const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say: function () { 
    console.log(this.sound);
  }
};

const cat = {
  name: '야옹이',
  sound: '야옹~'
};

cat.say = dog.say;
cat.say();
// 야옹~

conset catSay = cat.say; // 변수에 할당했을 때는 this가 값을 찾지 못한다. 
catSay();
// TypeError

```

<br>

## Getter 와 Setter 함수

1. Getter : 값을 조회할 때 사용한다. 

```javascript

const numbers = {
  a: 1,
  b: 2,
  get sum() { // get 선언문 사용
    console.log('sum 함수가 실행됩니다!');
    return this.a + this.b;
  }
};

console.log(numbers.sum);
numbers.b = 5;  // numbers의 b값을 변경하고, 다시조회한다. 
console.log(numbers.sum);

// 3
// 6

```

<br>

2. Setter : 값을 설정할 때 사용한다.

```javascript

const dog = {
  _name : '멍멍이',
  set name(value) { // set 선언문 사용
  	console.log('이름이 바뀝니다..' + value);
    this._name = value;
  }
};

console.log(dog._name);
dog.name = '뭉뭉' // 함수에 값을 주고, 다시 조회한다. 
console.log(dog._name);

// 멍멍이
// 이름이 바뀝니다..뭉뭉
// 뭉뭉

```

<br>

3. Getter, Setter 함수를 함께 사용

```javascript

const dog = {
  _name : '멍멍이',
  get name() { // get 
    console.log('_name을 조회합니다..');
    return this._name;
  },
  set name(value) {  // set
  	console.log('이름이 바뀝니다..' + value);
    this._name = value;
  }
};

console.log(dog.name); // dog._name 이 아닌, dog.name으로 바뀐 값을 얻을 수 있다. 
dog.name = '뭉뭉'
console.log(dog.name);

// _name을 조회합니다.
// 멍멍이
// 이름이 바뀝니다..뭉뭉
// _name을 조회합니다.
// 뭉뭉

```

<br>

4. Getter, Setter 응용

```javascript

const numbers = {
  _a: 1,
  _b: 2,
  sum: 3,
  calculate() {
    console.log('calculate');
    this.sum = this._a + this._b;
  },
  get a() {
    return this._a;
  },
  get b() {
    return this._b;
  },
  set a(value) {
    console.log('a가 바뀝니다.');
    this._a = value;
    this.calculate();
  },
  set b(value) {
    console.log('b가 바뀝니다.');
    this._b = value;
    this.calculate();
  }
};

console.log(numbers.sum);
numbers.a = 5; 
numbers.b = 7;
numbers.a = 9; // 값이 바뀔 때만 연산이 실행되는 것을 볼 수 있다.
console.log(numbers.sum);
console.log(numbers.sum); 
console.log(numbers.sum); // 해당 값을 불러오기만 함.

// 3
// a가 바뀝니다.
// calculate
// b가 바뀝니다.
// calculate
// a가 바뀝니다.
// calculate
// 16
// 16
// 16

```

<br>

## 배열

- javascript의 배열은 각각의 인덱스에 다른 type의 값을 넣어도 괜찮다. 

- 기본내장함수 

  1. ` push` : 새로운 값을 넣어준다. 

  ```javascript
  
  const objects = [{ name: '멍멍이' }, { name: '야옹이' }];
  
  objects.push({
    name: '멍뭉이'
  });
  
  ```

  <br>

  2. `length` : 배열의 크기 알아내기

  ```javascript
  
  console.log(objects.length);
  
  // 3
  
  ```

  <br>

  3. `forEach` : 배열의 for문

  ```javascript
  
  const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];
  
  // 사용법 1
  function print(hero) {
    console.log(hero);
  }
  
  superheroes.forEach(print)
  
  // 사용법 2
  superheroes.forEach(function(hero) {
    console.log(hero);
  })
  
  // 사용법 3
  superheroes.forEach(hero => {
    console.log(hero);
  });
  
  ```

  <br>

  4. `map` : 새롭게 배열을 다시  만듦

  ```javascript
  
  const array = [1, 2, 3, 4, 5, 6, 7, 8];
  
  // forEach로 구현하기
  const squared = [];
  
  array.forEach(n => {
    squared.push(n * n);
  });
  
  console.log(squared);
  
  // map 사용법 1
  const square = n => n * n;
  const squared = array.map(square);
  console.log(squared);
  
  // map 사용법 2
  const squared = array.map(n => n * n);
  console.log(squared);
  
  ```

  <br>

  - map 활용하기

  ```javascript
  
  const items = [
    {
      id: 1,
      text: 'hello'
    },
    {
      id: 2,
      text: 'bye'
    }
  ];
  
  const texts = items.map(item => item.text);
  console.log(texts);
  
  // ["hello", "bye"]
  
  ```

  <br>

  5. `indexOf` : 특정 항목이 배열의 몇 번째 인지 알아봄 / return값이 -1 이면, 일치하는 값이 없다는 것

  ```javascript
  
  const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];
  
  const index = superheroes.indexOf('토르');
  console.log(index);
  
  // 2
  
  ```

  <br>

  6. `findIndex` : 배열 안에 있는 값이 객체이거나, 배열일 때 사용한다. 

  ```javascript
  
  const todos = [
    {
      id: 1,
      text: '자바스크립트 입문',
      done: true
    },
    {
      id: 2,
      text: '함수 배우기',
      done: true
    },
    {
      id: 3,
      text: '객체와 배열 배우기',
      done: true
    },
    {
      id: 4,
      text: '배열 내장함수 배우기',
      done: false
    }
  ];
  
  const index = todos.findIndex(todo => todo.id === 3); // 검사하고자 하는 조건을 반환하는 함수를 넣어서 찾을 수 있습니다.
  console.log(index);
  
  // 2
  
  ```

  <br>

  7. ` find` : `findIndex` 랑 비슷한데, 찾아낸 값이 몇번째인지 알아내는 것이 아니라, 찾아낸 값 자체를 반환합니다.

  ```javascript
  
  const todos = [
    {
      id: 1,
      text: '자바스크립트 입문',
      done: true
    },
    {
      id: 2,
      text: '함수 배우기',
      done: true
    },
    {
      id: 3,
      text: '객체와 배열 배우기',
      done: true
    },
    {
      id: 4,
      text: '배열 내장함수 배우기',
      done: false
    }
  ];
  
  const todo = todos.find(todo => todo.id === 3); // find 사용
  console.log(todo);
  
  // {id: 3, text: "객체와 배열 배우기", done: true}
  
  ```

  <br>

  8. `filter` : 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열을 만듭니다.

  ```javascript
  
  const todos = [
    {
      id: 1,
      text: '자바스크립트 입문',
      done: true
    },
    {
      id: 2,
      text: '함수 배우기',
      done: true
    },
    {
      id: 3,
      text: '객체와 배열 배우기',
      done: true
    },
    {
      id: 4,
      text: '배열 내장함수 배우기',
      done: false
    }
  ];
  
  // 사용법 1
  const tasksNotDone = todos.filter(todo => todo.done === false);
  console.log(tasksNotDone);
  
  // 사용법 2
  const tasksNotDone = todos.filter(todo => !todo.done);
  
  // [
  //  {
  //    id: 4,
  //    text: '배열 내장 함수 배우기',
  //    done: false
  //  }
  // ];
  
  ```

  <br>

  9. `splice` : 배열의 특정 항목을 지울 때 사용한다. index값을 주어야 한다.

  ```javascript
  
  const numbers = [10, 20, 30, 40];
  const index = numbers.indexOf(30);
  const spliced = numbers.splice(index, 2); // splice(시작 인덱스, 지울 갯수)
  console.log(spliced);
  console.log(numbers);
  
  // [ 30, 40 ]
  // [ 10, 20 ]
  
  ```

  <br>

  10. ` slice` : 배열을 잘라낼 때 사용하는데, 중요한 점은 기존의 배열은 건들이지 않는 다는 것입니다.

  ```javascript
  
  const numbers = [10, 20, 30, 40];
  const sliced = numbers.slice(0, 2); // slice(시작 인덱스, 끝 인덱스)
  
  console.log(sliced); // [10, 20]
  console.log(numbers); // [10, 20, 30, 40] 
  
  ```

  <br>

  11. `shift` : 첫 번째 원소를 추출해준다.  /  ` pop` : 마지막 원소를 추출해준다.  /  `unshift` : 맨 앞에 새 원소를 추가한다.

  ```javascript
  
  // shift
  const numbers = [10, 20, 30, 40];
  const value = numbers.shift();
  console.log(value); // 10
  console.log(numbers); // [20, 30, 40]
  
  // pop
  const numbers = [10, 20, 30, 40];
  const value = numbers.pop();
  console.log(value); // 40
  console.log(numbers); // [10, 20, 30]
  
  
  // unshift
  const numbers = [10, 20, 30, 40];
  numbers.unshift(5);
  console.log(numbers); // [5, 10, 20, 30, 40]
  
  ```

  <br>

  12. `concat` : 여러개의 배열읠 하나의 배열로 합쳐줌  / `join` : 배열 안의 값들을 문자열 형태로 합쳐줍니다.

  ```javascript
  
  // concat
  const arr1 = [1, 2, 3];
  const arr2 = [4, 5, 6];
  const concated = arr1.concat(arr2);
  console.log(arr1); // [1, 2, 3] 기존 배열이 바뀌지 않는다. 
  console.log(concated); // [1, 2, 3, 4, 5, 6];
  
  
  // join(구분자)
  const array = [1, 2, 3, 4, 5];
  console.log(array.join()); // 1,2,3,4,5
  console.log(array.join(' ')); // 1 2 3 4 5
  console.log(array.join(', ')); // 1, 2, 3, 4, 5
  
  ```

  <br>

  13. `reduce` : 배열안에 있는 모든 값을 이용하여 연산을 할 때 유용하다.

  ```javascript
  
  // 배열의 모든 값을 더한다고 할 때,
  
  const numbers = [1, 2, 3, 4, 5];
  
  // accumulator : 누적된 값이 저장되는 변수
  // current : 배열의 각각의 값
  // reduce(함수, accumulator의 초기값)
  let sum = numbers.reduce((accumulator, current) => accumulator + current, 0);
  
  console.log(sum); // 15
  
  ```

  - 활용

  ```javascript
  
  const numbers = [1, 2, 3, 4, 5];
  
  // index : 배열의 몇 번째 index인지 알려준다.
  // array : 함수를 실행하고 있는 자기자신을 의미한다.
  let sum = numbers.reduce((accumulator, current, index, array) => {
    if (index === array.length - 1) {
      return (accumulator + current) / array.length;
    }
    return accumulator + current;
  }, 0);
  
  console.log(sum); // 3
  
  ```

  <br>

  - 활용 2

  ```javascript
  
  const alphabets = ['a', 'a', 'a', 'b', 'c', 'c', 'd', 'e'];
  const counts = alphabets.reduce((acc, cur) => {
    if (acc[cur]) {
      acc[cur] += 1;
    } else {
      acc[cur] = 1;
    }
    return acc;
  }, {}) // 초기 값을 빈 객체로 시작
  
  console.log(counts); // Object {a: 3, b: 1, c: 2, d: 1, e: 1}
  
  ```

<br>

## 반복문

- `for .. of ..`  : 배열의 반복문으로 사용하나, 더 쉬운 방법이 있다. 

```javascript

let numbers = [10, 20, 30, 40, 50];
for (let number of numbers) {
  console.log(number);
}

```

<br>

` [ 참고 ] ` : 객체의 리스트화

```javascript

const doggy = {
  name: '멍멍이',
  sound: '멍멍',
  age: 2
};

console.log(Object.entries(doggy)); // entries
// [Array[2], Array[2], Array[2]]
// 0: Array[2]
// 		0: "name"
// 		1: "멍멍이"
// 1: Array[2]
// 		0: "sound"
// 		1: "멍멍"
// 2: Array[2]
// 		0: "age"
//		1: 2

console.log(Object.keys(doggy)); // keys
// ["name", "sound", "age"]
// 	0: "name"
// 	1: "sound"
// 	2: "age"

console.log(Object.values(doggy)); // values
// ["멍멍이", "멍멍", 2]
//	0: "멍멍이"
//	1: "멍멍"
//	2: 2
 
```

<br>

- `for .. in .. ` : 객체의 반복문으로 많이 사용한다. 

```javascript

const doggy = {
  name: '멍멍이',
  sound: '멍멍',
  age: 2
};

for (let key in doggy) {
  console.log(`${key}: ${doggy[key]}`);
}

// name: 멍멍이 
// sound: 멍멍 
// age: 2 
```

<br>

## 프로토타입과 클래스(객체 생성자)

```java

function Animal(type, name, sound) { // 객체생성함수는 대문자로 시작하는 것이 관례
  this.type = type;
  this.name = name;
  this.sound = sound;
  this.say = function() {
    console.log(this.sound);
  } 
}

const dog = new Animal('개', '멍멍이', '멍멍'); // 객체 생성
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say(); // 멍멍
cat.say(); // 야옹

```

<br>

- 위의 코드를 이용하여 '프로토타입' 활용하기

```javascript

function Animal(type, name, sound) { // 객체생성함수는 대문자로 시작하는 것이 관례
  this.type = type;
  this.name = name;
  this.sound = sound;
}

// 객체생성함수서 공통된 함수를 가질 때, 함수를 재사용 할 수 있도록 해준다. 
Animal.prototype.say = function() {
    console.log(this.sound);
}

const dog = new Animal('개', '멍멍이', '멍멍'); // 객체 생성
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say(); // 멍멍
cat.say(); // 야옹

```

<br>

- 객체 생성자 상속하기

```javascript

function Animal(type, name, sound) { // 객체생성함수는 대문자로 시작하는 것이 관례
  this.type = type;
  this.name = name;
  this.sound = sound;
}

Animal.prototype.say = function() {
    console.log(this.sound);
}

// Animal함수를 상속함
function Dog(name, sound) {
  // 객체생성자 자기자신인 this값을 파라미터 값으로 줘야함.
  Anilmal.call(this, '개', name, sound);
}

function Cat(name, sound) {
  Anilmal.call(this, '고양이', name, sound);
}

// prototype을 맞춰주어야 함
Dog.prototype = Animal.prototype;
Cat.prototype = Animal.prototype;

const dog = new Dog('멍멍이', '멍멍'); // 객체 생성
const cat = new Cat('야옹이', '야옹');


dog.say(); // 멍멍
cat.say(); // 야옹

```

<br>

- `클래스(ES6)` // 프로토타입을 조금 더 쉽게 사용하려고 나옴.

```javascript

// class로 선언
class Animal {
  
  // 구조를 먼저 잡아줌
  constructor(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
  } 
  
  // 함수 추가 가능, 자동으로 prototype으로 설정된다. 
  say() {
    console.log(this.sound);
  }
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say(); // 멍멍
cat.say(); // 야옹

```

<br>

- `클래스 상속 활용하기` 

```javascript

class Animal {
  constructor(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
  } 
  
  say() {
    console.log(this.sound);
  }
}

class Dog extends Animal {
  constructor(name, sound) {
    // 부모의 구조를 재구성함
    super('개', name, sound)
  }
}

class Cat extends Animal {
  constructor(name, sound) {
    // 부모의 구조를 재구성함
    super('고양이', name, sound)
  }
}

const dog = new Dog('멍멍이', '멍멍'); // 객체 생성
const cat = new Cat('야옹이', '야옹');


dog.say(); // 멍멍
cat.say(); // 야옹

```



