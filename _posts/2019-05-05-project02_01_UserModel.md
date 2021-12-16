---
layout: post
title:  "project2_ Customizing User Model 01.user model custom"
author: fennec-fox
tags: Projects
subtitle: "Django / user 모델만들기, 활용하기"
category:  blog
---

### Django_Projects2 [ User model custom ] 과정 내용 정리

<br>

# 원하는 User model

1) 로그인시 ID와 password 받음  

2) 회원의 ID를 Email로 받기

3) 회원에게 받을 정보 : nickname, email, password 

# 1. Customizing User Model

1)  username을 삭제하고, email을 기본 field로 지정한 User model 

- 참고
  - django 공식문서 => [Customizing authentication in Django](https://docs.djangoproject.com/en/2.2/topics/auth/customizing/#a-full-example)
  - [testdriven.io](https://testdriven.io/blog/django-custom-user-model/)
  - 초보몽키의 개발공부로그 [사용자 인증](https://wayhome25.github.io/django/2017/05/18/django-auth/)

```python

class MyUserManager(BaseUserManager):
    use_in_migrations = True

    def _create_user(self, email, password, **extra_fields):
        if not email:
            raise ValueError('The given username must be set')
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_user(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', False)
        extra_fields.setdefault('is_superuser', False)
        return self._create_user(email, password, **extra_fields)

    def create_superuser(self, email, password, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self._create_user(email, password, **extra_fields)


class MyUser(AbstractBaseUser, PermissionsMixin):
    nickname_validator = UnicodeUsernameValidator()

    email = models.EmailField(
        verbose_name='email address',
        max_length=255,
        unique=True,
    )
    nickname = models.CharField(
        max_length=150,
        validators=[nickname_validator],
        default='admin',
        error_messages={
            'unique': _("A user with that username already exists."),
        },
    )
    date_joined = models.DateTimeField(_('date joined'), default=timezone.now)
    is_staff = models.BooleanField(
        _('staff status'),
        default=False,
        help_text=_('Designates whether the user can log into this admin site.'),
    )
    is_active = models.BooleanField(
        _('active'),
        default=True,
        help_text=_(
            'Designates whether this user should be treated as active. '
            'Unselect this instead of deleting accounts.'
        ),
    )

    objects = MyUserManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['nickname']

    def __str__(self):
        return self.email

    def clean(self):
        super().clean()
        self.email = self.__class__.objects.normalize_email(self.email)

    def get_full_name(self):
        return self.nickname

    def get_short_name(self):
        return self.nickname

    def email_user(self, subject, message, from_email=None, **kwargs):
        send_mail(subject, message, from_email, [self.email], **kwargs)


```

<br>

2) 참고사항

- Setting.py 에 AUTH_USER_MODEL 지정해주기

```python

AUTH_USER_MODEL = 'user.MyUser'

```

<br>

- foreignkey 설정하기

```python

# models.Foreignkey 사용하기
# 직접 모델을 입력하면 안되고, settings.AUTH_USER_MODEL을 사용해야 함

from django.conf import settings
from django.db import models

class Article(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )

```

<br>

# 2. User Model 변수 + 메소드 

### 1) `AbstractBaseUser` 상속 받은 후 , 설정가능한 변수들

- `USERNAME_FIELD` : 고유식별자로 사용될 Field 이름(unique=True 꼭 정의되어 있어야 함)
- `EMAIL_FIELD` : 메일 필드 이름
- `REQUIRED_FIEDLD` : 사용자를 생성할 때 표시되는 필드 이름 목록(USERNAME_FIELD,  password 는 기본으로 설정되어 있어서 넣지 않음 )

```python

class MyUser(AbstractBaseUser):
    ...
    date_of_birth = models.DateField()
    height = models.FloatField()
    ...
    REQUIRED_FIELDS = ['date_of_birth', 'height']

```

- `is_active` : 사용자가 활성으로 간주되는지 여부를 나타냄

```python

  ...
  is_active = models.BooleanField(default=True)

  ...

```

- `get_full_name()` , `get_short_name()` : name의 옵션으로 한국어는 firstname과 lastname이 따로 없으므로 잘 사용하지 않음.

  하지만, 개발할 때 다른 사람이 이 기능을 사용 할 수도 있으므로 메소드 자체를 지우는 것 보다는 return 값을 변경해준다.

```python

  ... 
    def get_full_name(self):
        return self.nickname

    def get_short_name(self):
        return self.nickname
      
  ...          
```

<br>

### 2) `AbstractBaseUser`의 하위 클래스

- `clean()` : USERNAME_FIELD의 필드값을 유니코드로 정규화 함

- `get_username()` : USERNAME_FIELD의 필드값을 돌려줍니다.

- `get_email_field_name()` : EMAIL_FIELD의 메일필드의 이름값을 돌려줌 (default : 'email' )
- `is_authenticated` : 로그인여부

```python
'''
is_authenticated 이용하여 로그인이 되어 있는지 확인 가능하다 
'''
# 현재 로그인한 사용자를 나타냄
>>> request.user
<SimpleLazyObject: <User: admin@admin.com>>
    
    
# 로그인한 모든 사용자의 is_authenticated의 값은 True    
>>> request.user.is_authenticated
True

```

- `is_anonymous` : 로그아웃 여부
- `set_password()` : 패스워드를 해싱처리함
- `check_password()` : 지정된 패스워드가 올바른 패스워드인지 확인함

<br>

# 2. User Manager Model 변수 + 메소드

- `create_user(*username_field,*password=None,**other_fields)` : user를 생성해줌

- `create_superuser(*username_field,*password=None,**other_fields)` : superuser를 생성해줌

<br>

# 3. 사용자의 상태 및 권한

1) `CustomUser` 의 변수

- `is_staff` : 사용자에게 관리자 사이트에 대한 액세스 권한이 허용된 경우 True를 반환
- `is_active` : 사용자 계정이 현재 활성 상태인 경우 True를 반환(사용자가 가입하면 활성화 되어있음)

- `has_perm(perm, obj=None)` : 사용자에게 명명된 사용 권한이 있는 경우 True를 반환
- `has_module_perm(app_label)` : 사용자에게 지정된 앱의 모델에 액세스할 수 있는 권한이 있는 경우 True를 반환

2) `PermissionsMixin` 의 변수

- `is_superuser` : 사용자가 모든 권한을 갖도록 지정함

```python

    is_superuser = models.BooleanField(
        _('superuser status'),
        default=False,
        help_text=_(
            'Designates that this user has all permissions without '
            'explicitly assigning them.'
        ),

```

<br>

# 4. Setting.py의 변수들

```python

# [setting.py]


[ 변수 ]  = 'Default 값'

# 사용자를 나타내는 데 사용할 모델 
AUTH_USER_MODEL = 'auth.User'
      
# 로그인 후 리디렉션되는 URL
LOGIN_URL = '/accounts/login/'

# 로그아웃 후 리디렉션되는 URL
LOGOUT_REDIRECT_URL = None

# loginview에서 GET 매개 변수를 얻지 못했을 때, 리디렉션되는 URL
LOGIN_REDIRECT_URL = '/accounts/profile/'

# LogoutView에 next_page 속성이 없는 경우 로그아웃 후 요청이 리디렉션되는 URL
LOGOUT_REDIRECT_URL = None

# 암호 재설정 링크가 유효한 최소 기간
PASSWORD_RESET_TIMEOUT_DAYS = 3

# Django가 암호를 저장하는 방법
PASSWORD_HASHERS =
[
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
    'django.contrib.auth.hashers.Argon2PasswordHasher',
    'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
]      

# 사용자 암호의 강도를 확인하는 데 사용되는 검증자 목록
AUTH_PASSWORD_VALIDATORS = []


```

<br>

# 5. Create _user, Create _superuser

- ###`create _user`

  1) `모델 폼을 이용`해 가입페이지로 user 정보를 받아서 생성

  2) `create_user` 의 함수를 호출하여 정보를 입력하여 생성

  ```python
  
  # user모델에 정의함
    objects = MyUserManager()
  
  ```

  MyUserManager에 정의해놓은 create_user로 사용자를 생성할 수 있다.

<br>

- ### `create_superuser`

  1) create_user를 통해서 생성할 수도 있으나, 편하게

  ```python
  
  $ python manage.py createsuperuser 
  
  ```

  의 명령어로 생성할 수 있다.

<br>

# 6. 암호변경하기

```python

>>> u = User.objects.get(username='john')
>>> u.set_password('new password')
>>> u.save()

```

<br>



