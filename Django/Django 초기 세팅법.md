# Django 초기 세팅법

## 0️⃣ 프로젝트를 진행할 폴더 생성

1. `mkdir <폴더명>`

## 1️⃣ 프로젝트 폴더로 이동 후 `.gitignore` 파일 생성

1. `cd <폴더명>`

2. `.gitignore` 파일 생성
   
   * www.toptal.com/developers/gitignore/ 에 접속
     
     * `python`, `django`, `VisualStudio` 등을 선택한 후 해당 내용 copy & paste

## 2️⃣ 가상환경 설정 및 활성화

1. `python -m venv venv`

2. `source venv/Script/activate`
   
   * Mac 인 경우 - `source venv/Bin/activate`

3. 활성화 상태 확인
   
   ![](Django%20초기%20세팅법_assets/2022-09-08-22-23-38-image.png)
   
   * `pip list` - 정상적으로 적용되었는지 확인

## 3️⃣ Django 및 필요한 패키지 설치

1. `pip install django==3.2` - django LTS ver.

2. requirements.txt 파일이 존재하는 경우
   
   * `pip install -r requirements.txt` - 파일에 있는 패키지 버전을 자동으로 설치

3. requirements.txt 파일이 없는 경우
   
   * `pip freeze > requirements.txt` - 현재까지 개발환경에서 사용하는 패키지의 버전을 명시하는 파일 설치

## 4️⃣ 장고 프로젝트 시작

1. `django-admin startproject config .` 
   
   * `.` 을 붙이면 현재 폴더 내에 바로 생성
   
   * `.` 안붙이면 프로젝트 폴더를 만들고 그 내부에 필요 폴더와 파일 생성

## 5️⃣ 장고 application 생성

1. `python manage.py startapp <앱이름>`
   
   * 앱 이름은 복수형으로 짓는 것이 관행

2. `settings.py` 에 생성한 앱을 등록
   
   * `,` 빠지지 않도록 주의❗
   
   ![](Django%20초기%20세팅법_assets/2022-09-08-22-36-42-image.png)

## 6️⃣ `base.html` 파일 만들기

1. 코드의 재사용성을 높이기 위한 파일

2. 위치는 전체 폴더 아래에 `templates` 라는 폴더 생성

3. `templates` 폴더를 `settings.py` 에 등록
   
   * `BASE_DIR / 'templates'`
   
   ![](Django%20초기%20세팅법_assets/2022-09-08-22-33-29-image.png)

4. `templates` 폴더 내부에 `base.html` 파일 생성
   
   * 부트스트랩의 CDN 주소 포함
   
   * DTL의 block tag 활용
     
     ```html
      <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Document</title>
         <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
     </head>
     <body>
         {% block content %}
         {% endblock content %}
         <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
     </body>
     </html>
     ```

## 7️⃣ url 분리

1. 앱 폴더 내부에 `urls.py` 파일 생성

2. `urls.py` 내부에 코드 작성
   
   * `urlpatterns` 라는 빈 리스트 꼭 생성해야 오류가 나지 않음❗
   
   * `app_name` 설정
     
     ```python
     from django.urls import path
     from . import views
     
     
     app_name = 'accounts'
     urlpatterns = [
         
     ]
     
     
     ```

3.  프로젝트 폴더 내 `urls.py` 에서 방금 생성한 `앱/urls.py` 를 등록

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('accounts.urls')),
]
```

## 8️⃣ (추가) 회원 가입 기능이 있는 경우 User Custom 세팅

1. `models.py` 에 User 모델을 정의
   
   * 이 때 상속 받는 클래스는 `AbstractUser`

```python
# accounts/models.py

from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass    # 비워두게 되면 에러가 발생하므로 pass 를 작성해둠
```

2. `settings.py` 에 `AUTH_USER_MODEL` 값을 설정

```python
# settings.py
...

# 이 때 accounts 는 User 클래스가 정의된 application 이름
AUTH_USER_MODEL = 'accounts.User'
```

3. (선택사항) `admin.py` 에 등록

```python
# accounts/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin  # 기존에 사용하는 User 관리 interface 설정
from .models import User  # 새롭게 등록한 User 모델

admin.site.register(User, UserAdmin)
```


