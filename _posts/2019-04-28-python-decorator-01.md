---
layout: post
title:  "python_ decorator"
author: fennec-fox
tags: python
subtitle: "함수형 decorator/ 클래스형 decorator"
category:  python
comments: true
---

### Python [Decorator] 내용 정리

<br>

# 함수형 데코레이터

- 대상함수를 wrapping 하고, 이 wrapping 된 함수의 앞뒤에 추가적으로 꾸며질 구문들을 정의해서 손쉽게 재사용 가능하게 해준다.

  쉽게 설명하면, 메인 구문이 있고 여기에 부가적인 구문을 추가하고 싶은데 부가적인 구문을 반복해서 사용하고 싶은 경우 사용한다.

<br>

```python

from datetime import date

def deco(func):
  today = date.today()
  print(today)
  func()
  return func


def A(func):
  print('a')
  return func



@A
@deco
def B():
  return 'b'



# deco, A, B 순서로 실행된다.  
>>> print(B())
2019-04-29
a
b

```

<br>

# 클래스형 데코레이터

- 클래스는 함수처럼 호출 할 수 없기 때문에 `__call__` 함수를 꼭 추가해주어야 한다.

### `__call__` 함수가 없으면 에러가 난다

```python

class Hello():

  def __init__(self):
    pass


>>> hello = Hello()
>>> print(hello())

TypeError: 'Hello' object is not callable

```

<br>

```python

class Hello():

  def __init__(self):
    pass

  def __call__(self):
    return 'hello'


>>> hello = Hello()
>>> print(hello())

hello

```

<br>

- 클래스 데코레이션을 만들어보자

```python

class Hello():

  def __init__(self, func):
    self.func = func

  # wrapper 함수의 역할을 함
  def __call__(self, *args):
    return 'hello, '+ self.func(*args)


@Hello
def print_name(name):
  return name


>>> print(print_name('AA'))
hello, AA

```

1. 함수가 실행이 되면, 자동으로 Hello의 객체가 생성된다
2. `__init__` 함수의 parameter로 함수 자신이 들어간다.
3. `__call__` 함수가 실행되어 최종결과가 출력된다.

<br>

- 다음과 같은 예제 함수를 만들 수 있다.[출처:jupiny](https://jupiny.com/2016/09/25/decorator-class/)

```python

class Tagify():

    def __init__(self, function):
        self.function = function

    def __call__(self, *args, **kwargs):
        tagified_p = self.tagify('p', self.function(*args, **kwargs))
        tagified_b = self.tagify('b', tagified_p)
        tagified_i = self.tagify('i', tagified_b)
        return tagified_i

    def tagify(self, tag, text):
        return "<{tag}>{text}</{tag}>".format(tag=tag, text=text)


@Tagify
def set_text(text):  
    return text

set_text('python') # <i><b><p>python</p></b></i>  

```
