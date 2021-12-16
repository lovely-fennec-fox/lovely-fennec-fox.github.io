---
layout: post
title:  "Django_ DB관리 프로그램"
author: fennec-fox
tags: Django
subtitle: "Ask django / 장고 Admin, masql workbench"
category:  blog


---

### Django [Admin] 내용 정리

<br>

- `'/admin/'` 주소는 노출이 쉬우니 다른 주소로 변경하는 것을 권장
- 모델 클래스 등록을 통해, 조회/ 추가/ 수정/ 삭제 웹 UI를 제공한다. 

<br>

# Django에서 Admin 정의하는 방법

```python

from django.contrib import admin
from .models import Item # 모델이름

# 방법1
admin.site.register(Item)


# 방법2
class ItemAdmin(admin.ModelAdmin):
    list_display = ('publisher', 'title', 'content')
    
admin.site.register(Item)    


# 방법3
@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):

```

<br>

# masql workbench

장고 Admin보다 빠르고, 유용한 기능들을 추가해서 가지고 있음

[MySQL](https://dev.mysql.com/) 에서 다운받아 사용하면된다.

### ` 참고 `

- db파일변환하기 => [https://www.rebasedata.com/](https://www.rebasedata.com/)

