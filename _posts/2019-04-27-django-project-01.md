---
layout: post
title:  "project1_ smartstore project"
author: fennec-fox
tags: Projects
subtitle: "project"
category:  blog


---

### 장고를 사용해서 만든 첫번째 프로그램 중요 내용 정리

{:toc}

<br>

# 만들 프로그램

Smartstore 순위관리프로그램, 공부하면서 따라서 만들어 봄

- [스토어팜 순위관리프로그램](<https://www.youtube.com/watch?v=eN3suuk8nQc>)

<br>

# 만든 프로그램

- [GitHub](<https://github.com/lovely-fennec-fox/smartstore_ranking>) 

<br>

# setting

- [개발환경 / install](https://everyday-00.tistory.com/entry/store-Django를-이용한-첫-프로그램-만들기-소개?category=823107)

- [장고 기본 setting](https://everyday-00.tistory.com/entry/Django-개요-및-설치?category=821528)
- [모델(DB)만들기](https://everyday-00.tistory.com/entry/store-model?category=823107)

<br>

# 기초내용 정리

- [Beautiful soup](https://lovely-fennec-fox.github.io//blog/2019/04/03/python_16/)

- [requests](https://everyday-00.tistory.com/entry/Python-Library-requests?category=822996)

- [templatetags](https://everyday-00.tistory.com/entry/Django-Templates?category=821528)

- [Try/except](https://lovely-fennec-fox.github.io//blog/2019/03/27/python_3/)

- [json](https://lovely-fennec-fox.github.io//blog/2019/04/04/python_18/)



<br>

# 서버에서 비동기로 Data(json) 받아올 때

### JavaScript_ fetch 활용('csrf_token'추가하기)

1. 비동기란?

   동기는 말 그대로 동시에 일어난다는 뜻을 가지고, 요청과 그 결과가 동시에 일어난다는 약속입니다.

   비동기는 동시에 일어나지 않는다는 것을 의미합니다. 요청과 결과가 동시에 일어나지 않을거라는 약속입니다.

2. 비동기의 장단점

   동기방식은 설계가 매우 간단하고 직관적이지만, 결과가 주어질 때까지 아무것도 못하고 대기해야한다는 단점이 있다.

   비동기방식은 동기보다 복잡하지만 결과가 주어지는데 시간이 걸리면, 그 시간동안 다른 작업을 할 수 있으므로 자원을 효율적으로 사용할 수 있다.

3. FormData를 만들어 csrf_token 값을 넣어서 서버에 data요청

```javascript

var myList = document.querySelector('ul');

function requests(){
  
  const data = new FormData();
  data.append('csrfmiddlewaretoken', csrf_token값)
  
  var Init = {
    method: 'POST',
    body: data
  }
  
  fetch('url', Init)
    .then(function(response){
      return response.json(); 
    })
    .then(function(data){ 
      for (var i = 0; i data.products.length; i++) {
        var listItem = document.createElement('li');
        listItem.innerHTML = '<strong>' + data.products[i].name + '</strong>';
      myList.appendChild(listItem);
      }
    });
  }

```

- [Django 공식문서에서의 방법]([¶](https://docs.djangoproject.com/en/2.1/ref/csrf/#ajax))

<br>

# 장고ORM_queryset

1. queryset 이란?

   내부적으로 queryset은 실제로 데이터베이스에 접근하지 않고, 구성, 필터링, 슬라이스를 진행한다. queryset에 저장할 때까지는 실제 데이터베이스 에서는 작업이 수행되지 않는다. 

2. Query set API 

- [Query set 기초 정리](https://everyday-00.tistory.com/entry/Django-setting-models?category=821528)

- [Query set API](https://docs.djangoproject.com/en/2.2/ref/models/querysets/#queryset-api-reference)

<br>

<br>

