---
layout: post
title:  "Django_ Messages_Framework"
author: fennec-fox
tags: Django
subtitle: "Ask django / Messages_Framework"
category:  blog



---

### Django [Messages_Framework] 내용 정리

- form 을 만든 후, User를 위한 1회성 메세지를 담는 용도

  ex> '저장했습니다', '로그인 되었습니다.'

<br>

- message levels를 통한 메시지 분류

1. INFO : 해당 유저에 대한 정보성 메세지
2. SUCCESS : 액션에 성공적으로 수행되었음을 알림
3. WARNING : 실패가 아직 발생하진 않았지만 임박했다.
4. ERROR : 액션이 수행되지 않았거나, 다른 Failure가 발생했다. 

<br>

# 사용방법

1. messages 등록코드

```python

# [views.py]

from django.contrib import messages

def post_new(request):
  if form.is_valid():
    post = form.save()
    messages.add_message(request, messages.SUCCESS, '새글등록')
    # shortcut 형태
    messages.success(request, '새글등록') 

  return redirect(success_url)



# [templates.html]
{% raw %}
{% if messages %}
  <ul class='messages'>
    {% for message in messages %}
      <li>                   
        [{{ message.tags }}]{{ message.message }} # {{message}} 라고만 써도 됨
      </li>
    {% endfor %}
  </ul>  
{% endraw %}

```

<br>

2. Bootstrap4 alert HTML 마크업

```python

<div class='alert alert-info'></div>  # 정보
<div class='alert alert-success'><div> # 성공시
<div class='alert alert-warning'><dif> # 실패임박



# tmeplates에서 사용법 

{% raw %}
{% if messages %}
  {% for message in messages %}
    <div class='alert alert-{{ message.tag }}'>
      {{ message.message}}
    </div>
  {% endfor %}
{% endraw %}

```

<br>

# Message_tags 수정

```python

# [ settings.py ]

from django.contrib.messages import constants
MESSAGE_TAGS = {
  constants.DEBUG: 'secondary',
  constants.ERROR: 'danger'
}

```

<br>