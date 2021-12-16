---
layout: post
title:  "project2_ Customizing User Model 02.LoginView, LogoutView 이용하기"
author: fennec-fox
tags: Projects
subtitle: "Django / LoginView, LogoutView 구현하기 "
category:  blog

---

### Django_Projects2 [ LoginView, LogoutView 구현하기] 과정 내용 정리

<br>

# 1. 로그인 페이지 구현

1) 로그인 버튼을 누르면 로그인 페이지로 이동

2) 로그인이 성공하면 메인페이지로 이동

3) navigation의 '로그인'이 '로그아웃' 으로 바뀜

<br>

# 1-1 Login template 만들기

장고의 시스템을 그대로 이용하려면, 

`myapp`의 `templates` 아래 `registration` 폴더를 만들어 `login.html` 파일을 만들어 form을 만들어준다. 

```html

<!-- tmeplates/ registration/ login.html -->
{% raw %}

<form method="post">
  {% csrf_token %}
  <h2>Log In</h2>

    <!-- user name -->
    <span class="input-item">Email</span>

    <!-- user name Input-->
      {{ form.username }}

    <!-- Password -->
    <span class="input-item">Password</span>

    <!-- Password Input-->
      {{ form.password }}

  <!-- button LogIn -->
  <button class="log-in" type='submits'> Log In </button>

</form>

{% endraw %}
```

<br>

# 1-2. Urls 설정

```python

# [Urls.py]

urlpatterns = [
    path('accounts/', include('django.contrib.auth.urls')),
]

```

<br>

위와 같이 설정 후,

`accounts/login/` 으로 호출하면 정상작동 됨

- 장고 URL패턴

```python

accounts/login/ [name='login']
accounts/logout/ [name='logout']
accounts/password_change/ [name='password_change']
accounts/password_change/done/ [name='password_change_done']
accounts/password_reset/ [name='password_reset']
accounts/password_reset/done/ [name='password_reset_done']
accounts/reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/reset/done/ [name='password_reset_complete']

```

<br>

# 2. 로그아웃 페이지 구현

1) 로그아웃 버튼을 누르면 메인페이로 이동

2) navigation의 '로그아웃'이 '로그인' 으로 바뀜

<br>

- Logout사용법

```python

from django.contrib.auth import logout

def logout_view(request):
    logout(request)
    
    return HttpResponseRedirect('/user/main/') 
    # Redirect to a success page.

```

<br>

# 3. Login, Logout 쉽게 구현하기

```python

from django.contrib.auth import views as auth_views

urlpatterns = [
url('logout/', auth_views.logout, {'next_page' : '/'}),
url('login/', auth_views.login,  {'template_name':'memo_app/login.html'}),
]

```

위와 같이 url만으로도 구현 가능

# 4. Django LoginView 살펴보기

<br>

- `Url이름` : 'login'

<br>

- `속성` : [ default 값 ] - 기능

- `template_name`: [ registration/login.html ] - 로그인 페이지 템플릿 
- `redirect_field_name` : [ REDIRECT_FIELD_NAME = 'next'] - GET 로그인 후 리디렉션 할 URL이 포함 된 필드 의 이름
- `extra_context` : [ None ] - 기본 컨텍스트 데이터에 추가되는 데이터 사전
- `authentication_form` : [ None ] - 인증에 사용할 class 지정
- `redirect_authenticated_use` : [ false ] - 재로그인을 하는 것을 제어(어디에 사용되는지 조금 더 공부해야 함)
- `success_url_allowed_hosts` : [ set() ] - 로그인 성공시 리디렉션 되는 host. 지정하지 않으면  [`settings.LOGIN_REDIRECT_URL`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-LOGIN_REDIRECT_URL) 로 제공됨

<br>

# 6. Django LogoutView 살펴보기

<br>

- `Url이름` : 'logout'

<br>

- `속성` : [ default 값 ] - 기능
- `next_page`: [ None ] - 로그아웃 후 리디렉션 되는 URL 지정하지 않으면 [`settings.LOGOUT_REDIRECT_URL`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-LOGOUT_REDIRECT_URL) 로 제공됨
- `template_name`: [ registration/logged_out.html] - 로그아웃 템플릿
- `logout_then_login(request, login_url=None)` : 사용자를 로그아웃한 다음 로그인 페이지로 리디렉션

<br>