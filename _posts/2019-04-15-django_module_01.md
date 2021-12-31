---

layout: post
title:  "Django_Module_Paginator"
author: fennec-fox
tags: Django
subtitle: "Django / Module_Paginator"
category:  django

---

### Django [Module_Paginator] 내용 정리

<br>

# 사용법

```python

from django.core.paginator import Paginator

>>> objects = ['john', 'paul', 'george', 'ringo']

>>> p = Paginator(objects, 2)    # objects를 2개씩 보여줌 

```

+ Paginator.get_page() : Returns a Page object with the given 1-based index
+ Paginator.count : The total number of objects
+ Paginator.num_pages : The total number of pages.
+  Paginator.page_range : range iterator of page numbers
+  Paginator.page() : class Page(object_list, number, paginator)

<br>

```python

# Paginator.get_page()
>>> p.get_page(1)
<Page 1 of 2>

# Paginator.count
>>> p.count      
4
# Paginator.num_pages
>>> p.num_pages
2

# Paginator.page_range 
>>> type(p.page_range)
<class 'range_iterator'>
>>> p.page_range
range(1, 3)

# Paginator.page()
>>> page1 = p.page(1)
>>> page1
<Page 1 of 2>
>>> page2 = p.page(2)

```

- Page.object_list : The list of objects on this page

- Page.has_next(): Returns True if there’s a next page.

- Page.has_previous() : Returns True if there’s a previous page.

-  Page.has_other_pages() : Returns True if there’s a next or previous page.

```python

# Page.object_list
>>> page1.object_list
['john', 'paul']
>>> page2.object_list
['george', 'ringo']

# Page.has_next()
>>> page2.has_next()
False

# Page.has_previous()
>>> page2.has_previous()
True

# Page.has_other_pages()
>>> page2.has_other_pages()
True

```

- Page.next_page_number(): Returns the next page number. Raises InvalidPage if next page doesn’t exist.

- Page.previous_page_number() : Returns the previous page number. Raises InvalidPage if previous page doesn’t exist.

- Page.start_index() : The 1-based index of the first item on this page

- Page.end_index() : # The 1-based index of the last item on this page

```python

# Page.next_page_number()
>>> page2.next_page_number()
Traceback (most recent call last):
...
EmptyPage: That page contains no results
  
# Page.previous_page_number() 
>>> page2.previous_page_number()
1

# Page.start_index()
>>> page2.start_index() 
3

# Page.end_index()
>>> page2.end_index() 
4

```

<br>

# 예제

```python

# [ views.py ]  
from django.core.paginator import Paginator
from django.shortcuts import render

def listing(request):
    contact_list = Contacts.objects.all()
    paginator = Paginator(contact_list, 25) # Show 25 contacts per page

    page = request.GET.get('page')
    contacts = paginator.get_page(page)
    return render(request, 'list.html', {'contacts': contacts})

```

```python

# templates

{% raw %}

{% for contact in contacts %}
    {# Each "contact" is a Contact model object. #}
    {{contact.full_name|upper}}<br>
    ...
{% endfor %}

<div class="pagination">
    <span class="step-links">
       {% if contacts.has_previous %}
            <a href="?page=1">&laquo; first</a>
            <a href="?page={{contacts.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>

        {% if contacts.has_next %}
            <a href="?page={{contacts.next_page_number }}">next</a>
            <a href="?page={{contacts.paginator.num_pages }}">last &raquo;</a>
        {% endif %}
    </span>
</div>

{% endraw %}


```

<br>

