# Namespace

> 개체를 구분할 수 있는 범위를 나타내는 이름 공간에 대한 이해

## ▶ Namespace 의 필요성

### ✔ 2가지 문제 발생

* 1️⃣ **URL namespace**
  
  * articles app index 페이지에 작성한 두 번째 앱 index로 이동하는 하이퍼링크를 클릭 시 현재 페이지로 다시 이동

* 2️⃣ **Template namespace**
  
  * pages app의 index url(~/pages/index)로 직접 이동해도 articles app의 index 페이지가 출력됨

## ▶ URL Namespace

* URL namespace를 사용하면 서로 다른 앱에서 동일한 URL 이름을 사용하는 경우에도 이름이 지정된 URL을 고유하게 사용할 수 있다.

* <mark>**`app_name` 속성을 작성해 URL namespace를 설정!**</mark>
  
  * urls.py 내부에 **`app_name = 'articles'`, `app_name = 'pages'`** 설정
  
  * **`{% url 'articles:index' %}`** 형태로 수정
  
  * **app_name을 지정한 이후에는 url 태그에서 반드시 app_name:url_name 형태로만 사용해야 한다**❗ 그렇지 않으면  **`NoReverseMatch`(url 태그 수정)** 에러 발생❗

## ▶ Template namespace

* Django는 기본적으로 <mark>**`app_name/templates/`**</mark> 경로에 있는 templates 파일들만 찾을 수 있으며, <mark>**settings.py의 INSTALLED_APPS 에 작성한 app 순서로 template을 검색 후 렌더링**</mark>

* Django template의 기본 경로 자체를 변경할 수 없기 때문에 물리적으로 이름 공간을 만들어야 한다.
  
  * 폴더 구조를 <mark>**`app_name/template/app_name/`**</mark> 형태로 변경
  
  * ![](Django_2_assets/2022-08-31-23-40-54-image.png)
  
  * 템플릿 경로를 모두 변경해주어야 한다!
    
    * `articles/index.html`
    
    * `pages/index.html`



# Database

> 체계화된 데이터의 모임
> 
> 검색 및 구조화 같은 작업을 쉽게 하기 위해 조직화된 데이터를 수집하는 저장 시스템

## ▶ 기본 구조

### 1️⃣ 스키마(Schema)

* 데이터베이스에서 자료의 구조, 표현 방법, 관계 등을 정의한 구조 (=Structure)

![](Django_2_assets/2022-08-31-23-47-15-image.png)

### 2️⃣ 테이블(Table)

* 필드와 레코드를 사용해 조직된 데이터 요소들의 집합

* 관계(Relation)라고도 부름
  
  ![](Django_2_assets/2022-08-31-23-49-03-image.png)

* **필드(field) = 컬럼**
  
  * 각 필드에는 고유한 데이터 형식이 지정됨
    
    * INT, TEXT 등

* **레코드(record) = 행**
  
  * 테이블의 데이터는 레코드에 저장됨

* **PK (Primary Key) = 기본키**
  
  * 각 레코드의 고유한 값(식별자로 사용)
  
  * 기술적으로 <mark>**다른 항목과 절대로 중복되어 나타날 수 없는 단일값(unique)을 가짐**</mark>
  
  * **데이터베이스 관리 및 테이블 간 관계 설정 시 주요하게 활용**

![](Django_2_assets/2022-08-31-23-53-17-image.png)

### ✔ 쿼리(Query)

* 데이터를 조회하기 위한 명령어를 의미

* **조건에 맞는 데이터를 추출하거나 조작하는 명령어** (주로 테이블형 자료구조에서)

* Query를 날린다 = 데이터베이스를 조작한다.



# Model

> **Django는 웹 애플리케이션의 데이터를 구조화하고 조작하기 위한 추상적인 모델을 제공**

![](Django_2_assets/2022-08-31-23-59-01-image.png)

* Model을 통해 데이터에 접속하고 관리

* **사용하는 데이터들의 필수적인 필드(column)들과 동작(인스턴스와 메서드)들을 포함**

* 저장된 데이터베이스의 구조

* **일반적으로 각각의 모델은 하나의 데이터베이스 테이블에 mapping**
  
  * <mark>**모델 클래스 1개 == 데이터베이스 테이블 1개**</mark>

## ▶ Model 작성하기

* models.py 작성
  
  * 모델 클래스를 작성하는 것은 데이터베이스 <mark>**테이블의 스키마를 정의하는 것**</mark>
  
  * "모델 클래스 == 테이블 스키마"
    
    ![](Django_2_assets/2022-09-01-00-12-45-image.png)
  
  * <mark>**id 컬럼은 테이블 생성 시 Django가 자동으로 생성**</mark>

## ▶ Model 이해하기

* **`models.Model`**
  
  * 각 모델은 django.db.models 모듈의 Model 클래스를 상속받아 구성됨
  
  * <mark>**클래스 상속 기반 형태의 Django 프레임워크 개발**</mark>

* **`title = models.CharField(max_length=10)`**

* **`content = models.TextField()`**
  
  * models 모듈을 통해 어떠한 타입의 DB 필드를 정의할 것인지 정의
  - 클래스 변수명 = DB 필드의 이름
  - 클래스 변수 값(models 모듈의 Field 클래스) = DB 필드의 데이터 타입

* **데이터베이스 스키마 생성**
  
  ![](Django_2_assets/2022-09-01-00-29-40-image.png)

### ✔ Model Field

* **데이터 유형에 따라 다양한 모델 필드를 제공**
  
  * `DataField(),, CharField(), IntegerField()` 등
  
  * https://docs.djangoproject.com/en/3.2/ref/models/fields/

* <mark>**`CharField(max_length=None, **options)`**</mark>
  
  * 길이의 제한이 있는 문자열을 넣을 때 사용
  
  * **<mark>`max_length`</mark>**
    
    * 필드의 최대 길이(문자, 255자)
    
    * **CharField의 필수 인자**
    
    * **데이터베이스의 Django의 유효성 검사(값을 검증하는 것)에서 활용됨**

* <mark>`TextField(**options)`</mark>
  
  * 글자 수가 많을 때 사용

## ▶ Migrations

> **Django가 모델에 생긴 변화(필드 추가, 모델 삭제 등)를 DB에 반영하는 방법**

### ✔ 주요 명령어

#### 1️⃣ `python manage.py makemigrations`

* 모델을 작성 혹은 변경한 것에 기반한 새로운 migration(설계도)을 만들 때 사용

* **테이블을 만들기 위한 설계도를 생성하는 것**

#### 2️⃣ `python manage.py migrate`

* **makemigration 으로 만든 설계도를 실제 db.splite3 DB 파일에 반영하는 과정**

* 결과적으로 **모델에서의 변경사항들과 DB의 스키마가 동기화를 이룸**
  
  * <mark>**모델과 DB의 동기화**</mark>

![](Django_2_assets/2022-09-01-00-36-57-image.png)

### ✔ 추가 필드 정의 - Model 변경사항 반영하기

* 기존에 id, title, content 컬럼을 가진 테이블에 2개의 컬럼을 추가(변경)하는 상황
  
  * `python manage.py makemigrations`

* Django 입장에서는 **기존 테이블에 새로운 컬럼을 추가하라는 요구 사항인데 이 컬럼들은 기본적으로 빈 값으로 추가될 수 없음❗**
  
  * **그래서 추가되는 컬럼에 기본값을 설정하라는 것임**

![](Django_2_assets/2022-09-01-00-43-50-image.png)

* 각 보기 번호의 의미
  
  1. 다음 화면으로 넘어가서 새 컬럼의 기본 값을 직접 입력하는 방법
  
  2. 현재 과정에서 나가고 모델 필드에 default 속성을 직접 작성하는 방법

![](Django_2_assets/2022-09-01-00-46-02-image.png)

* 그냥 Enter 입력하면 Django에서 기본적으로 timezone 모듈의 now 메서드 반환 값을 사용한다.

* **새로운 설계도를 생성했기 때문에 DB와 동기화를 진행해야 함**
  
  * `python manage.py migrate`

* **DateTimeField()**
  
  * datetime.datetime 인스턴스로 표시되는 날짜 및 시간을 값으로 사용하는 필드
  
  * **선택인자**‼
    
    * 1️⃣ **`auto_now_add`**
      
      * 최초 생성 일자 = 최초 insert 시에만 현재 날짜와 시간으로 갱신
    
    * 2️⃣ **`auto_now`**
      
      * 최종 수정 일자 = ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신

# ORM

> Object-Relational-Mapping
> 
> 객체 지향 프로그래밍 언어를 사용하여 **호환되지 않는 유형(Django와 SQL)의 시스템 간에 데이터를 변환하는 프로그래밍 기술**

![](Django_2_assets/2022-09-01-00-55-53-image.png)

* **장점**
  
  * **SQL을 잘 알지 못해도 객체지향 언어로 DB 조작 가능**
  
  * 생산성이 높음 = 빠르게 개발할 수 있음

* **단점**
  
  * **ORM 만으로는 완전한 서비스를 구현하기 어렵다.**

## ▶ QuerySet API

* Django shell 실행
  
  * `python manage.py shell_plus`

* **Database API**
  
  * Model을 정의하면 데이터를 만들고 읽고 수정하고 지울 수 있는 API를 제공
  
  * **구문**
    
    ![](Django_2_assets/2022-09-01-01-07-22-image.png)

* **objects manager**
  
  * 데이터베이스 쿼리 작업을 가능하게 하는 인터페이스
  
  * 이 manager(objects)를 통해 특정 데이터를 조작(메서드)할 수 있음
  
  * **DB를 Python class로 조작할 수 있도록 여러 메서드를 제공하는 manager**

* **Query**
  
  * 데이터베이스에 특정한 데이터를 보여달라는 요청
  
  * 쿼리문을 작성한다 = 원하는 데이터를 얻기 위해 데이터베이스에 요청할 코드 작성
  
  * **ORM에 의해 SQL로 변환되어 DB에 전달 & DB의 응답 데이터를 ORM이 QuerySet이라는 자료 형태로 변환하여 우리에게 전달**

* **QuerySet**
  
  * **데이터베이스에게서 전달 받은 객체 목록(데이터 모음) - 순회 및 인덱스 접근 가능**
  
  * 필터를 걸거나 정렬 등으로 수행 가능
  
  * 단, DB가 단일한 객체를 반환할 때는 QuerySet이 아닌 모델(Class)의 인스턴스로 반환

* **QuerySet API**
  
  * QuerySet 과 상호작용하기 위해 사용하는 도구 (메서드, 연산자 등)
    
    ![](Django_2_assets/2022-09-01-01-13-26-image.png)

## QuerySet API

### ✔ CRUD

> 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능 4가지

* Create / Read / Update / Delete = 생성 / 조회 / 수정 / 삭제


