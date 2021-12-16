---

layout: post
title:  "CBV_ TemplateView"
author: fennec-fox
tags: Django
subtitle: "Django / CBV_ generic_ TemplateView"
category:  blog

---

### Django [CBV_ generic_ TemplateView] 내용 정리

<br>

### 공부방법

- view 는 기능이 너무 많으니 as_view , dispatch 만 먼저 확인하기

<br>

# TemplateView_ 사용법

```python

# [ views.py ] 

from django.views.generic import TemplateView

class AboutView(TemplateView):
    template_name = "about.html"
    
```

```python

# [ urls.py ] 

from django.urls import path
from some_app.views import AboutView

urlpatterns = [
    path('about/', AboutView.as_view()),
]

```

class django.views.generic.base.``TemplateView``

URL로 캡춰 된 파라미터를 포함한 문맥을 사용해, 지정된 템플릿을 렌더링합니다.

코드를 작성해보자.

<br>

1. 템플릿 작성

```python

{% raw %}

<body>

Welcome, my homepage!

</body>

{% endraw %}

```

<br>

2. views.py에 Class 생성하고 TemplateView를 상속받는다.

   보통 클래스 뷰의 이름은 대문자로 시작하고 끝에 View가 붙는다 (IndexView, DetailView, ResultsView 등)

```python

from django.views.generic.base import TemplateView

class HomePageView(TemplateView):

# a template_name를 정의하지 않으면 예외 발생  
    template_name = "test.html" 

```

<br>

3. urls.py 를 작성한다. 

```python

from django.urls import path    
from polls.views import HomePageView   # 앱 이름

 
# .as_view() 를 통해서 generic view를 적용할 수 있다.
urlpatterns = [
    path('homepage', HomePageView.as_view()),  
]

```

<br>

4. 사용가능한 설정 변수들

```python

class HomePageView(TemplateView):

  # 템플릿 이름
  template_name = "test.html" 

  # context 추가
  extra_context = {'title': 'hello'}


```

<br>

# TemplateView_ 자세히 알아보자

```python

class TemplateView(TemplateResponseMixin, ContextMixin, View):
    """
    Render a template. Pass keyword arguments from the URLconf to the context.
    """
    def get(self, request, *args, **kwargs):
        context = self.get_context_data(**kwargs)
        return self.render_to_response(context)

```

- `get함수` 활용법 : context 추가할 때 사용함

```python

class Test(TemplateView):
  template_name = 'test.html'

  def get(self, request, *args, **kwargs):
    
    # 주소에서 정보 받아올 때
    context = request.GET
    
    # context 지정
    context = {'test':'abcd'}
    
    return self.render_to_response(context)

```

**Ancestors** **(MRO)**

이 뷰는 다음 뷰에서 메서드와 특성을 상속받습니다.

- [`django.views.generic.base.TemplateResponseMixin`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/mixins-simple/#django.views.generic.base.TemplateResponseMixin)
- [`django.views.generic.base.ContextMixin`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/mixins-simple/#django.views.generic.base.ContextMixin)
- [`django.views.generic.base.View`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/base/#django.views.generic.base.View)

**메소드 흐름도**

1. [`setup()`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/base/#django.views.generic.base.View.setup)
2. [`dispatch()`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch)
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed)
4. [`get_context_data()`](https://docs.djangoproject.com/en/2.2/ref/class-based-views/mixins-simple/#django.views.generic.base.ContextMixin.get_context_data)

<br>

# 1. TemplateResponseMixin 

```python

class TemplateResponseMixin:
""" 템플릿을 렌더링 하는데 사용한다."""
    
# 문자열로 정의 된대로 사용할 템플릿의 전체 이름
    template_name = None
# Default는 Django가 모든 구성된 엔진에서 템플릿을 검색하도록 지시  
    template_engine = None
# render_to_response메소드에 의해 리턴 될 응답 클래스 
    response_class = TemplateResponse
# 응답에 사용할 컨텐트 유형
    content_type = None


```

<br>

# 2. ContextMixin

```python

class ContextMixin:
"""
get_context_data()의 리턴 값과 기본 값과 혼합.
"""
# 컨텍스트에 포함 할 dict / ex> {'title': 'Custom Title'}
    extra_context = None


```

<br>

# 3. View

```python

class View:
"""모든 views의 상위 클래스"""
  http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']

    def __init__(self, **kwargs):
        for key, value in kwargs.items():
            setattr(self, key, value)              

    @classonlymethod
    def as_view(cls, **initkwargs):
"""요청 응답 프로세스의 기본 진입점."""
        for key in initkwargs:
            if key in cls.http_method_names:
                raise TypeError("..에러"% (key, cls.__name__))
            if not hasattr(cls, key):
                raise TypeError("..에러"% (cls.__name__, key))


# request 인수를 받아들이고 HTTP 응답을 반환하는 메서드                
    def dispatch(self, request, *args, **kwargs):
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)

```

<br>
