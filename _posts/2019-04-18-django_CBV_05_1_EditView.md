---

layout: post
title:  "CBV_ CreateView"
author: fennec-fox
tags: Django
subtitle: "Django / CBV_ CreateView"
category:  blog

---

### Django [CBV_ generic_ Formviews] 내용 정리

<br>

# CreateView

- 활용법

```python

# [myapp / views.py] 

from django.views.generic.edit import CreateView
from myapp.models import Author

class AuthorCreate(CreateView):
    model = Author
    fields = ['name']
    template_name = 'author_form.html'
    
    
# [myapp / author_form.html]

{% raw %}
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Save">
</form>
{% endraw %}

```



# FormView와 CreateView가 다른점 

- `model의 인스턴스 값을 저장하고 ` 반환함

```python

class ModelFormMixin(FormMixin, SingleObjectMixin):
  
  def form_valid(self, form):
    """If the form is valid, save the associated model."""
    self.object = form.save()
    return super().form_valid(form)

```