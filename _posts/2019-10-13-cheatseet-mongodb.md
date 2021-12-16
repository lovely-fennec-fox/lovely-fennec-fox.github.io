---
layout: post
title: CheatSeet_ MongoDB
description: CheatSeet_ MongoDB
img: post-8.jpg
tags: [CheatSeet, MongoDB]
author: fennec-fox
---

<br>

#### 1) MongoDB 서버 상태 확인

```bash

$ mongostat

```

<br>

<br>

# 1. Mongoose

<br>

## 1 ]  connect

```javascript

mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true }).
try {
  await mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true });
} catch (error) {
  handleError(error);
}

// 다음과 같이 옵션을 더 줄 수도 있다.
// mongoose.connect('mongodb://username:password@host:port/database?options...', {useNewUrlParser: true});

```

<br>

<br>

## 2 ] schema

#### 1) schema 생성

```javascript

const mongoose = require('mongoose');

const imageSchema = new mongoose.Schema({
  width: Number,
  height: Number,
});

const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true, lowercase: true },
  // 이메일은 문자열이고, 필수이고, 유일해야 하고, 소문자여야 한다.
  password: { type: String, required: true, trim: true },
  // password는 문자열이고, 필수이고, 공백을 제거한다.
  nickname: String,
  birth: { type: Date, default: Date.now },
  // 생일은 기본 값으로 현재 시간이 들어간다.
  point: { type: Number, default: 0, max: 50, index: true },
  // point 필드는 숫자이고, 기본 값은 0이고, 최댓값은 50, 빠르게 찾을 수 있도록 인덱스를 넣어준다.
  image: imageSchema,
  // image필드에는 객체가 들어가는데 스키마 안에 스키마가 들어가는 것이 가능하다.
  likes: [String],
  // like 필드는 문자열의 배열이다.
  any: [mongoose.Schema.Types.Mixed ],
  // 아무거나 넣어도 되는 필드이다.
  id: mongoose.Schema.Types.ObjectId,
  // ObjectId를 넣을 수 있는 필드이다.
});
userSchema.index({ email: 1, nickname: 1 });
// 복합인덱스가 가능하다.


// Create Model & Export
module.exports = mongoose.model('User', userSchema);
// 첫번째 인자가 User이면 소문자화 후 복수형으로 바꿔서 users 컬렉션이 됩니다

```
<br>

#### 2) 스키마 적용하기

```javascript

require('./경로/userSchema'); 

```
<br>

#### 3) 스키마 + virtual

```javascript

// define a schema
var personSchema = new Schema({
  name: {
    first: String,
    last: String
  }
});

// compile our model
var Person = mongoose.model('Person', personSchema);

// create a document
var axl = new Person({
  name: { first: 'Axl', last: 'Rose' }
});


// virtual
personSchema.virtual('fullName').get(function () {
  return this.name.first + ' ' + this.name.last;
});

console.log(axl.fullName); // Axl Rose


```
<br>

#### 4) 스키마 + 인스턴스 메소드

```javascript

  // define a schema
  var animalSchema = new Schema({ name: String, type: String });

  // assign a function to the "methods" object of our animalSchema
  // 화살표 함수는 작동하지 않을 수 있으니, function 명령어를 사용하자.
  animalSchema.methods.findSimilarTypes = function(cb) {
    return this.model('Animal').find({ type: this.type }, cb);
  };

  var Animal = mongoose.model('Animal', animalSchema);
  var dog = new Animal({ type: 'dog' });



  dog.findSimilarTypes(function(err, dogs) {
    console.log(dogs); // woof
  });

```

<br>

#### 5) 스키마 + static 메소드

```javascript

  // Assign a function to the "statics" object of our animalSchema
  animalSchema.statics.findByName = function(name) {
    return this.find({ name: new RegExp(name, 'i') });
  };
  // Or, equivalently, you can call `animalSchema.static()`.
  animalSchema.static('findByBreed', function(breed) {
    return this.find({ breed });
  });

  const Animal = mongoose.model('Animal', animalSchema);
  let animals = await Animal.findByName('fido');
  animls = animals.concat(await Animal.findByBreed('Poodle'));

```

<br>

#### 6) 스키마 + query

```javascript

  animalSchema.query.byName = function(name) {
    return this.where({ name: new RegExp(name, 'i') });
  };

  var Animal = mongoose.model('Animal', animalSchema);

  Animal.find().byName('fido').exec(function(err, animals) {
    console.log(animals);
  });

  Animal.findOne().byName('fido').exec(function(err, animal) {
    console.log(animal);
  });

```

- statics은 모델에서 바로 쓸 수 있는 메소드이고, query는 한 번 쿼리 후에 이어서 쓰는 메소드입니다. 

<br>

#### 7) 스키마 + pre / post

```javascript

// 특정 동작 이전, 이후에 어떤 행동을 취할 지를 정의할 수 있습니다. 

userSchema.pre('save', function(next) {
  if (!this.email) { // email 필드가 없으면 에러 표시 후 저장 취소
    throw '이메일이 없습니다';
  }
  if (!this.createdAt) { // createdAt 필드가 없으면 추가
    this.createdAt = new Date();
  }
  next();
});


// mongoose 5.x 에서는 next() 대신 async/await를 사용한다.
// in Node.js >= 7.6.0:
schema.pre('save', async function() {
  await doStuff();
  await doMoreStuff();
});



userSchema.post('find', function(result) {
  console.log('저장 완료', result);
});

```

<br>

<br>

## 3 ] Model

#### 1 ) Model / document 생성, 저장

```javascript

// Model 생성
const Todo = mongoose.model('Todo', todoSchema);


// document 생성
const todo = new Todo({
  todoid: 1,
  content: 'MongoDB',
  completed: false,
  ...
});

// or

const todo = new Todo();
todo.todoid = 1;
todo.content = 'MongoDB';
todo.completed = false;
...


// 저장
todo.save();

```

<br>

<br>

## 4 ] RESTful API

- 예제

```javascript
// [ models/todo.js ] 


const mongoose = require('mongoose');

// Define Schemes
const todoSchema = new mongoose.Schema({
  todoid: { type: Number, required: true, unique: true },
  content: { type: String, required: true },
  completed: { type: String, default: false }
},
{
  timestamps: true
});

// Create new todo document
todoSchema.statics.create = function (payload) {
  // this === Model
  const todo = new this(payload);
  // return Promise
  return todo.save();
};

// Find All
todoSchema.statics.findAll = function () {
  // return promise
  // V4부터 exec() 필요없음
  return this.find({});
};

// Find One by todoid
todoSchema.statics.findOneByTodoid = function (todoid) {
  return this.findOne({ todoid });
};

// Update by todoid
todoSchema.statics.updateByTodoid = function (todoid, payload) {
  // { new: true }: return the modified document rather than the original. defaults to false
  return this.findOneAndUpdate({ todoid }, payload, { new: true });
};

// Delete by todoid
todoSchema.statics.deleteByTodoid = function (todoid) {
  return this.remove({ todoid });
};

// Create Model & Export
module.exports = mongoose.model('Todo', todoSchema);


```

<br>

```javascript
// [ routes/todos.js ] 


const router = require('express').Router();
const Todo = require('../models/todo');

// Find All
router.get('/', (req, res) => {
  Todo.findAll()
    .then((todos) => {
      if (!todos.length) return res.status(404).send({ err: 'Todo not found' });
      res.send(`find successfully: ${todos}`);
    })
    .catch(err => res.status(500).send(err));
});

// Find One by todoid
router.get('/todoid/:todoid', (req, res) => {
  Todo.findOneByTodoid(req.params.todoid)
    .then((todo) => {
      if (!todo) return res.status(404).send({ err: 'Todo not found' });
      res.send(`findOne successfully: ${todo}`);
    })
    .catch(err => res.status(500).send(err));
});

// Create new todo document
router.post('/', (req, res) => {
  Todo.create(req.body)
    .then(todo => res.send(todo))
    .catch(err => res.status(500).send(err));
});

// Update by todoid
router.put('/todoid/:todoid', (req, res) => {
  Todo.updateByTodoid(req.params.todoid, req.body)
    .then(todo => res.send(todo))
    .catch(err => res.status(500).send(err));
});

// Delete by todoid
router.delete('/todoid/:todoid', (req, res) => {
  Todo.deleteByTodoid(req.params.todoid)
    .then(() => res.sendStatus(200))
    .catch(err => res.status(500).send(err));
});

module.exports = router;

```

<br>

<br>

## 5 ] app.js 예제

```javascript

// ENV
require('dotenv').config();
// DEPENDENCIES
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const port = process.env.PORT || 4500;

// Static File Service
app.use(express.static('public'));
// Body-parser
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Node의 native Promise 사용
mongoose.Promise = global.Promise;

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI, { useMongoClient: true })
  .then(() => console.log('Successfully connected to mongodb'))
  .catch(e => console.error(e));

// ROUTERS
app.use('/todos', require('./routes/todos'));

app.listen(port, () => console.log(`Server listening on port ${port}`));

```

<br>

<br>

## 6 ] Query

- 예제

```javascript

User.find()
  .where('name').equals('zerocho')
  .where('birth').gt(20)
  .where('role').in(['owner', 'admin'])
  .sort('-medals')
  .limit(5)
  .select('name birth medals')


```

<br>

### where

어떤 필드를 조작할지 정하는 쿼리입니다. 뒤에 equals, ne, exists, gt, ... 등의 쿼리를 사용하면 됩니다.

```jsx
Users.find().where('name')
```

### equals, ne

equals는 일치하는 것, ne는 일치하지 않는 것입니다. exists와 ne를 포함한 쿼리들은 앞에 where이 있으면 where에서 지정한 필드를 업데이트합니다. where이 없다면 쿼리에서 직접 필드를 지정할 수도 있습니다.

```jsx
Users.find().where('name').equals('zerocho')
Users.find().equals('name', 'zerocho') // 위와 동일한 쿼리
Users.find().ne('name', 'zerocho')
```

### exists

필드 존재 여부를 쿼리합니다.

```jsx
Users.find().where('name').exists(true)
Users.find().exists('name', true) // 위와 동일한 쿼리
```

### gt, lt, gte, lte

각각 초과(gt), 미만(lt), 이상(gte), 이하(lte)입니다.

```jsx
Users.find().where('age').gt(20)
Users.find().gt('age', 20);
```

### in, nin, all

in은 배열 안의 값 중 하나라도 일치하는지, nin은 하나도 일치 안 하는지, all은 모두 다 일치하는 지를 쿼리합니다.

```jsx
Users.find().where('role').in(['owner', 'admin'])
Users.find().in('role', ['owner', 'admin'])
```

### and, or, nor

얘네들은 where를 사용하지 않습니다! 이점 주의하세요. and는 모든 조건을 만족하는지, or는 일부 조건을 만족하는지, nor는 모두 다 만족하지 않는지를 쿼리합니다.

```jsx
Users.find().all([{ name: 'zerocho' }, { age: 24 }]); // 이름이 zerocho고 나이가 24면
Users.find().or([{ name: 'zerocho' }, { name: 'babo' }]); // 이름이 zerocho거나 babo면
```

### size, mod, slice

얘네들은 조금 특수한 쿼리들입니다. size는 배열 안의 요소 개수가 일치하는지, mod는 나머지가 일치하는지를 쿼리합니다. slice는 쿼리의 특정 부분만 가져옵니다. skip limit과 비슷합니다.

```jsx
Users.find().where('items').size(2) // items 필드의 배열 요소 개수가 2인지
```

### within, box, circle, geometry, near, intersects, maxDistance

몽고DB는 위치 정보 쿼리 기능이 강력한 것으로 유명하죠. where로 위치 인덱스가 설정된 필드를 지정하고, box, circle 등의 쿼리를 사용합니다.

```jsx
Users.find().where('loc').within().box()
Users.find().where('loc').within({ box: [[15, 30], [50, 60]] })
Users.find().where('loc').near([50, 60]).maxDistance(30)
```

### distinct

이 기능은 알아두시면 좋습니다. 지정한 필드의 값들에서 중복을 제거한 후 가져옵니다. 예를 들어 10명의 사람의 역할이 각각 user, user, admin, owner, admin, user, user, user, user, user... (헥헥) 이라면 중복을 제거하여 ['user', 'owner', 'admin']이 됩니다.

```jsx
Users.find().distinct('role') // ['user', 'owner', 'admin']
```

### regex

정규표현식에 일치하는 것을 가져오기 위한 메소드입니다.

```jsx
Users.find().where('name').regex(/zerocho/)
Users.find().regex('name', /zerocho/)
```

### select

프로젝션(특정 필드만 가져오는 기능)을 위한 메소드입니다. 예를 들어 name과 age만 선택하면 `{ _id: ..., name: ..., age: ... }` 이렇게 결과가 출력됩니다. _id는 빼지 않는 이상 기본 출력됩니다. 특정 필드를 빼고싶다면 앞에 -(빼기)를 붙이면 됩니다.

```jsx
Users.findOne().select('name age') // { _id: ..., name: ..., age: ... }
Users.findOne().select('-_id name age') // { name: ..., age: ... }
Users.findOne().select({ name: 1, age: 1, _id: 0 }) // 이렇게 객체 형식으로 해도 됩니다.
```

### skip, limit

이것은 자주 보셨을 겁니다. skip은 건너뛰기, limit은 개수 제한을 나타냅니다. limit은 0이면 0개를 가져오는 게 아니라 다 가져옵니다.

```jsx
Users.find().skip(10).limit(5) // 11~15번째 사람 쿼리
```

### sort

역시 자주 보셨을 겁니다. 정렬이죠.

```jsx
Users.find().sort('age -medals'); // 나이 오름차순 정렬, 메달 내림차순 정렬
Users.find().sort({ age: 1, medals: -1 }); // 이렇게도 가능합니다.
Users.find().sort({ age: 'asc', medals: 'desc' }); // 이렇게도...
Users.find().sort({ age: 'ascending', medals: 'descending' }); // 이렇게도...
```

### find, findOne, findOneAndRemove, findOneAndUpdate

얘네는 왜 쿼리에 있을까요? 모델의 메소드라고 생각하실 수도 있겠습니다만, 쿼리에서도 사용 가능합니다.

```jsx
Users.find({ name: 'zerocho' }).find({ age: 24 })
```

이렇게요. 첫 번째 find는 모델의 메소드이고 두 번째 find는 쿼리의 메소드입니다.

```jsx
Users.find().where('name').equals('zerocho').findOne()
```

이런 요상한 것도 가능합니다. 이렇게는 자주 안 쓰지만요.

<br>

<br>

## 7 ] populate

#### 1) 스키마 구성

```javascript

// ref에 해당 ObjectId가 속해있는 모델을 넣어줍니다. 현재는 자기 자신(User)을 가리키지만 다른 컬렉션 모델이어도 상관없습니다.

const userSchema = new mongoose.Schema({
  name: String,
  friends: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  bestFriend: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
});
mongoose.model('User', userSchema);

```

<br>

#### 2) populate

```javascript

User.findOne({ name: 'zero' }).populate('bestFriend').exec((err, data) => {
  console.log(data);
}); // 또는 populate({ path: 'bestFriend' })도 가능


//  bestFriend 부분의 ObjectId가 실제 객체로 치환되었습니다
{
  _id: { $oid: '574e9c0f9f663a1700fbe06e' },
  name: 'zero',
  friends: [{ $oid: '59a66f8372262500184b5363' }, { $oid: '574e8f46c9100617001c9cb9' }],
  bestFriend: { 
    _id: { $oid: '574e8f46c9100617001c9cb9' },
    name: 'hero',
    friends: [{ $oid: '59a66f8372262500184b5363' }, { $oid: '574e9c0f9f663a1700fbe06e' }],
    bestFriend: { $oid: '59a66f8372262500184b5363' }
  }
}


// bestFriend의 name과 bestFriend만 보고 싶다면
User.findOne({ name: 'zero' }).populate('bestFriend', 'name bestFriend').exec((err, data) => {
  console.log(data);
});

// 결과
{
  _id: { $oid: '574e9c0f9f663a1700fbe06e' },
  name: 'zero',
  friends: [{ $oid: '59a66f8372262500184b5363' }, { $oid: '574e8f46c9100617001c9cb9' }],
  bestFriend: {
    _id: { $oid: '574e8f46c9100617001c9cb9' },
    name: 'hero',
    bestFriend: { $oid: '59a66f8372262500184b5363' }
  }
}

```

<br>

<br>

## 8 ] aggregation

#### 1) 기본개념 : [https://docs.mongodb.com/manual/aggregation/#aggregation-framework](https://docs.mongodb.com/manual/aggregation/#aggregation-framework)

- MongoDB의 Aggregation은 Sharding 기반의 데이터를 효율적으로 처리하고 집계하는 프레임워크라고 이해하면 됨
- documents를 grouping, filtering 등 다양한 연산을 적용하여 계산된 결과를 반환

<br>

#### 2) 사용법

- 반환되는 문서는 몽구스 문서가 아닌 일반 자바 스크립트 객체입니다.

```javascript

const aggregate = Model.aggregate([
  { $project: { a: 1, b: 1 } },
  { $skip: 5 }
]);

// or

Model.
  aggregate([{ $match: { age: { $gte: 21 }}}]).
  unwind('tags').
  exec(callback);


```

<br>

#### 3) 명령어 [ mongoDB (SQL) ]

##### `$group (GROUP BY), $match (WHERE), $sort (ORDER BY), $sum (SUM)`

```javascript

// SQL: SELECT COUNT(*) AS count FROM zip
// mongodb aggreate:
// _id 필드는 mandatory, 하지만, 전체 doc에 대한 계산값이 필요할 때는 null 로 넣으면 됨
result = db.zip.aggregate([
    {'$group' : 
        {
            '_id' : 'null', 
            'count' : {'$sum' : 1}
        }
    } 
])


// SQL: SELECT state, SUM(pop) FROM zip GROUP BY state
// mongodb aggreate:
result = db.zip.aggregate([
    {'$group' : 
        {
            '_id' : '$state', 
            'total_pop' : {'$sum' : '$pop'}
        }
    } 
])


// SQL: SELECT state, SUM(pop) FROM zip GROUP BY state ORDER BY SUM(pop)
// mongodb aggreate:
result = db.zip.aggregate([
    {'$group' : 
        {
            '_id' : '$state', 
            'total_pop' : {'$sum' : '$pop'}
        }
    }, 
    {'$sort' : 
        {'total_pop': 1} 
    },
    {'$limit' : 5},
    {'$project' : { '_id' : 0 }}
])


// SQL: SELECT * FROM zip WHERE state = 'MA' 
// mongodb aggregate:
result = db.zip.aggregate([
    {'$match' : {'state' : 'MA'}}
])


// 내림차순 정렬하기
result = db.zip.aggregate([
             {'$group' : {'_id' : '$state', 'total_pop' : {'$sum' : '$pop'}}}, 
             {'$match' : {'total_pop' : {'$gte' : 10 * 1000 * 1000}}},
             {'$sort' : {'total_pop' : -1}}
])



// SQL: SELECT MAX(pop), MIN(pop) FROM zip GROUP BY state
// mongodb aggregate:
result = db.zip.aggregate([
    {'$group' : {'_id' : '$state', 'maximum' : {'$max' : '$pop'}, 'minimum' : {'$min' : '$pop'} }}
])


```

<br>

#### 4) 산술표현식, 날짜 표현식, 문자열 표현식

```javascript

* 산술표현식
 - $add       [a1 [, a2, a3 ... an]]
 - $substract [a1, a2]
 - $multiply  [a1 [, a2, a3 ... an]]
 - $divide    [a1, a2]
 - $mod       [a1, a2]

* 날짜 표현식
 - $year, $month, $week, $dayOfMonth, $dayOfWeek, $dayOfYear, $hour, $minute, $second

* 문자열 표현식
 - $substr
 - $concat

```

##### `산술표현식`

```javascript

result = db.zip.aggregate([
    {'$group' : 
        {
            '_id' : '$state', 
            'max' : {'$max' : '$pop'}, 
            'min' : {'$min' : '$pop'} 
        } 
    },
    {'$project' : 
        {
            'maxmin' : {'$add' : ['$max', '$min']} 
        } 
    }
])

// 결과
// {'_id': 'CA', 'maxmin': 99568}
// {'_id': 'MT', 'maxmin': 40128}

```

##### `날짜표현식`

```javascript

result = db.test_db.aggregate([
    {'$project' : 
        {
            '_id' : 0,
            'year' : { '$year': '$date' },
            'month' : { '$month': '$date' },
            'day' : { '$dayOfMonth': "$date" },
            'hour' : { '$hour': "$date" },
            'minutes' : { '$minute': "$date" },
            'seconds' : { '$second': "$date" },
            'milliseconds' : { '$millisecond': "$date" },
            'dayOfYear' : { '$dayOfYear': "$date" },
            'dayOfWeek' : { '$dayOfWeek': "$date" },
            'week': { '$week': "$date" }
        }
    }
])

// 결과
// {
//   'day': 20,
//   'dayOfWeek': 2,
//   'dayOfYear': 324,
//   'hour': 11,
//   'milliseconds': 971,
//   'minutes': 12,
//   'month': 11,
//   'seconds': 44,
//   'week': 47,
//   'year': 2017
// }

```

##### `문자열 표현식`

```javascript

// $concat operator 활용 예

db.test_db.insert_one(
    { 'firstname' : 'Dave', 'lastname' : 'Lee' }
)

result = db.test_db.aggregate([
    {'$project' : 
        {
            '_id' : 0,
            'all_data' : { '$concat' : ['Full Name: ', '$firstname', ' ', '$lastname' ]}
        }
    }
])

// 결과
// {'all_data': 'Full Name: Dave Lee'}



// $substr operator 활용 예

db.test_db.insert_one(
    { 'fullname' : 'Dave Lee' }
)

result = db.test_db.aggregate([
    {'$project' : 
        {
            '_id' : 0,
            'first_name' : { '$substr' : ['$fullname', 0, 4 ]},
            'last_name' : { '$substr' : ['$fullname', 5, 7 ]}
        }
    }
])

// 결과
// {'first_name': 'Dave', 'last_name': 'Lee'}

```

<br>