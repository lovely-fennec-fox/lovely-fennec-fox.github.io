### Django [CBV_ generic_ TemplateView_function] 내용 정리

<br>

# 1. TemplateView

```python

class HelloView(TemplateView):

    template_name = 'polls/hello_view.html'

    def get(self, request, *args, **kwargs):
        context = self.get_context_data(**kwargs)
        return self.render_to_response(context)

```

- `get_context_data()` —> ContextMixin `context를 지정한 후`
- `render_to_response()` —> TemplateResponseMixin `return a response`

# 2. ContexMixin

```python

class ContextMixin:

  extra_context = None

  def get_context_data(self, **kwargs):
    kwargs.setdefault('view', self)
    # {'view': <__main__.ContextMixin object at 0x7fc8a3d8b208>} 
    if self.extra_context is not None:
      kwargs.update(self.extra_context)
    # {'view': <__main__.ContextMixin object at 0x7fc8a3d8b208>, extra_context}    
    return kwargs

```

# 3. TemplateResponseMixin

```python

class TemplateResponseMixin:
  template_name = None
  template_engine = None
  response_class = TemplateResponse
  content_type = None

    # render_to_response({'view': <__main__.ContextMixin object at 0x7fc8a3d8b208>, extra_context}) 호출   
  def render_to_response(self, context, **response_kwargs): 
    # content_type 추가
      response_kwargs.setdefault('content_type', self.content_type)
    # context 추가   
      return self.response_class(
        request=self.request,
        template=self.get_template_names(),
        context=context,
        using=self.template_engine,
        **response_kwargs
      )
    # <__main__.TemplateResponse object at 0x7fc8a3d9ae48> 반환

```

# 4. View

```python

class View:
    http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']

    def __init__(self, **kwargs):
        for key, value in kwargs.items():
            setattr(self, key, value)

    @classonlymethod
    def as_view(cls, **initkwargs):
        for key in initkwargs:
            if key in cls.http_method_names:
                raise TypeError("You tried to pass in the %s method name as a ""keyword argument to %s(). Don't do that." % (key, cls.__name__))
            if not hasattr(cls, key):
                raise TypeError("%s() received an invalid keyword %r. as_view ""only accepts arguments that are already ""attributes of the class." % (cls.__name__, key))

        def view(request, *args, **kwargs):
            self = cls(**initkwargs)
            if hasattr(self, 'get') and not hasattr(self, 'head'):
            self.head = self.get
            self.request = request
            self.args = args
            self.kwargs = kwargs
            return self.dispatch(request, *args, **kwargs)
        view.view_class = cls
        view.view_initkwargs = initkwargs
        update_wrapper(view, cls, updated=())
        update_wrapper(view, cls.dispatch, assigned=())
        return view

# 요청 인수와 인수를 수락하고 HTTP 응답을 반환함
# HTTP method를 검사하고 매치되는 method로 GET는 get()를, POST는 post()를 하도록 위임될 것이다.  
    def dispatch(self, request, *args, **kwargs):
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)

```

