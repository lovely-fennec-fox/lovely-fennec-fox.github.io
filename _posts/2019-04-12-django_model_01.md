---

layout: post
title:  "Django_ 모델, 모델필드, 필드옵션"
author: fennec-fox
tags: Django
subtitle: "Django / model, field, option"
category:  django

---

### Django [models] 내용 정리

<br>

**Django Model**

- 장고 내장 ORM

- ORM의 역할 : SQL 을 직접 작성하지 않아도 장고 모델을 통해 데이터베이스로 접근한다.(조회/추가/수정/삭제)

  <br>

  **장고 커스텀 모델정의**

- 위치: 앱 / models.py
- Database 테이블 구조와 타입을 먼저 설계를 한 다음에 모델을 정의한다.
- 모델 클래스명은 단수형을 사용한다.(Persons 아니고,  Person)

<br>

# models.py_ 사용법

1. app 을 생성한 후, models.py 에 작성하기

2. Models.py 에 코드 작성 후, [`manage.py makemigrations`] 으로 테이블을 생성한 후  [`manage.py migrate`] 로 run 한다. 

3. Admin.py 에 모델클래스 등록

   ```python
   
   # 앱폴더/admin.py
   from django.contrib import admin
   from .models import Person
   
   admin.site.register(Person)
   
   ```

<br>

# 모델을 만들어보자.

```python

# 앱폴더/models.py
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

```

위의 코드는 아래의 database table 과 같다.

```sqlite

CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);

```

### 생성된 DB테이블 명

- 디폴트 값 : '앱이름_모델명'

  ex> blog 앱 / Person 모델

    ———>  "blog_person"

<br>

# Model field + Option

- Djanog all Fields => [Model field reference](https://docs.djangoproject.com/en/2.1/ref/models/fields/#module-django.db.models.fields)

<br>

### 많이 사용하는 Fields

1. 문자열 : CharField
2. 날짜/시간 : DateTimeField
3. 참/거짓 : BooleanField
4. 숫자 : IntegerField
5. 파일 : FileField, ImageField
6. 이메일 : EmailField
7. URL : URLField

### 많이 사용하는 필드 공통 Option

0. 옵션명[디폴트 값] : 기능

1. `blank` [False] : empty 허용여부(null과 다름, null은 DB의 값에 관련됨 blank는 유효성 검사시 빈 값을 허용해주는 것을 말함.)
2. `null`  [False] : null 허용여부 
3. `default` [False] : 디폴트 값 지정
4. `unique` [False] : 현재 테이블 내에서 유일성 여부
5. `choices` : selectbox 소스로 사용 => [사용법](https://docs.djangoproject.com/en/2.1/ref/models/fields/#django.db.models.Field.choices)
6. `validators` : validators를 수행할 함수를 다수 지정 

```python

from django.db import models
   
class Musician(models.Model):
   
    # unique
    name = models.CharField(max_length=50, unique=True)
    # default
    instrument = models.CharField(max_length=100, default='-')

class Album(models.Model):
    # foreignkey 생성시, Musician을 인스턴스로 넣어야 함
    
    # on_delete: 부모db 삭제시 어떻게 할지 정해줌 
    # related_name: Musician.albums.objects로 접근 가능하게 함
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE, related_name='albums')
    
    # null
    name = models.CharField(max_length=100, null=False)
    
    # auto_now_add: 해당 레코드 생성시 현재 시간 자동저장
    release_date = models.DateTimeField(auto_now_add=True)
    
    # auto_now: 해당 레코드 갱신시 현재 시간 자동저장
    updated_at = models.DateTimeField(auto_now=True) 
    
    # IntegerField: 숫자
    num_stars = models.IntegerField()    

```

<br>

# Option 활용법

###   + `choices` : `get_[변수명]_display() ` 으로 접근

```python

from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)

```

```bash

>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display() 
'Large'

```

<br>

### + `Primary_key` : 읽기만 가능해서 값을 변경한 다음 저장하면   이전 개체와 함께 새 개체가 만들어집니다.

```python

from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)

```

```bash

>>> fruit = Fruit.objects.create(name='Apple')
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True)
<QuerySet ['Apple', 'Pear']>

```

<br>

### + `default` : 객체도 default 값이 될 수 있음

```python

def contact_default():
    return {"email": "to1@example.com"}

contact_info = JSONField("ContactInfo", default=contact_default)

```

<br>

# class Meta option

- Djanog meta option => [Model meta option](https://docs.djangoproject.com/ko/2.1/ref/models/options/#model-meta-options)

<br>

1. `abstract` : 공통된 필드가 많이 있는 모델 클래스들이 있을 때 물리적으로 존재하지 않는 가상의 클래스가 된다

   참고사이트 => [개발새발로그](https://dgkim5360.tistory.com/entry/django-model-inheritance) 

```python


class Vehicle(models.Model):
  name = models.CharField(max_length=100)
 
  # 다음의 옵션이 있으면, Vehicle은 물리적으로 생성되지 않음
  class Meta:
    abstract = True
    
class Car(Vehicle): 
  type = models.CharField(max_length=50)

```

2. `app_label` : app_name 을 지정해주는 이유는 django가 기본적으로 model을 생성할 때 class가 존재하는 바로 위의 파일을 앱으로 판단하여 거기에 모델을 생성하기에 모델이 앱의 외부에 정의 된 경우에는 앱의 이름을 넣어주어야 함.

3. `db_table` : 모델에서 사용할 데이터베이스의 테이블 이름

   디폴트 값 -> "app label(사용 된 이름)_모델의 클래스 이름"

4. `ordering` : 정렬

```python

from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
      # horn_length를 기준으로 오름차순으로 정렬/ '-'를 사용하면 내림차순  
        ordering = ["horn_length"]

```

5. `proxy` : 서브 클래스 모델로 처리됨

```python

from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        # ...
        pass

```

`MyPerson` 클래스는 부모 클래스인 `Person` 처럼 동일한 데이타베이스 테이블에 작동 됩니다. 또한 특별히, 어느 새로운 `Person` 의 인스턴스는 [``](https://docs.djangoproject.com/ko/2.1/topics/db/models/#id1)MyPerson``을 통해 접근 가능하고 기타 여러가지 기능이 가능하게 됩니다

```python

>>> p = Person.objects.create(first_name="foobar")
>>> MyPerson.objects.get(first_name="foobar")
<MyPerson: foobar>

```

6. `unique_together` : 고유값으로 묶여야하는 값

```python

unique_together = (("driver", "restaurant"),)

```

<br>