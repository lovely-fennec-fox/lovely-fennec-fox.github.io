---
layout: post
title:  "project2_ Customizing User Model 03.SignUpForm, ChangeUserPassword"
author: fennec-fox
tags: Projects
subtitle: "Django / SingUp구현하기, user 정보 수정하기 "
category:  project


---

### Django_Projects2 [ SignUp구현하기, user 정보 수정하기] 과정 내용 정리

<br>

# 1. 가입 페이지 구현하기

1)  가입버튼을 누르면 가입하기 페이지로 이동

2) 가입하기 페이지에서 nickname, email, password를 작성한 후 유효하면 Database에 저장되고 로그인 페이지로 이동

<br>

# 1-1 django 모델폼 수정하기

```python

# [forms.py]

from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from user.models import MyUser

# UserCreationForm 상속받아서 사용할 fields 추가시켜줌
class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm):
        model = MyUser
        fields = ('nickname', 'email')

# UserChangeForm 상속받아서 사용할 fields 추가시켜줌
class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = MyUser
        fields = ('nickname', 'email')

```

<br>

# 1-2 모델폼으로 CBV 생성

```python

# [views.py]

class SignUp(CreateView):
  	# 수정한 모델폼 지정 
    form_class = CustomUserCreationForm
    # 로그인 성공시 이동한 주소 이름 지정
    success_url = reverse_lazy('login')
    # 사용할 template 지정
    template_name = 'sign_up.html'

```

<br>

# 1-3 Url 설정

```python

    path('signup/', views.SignUp.as_view(), name='signup'),

```

<br>

