# Vue with DRF

* 개요
  
  * Server와 Client의 통신 방법 이해하기
  
  * CORS 이슈 이해하고 해결하기
  
  * DRF Auth System 이해하기
  
  * Vue와 API server 통신하기

# Server & Client

## ▶ Server

> **클라이언트에게 <mark>정보와 서비스를 제공</mark>하는 컴퓨터 시스템**

* **서비스 전체를 제공 == Django Web Service**
  
  * Django를 통해 전달 받은 HTML에는 하나의 웹 페이지를 구성할 수 있는 모든 데이터가 포함
  
  * 서버에서 모든 내용을 렌더링 하나의 HTML 파일로 제공
  
  * 정보를 포함한 web 서비스를 구성하는 모든 내용을 서버 측에서 제공

![](with%20DRF_assets/2022-11-14-10-15-37-image.png)

* **정보를 제공 == DRF API Service**
  
  * Django를 통해 관리하는 정보만을 클라이언트에게 제공
  
  * DRF를 사용하여 JSON으로 변환

![](with%20DRF_assets/2022-11-14-10-16-15-image.png)

## ▶ Client

> **<mark>Server가 제공하는 서비스에 적절한 요청</mark>을 통해 <mark>Server로부터 반환 받은 응답을 사용자에게 표현</mark>하는 기능을 가진 프로그램 혹은 시스템**

* Server가 제공하는 서비스에 적절한 요청
  
  * Server가 정의한 방식대로 요청 인자를 넘겨 요청
  
  * Server는 정상적인 요청에 적합한 응답 제공

![](with%20DRF_assets/2022-11-14-10-17-54-image.png)

* *잘못된 요청의 예*
  
  * *정의된 Model에 맞지 않는 field 명으로 요청할 경우 처리할 수 없음*

* Server로부터 반환 받은 응답을 사용자에게 표현
  
  * 사용자의 요청에 적합한 data를 server에 요청하여 응답 받은 결과로 적절한 화면을 구성

## ▶ 정리

* **Server는 정보와 서비스를 제공 = DRF**
  
  * **DB와 통신**하며 데이터를 생성, 조회, 수정, 삭제를 담당
  
  * 요청을 보낸 Client에게 **정상적인 요청이었다면 처리한 결과를 응답**

* **Client는 사용자의 정보 요청을 처리, server에게 응답 받은 정보를 표현 = Vue**
  
  * Server에게 정보(데이터)를 요청
  
  * **응답 받은 정보를 가공하여 화면에 표현**

# DRF

* **Model 구조(DB) 확인**

* **요청 경로 확인 = `urls.py` 체크**

* Dummy data 확인 = json 파일
  
  * `python manage.py migrate`
  
  * **`python manage.py loaddata aritcles.json comments.json` = 데이터 삽입**

* 서버 실행 후 전체 게시글 조회
  
  * Browser에서 url 입력 후 데이터 반환 확인
  
  * Postman을 통해 데이터 반환 확인

# Vue

* front-server 폴더 구조 확인 및 서버 구동 준비
  
  * `npm install` = `npm i`
  
  * `npm run serve`

* 컴포넌트 구조 확인

![](with%20DRF_assets/2022-11-14-10-31-29-image.png)

## ▶ 메인 페이지 구성

* **`views/ArticleView.vue` component 확인 및 route 등록**

* **`src/App.vue` 내 router-link 작성**

```html
<!-- src/App.vue -->
<nav>
  <router-link :to="{ name: 'ArticleView' }">Aritcles</router-link>
</nav>
```

* **`components/ArticleList.vue` 확인**
  
  * 전체 게시물을 표현할 컴포넌트
  
  * 화면 구성을 위한 최소한의 style 포함

```javascript
<template>
  <div class="article-list">
    <h3>Article List</h3>
  </div>
</template>

<script>
import ArticleListItem from '@/components/ArticleListItem'

export default {
  name: 'ArticleList',
}
</script>

<style>
.article-list {
  text-align: start;
}
</style>
```

* **`views/AritcleView.vue` 에서 ArticleList를 하위 컴포넌트로 등록**
  
  * 불러오기 > 등록하기 > 보여주기

* **`components/AritcleListItem.vue` 확인**
  
  * 각 게시글들의 정보를 표현할 컴포넌트
  
  * 데이터 없이 최소한의 기본 구조만 확인

* **`components/AritcleList.vue`**
  
  * `ArticleListItem` 하위 컴포넌트 등록 = 불러오기 > 등록하기 > 보여주기

* **`store/index.js`**
  
  * state에 articles 배열 정의
  
  * 화면 표현 체크용 데이터 생성

```javascript
export default new Vuex.Store({
  state: {
    articles: [
      {
        id: 1,
        title: '제목',
        content: '내용'
      },
      {
        id: 2,
        title: '제목2',
        content: '내용2'
      },
    ],
  },
```

* **`components/ArticleList.vue` 수정**
  
  * state에서 articles 데이터 가져오기
  
  * `v-for` 디렉티브를 활용하여 하위 컴포넌트에서 사용할 article 단일 객체 정보를 pass props

```javascript
<template>
  <div class="article-list">
    <h3>Article List</h3>
    <ArticleListItem
      v-for="article in articles" :key="article.id"
      :article="article"
    />
  </div>
</template>

<script>
export default {
  name: 'ArticleList',
  ...,
  computed: {
    articles() {
      return this.$store.state.articles
    }
  }
}
</script>
```

* **`components/ArticleListItem.vue` 수정**
  
  * 내려받은 prop 데이터로 화면 구성
  
  * prop 데이터의 타입은 명확하게 표기할 것

```javascript
<template>
  <div>
    <h5>{{ article.id }}</h5>
    <p>{{ article.title }}</p>
    <hr>
  </div>
</template>

<script>
export default {
  name: 'ArticleListItem',
  props: {
    article: Object,
  }
}
</script>
```

# Vue with DRF

## ▶ AJAX 요청 준비

* **`axios` 설정**
  
  * `npm i axios` = 설치
  
  * `store/index.js` 에서 불러오기
    
    * 요청 보낼 API server 도메인 변수에 담기

```javascript
// store/index.js
import axios from 'axios'

const API_URL = 'http://127.0.0.1:8000'
```

* **`store/index.js`**
  
  * `getArticles` 메서드 정의
  
  * 요청 보낼 경로 확인 필수

```javascript
export default new Vuex.Store({
  ...
  actions: {
    getArticles(context) {
      axios({
        method: 'get',
        // 전체 게시글 조회 페이지 주소 (Django url에서 확인 필요!)
        url: `${API_URL}/api/v1/articles`,
      })
        .then((res) => {
          console.log(res, context)
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
})
```

* **`views/AritcleView.vue`**
  
  * `getArticles` actions 호출
  
  * 인스턴스가 생성된 직후 요청을 보내기 위해 `created()` hook 사용

```javascript
<script>
import ArticleList from '@/components/ArticleList'

export default {
  name: 'ArticleView',
  components: {
    ArticleList,
  },
  computed:{
  },
  // 인스턴스가 실행될 때 바로 Django 측과 통신하기 위함
  created() {
    this.getArticles()
  },
  methods: {
    getArticles() {
      this.$store.dispatch('getArticles')
    }
  }
}
</script>
```

## ▶ 요청 결과 확인

> Vue와 Django 서버를 모두 활성화 후 메인 페이지 접속

* **Server에서는 200을 반환하지만, Client Console에서는 Error를 확인**

![](with%20DRF_assets/2022-11-14-12-05-07-image.png)

* *데이터를 확인할 수 없는 이유❓*
  
  * **CORS policy 에 의해 blocked 되었기 때문**❗

# CORS

> **Cross-Origin Resource Sharing**

## ▶ What Happened?

* 브라우저가 요청을 보내고 서버의 응답이 브라우저에 도착
  
  * **Server의 log는 200(정상) 반환**
  
  * Server는 정상적으로 응답했지만 브라우저가 막은 것

* *보안의 이유로 브라우저는 <mark>동일 출처 정책(SOP)에 의해</mark> 다른 출처의 리소스와 상호작용 하는 것을 제한*

## ▶ SOP (Same-Origin Policy)

> 동일 출처 정책

* **불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용 하는 것을 제한하는 보안 방식**

* *잠재적으로 해로울 수 잇는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임*

## ▶ Origin = "출처"

* **<mark>URL의 Protocol, Host, Port를 모두 포함</mark>하여 출처라고 부름**

* Same Origin 예시
  
  * `http://localhost:3000/posts/3`
  
  * `scheme/protocol`  // `Host` : `Port` 가 일치하는 경우에만 동일 출처로 인정

![](with%20DRF_assets/2022-11-14-11-13-09-image.png)

## ▶ CORS = 교차 출처 리소스 공유

> 어떤 출처에서 자신의 컨텐츠를 불러갈 수 있는지 **서버에 지정할 수 있는 방법**

* **추가 <mark>HTTP Header를 사용</mark>하여, 특정 출처에서 실행 중인 웹 어플리케이션이 <mark>다른 출처의 자원에 접근할 수 있는 권한을 부여</mark>하도록 브라우저에 알려주는 체제**

* **리소스가 자신의 출처와 다를 때 교차 출처 HTTP 요청을 실행**
  
  * 만약 다른 출처의 리소스를 가져오기 위해서는 이를 제공하는 **<mark>서버가 브라우저에게</mark> 다른 출처지만 접근해도 된다는 사실을 알려야 함**
  
  * 교차 출처 리소스 공유 정책 (CORS policy)

## ▶ CORS policy

> 다른 출처에서 온 리소스를 공유하는 것에 대한 정책

* CORS policy 에 위배되는 경우 브라우저에서 해당 응답 결과를 사용하지 않음
  
  * *Server에서 응답을 주더라도 브라우저에서 거절*

* 다른 출처의 리소스를 불러오려면 그 출처에서 **올바른 CORS header를 포함한 응답을 반환해야 함**

## ▶ How to set CORS

* HTTP Response Header 예시
  
  * **<mark>Access-Control-Allow-Origin</mark>**
    
    * 단일 출처를 지정하여 브라우저가 해당 출처가 리소스에 접근하도록 허용

필기 이미지 필요

## ▶ django-cors-headers library 사용하기

> django-cors-headers github에서 내용 확인 가능

* **응답에 CORS header를 추가해주는 라이브러리**

* 다른 출처에서 Django 애플리케이션에 대한 브라우저 내 요청을 허용함

* 라이브러리 설치 및 `requirements.txt` 업데이트
  
  * **`pip install django-cors-headers`**
  
  * `pip freeze > requirements.txt`

* **App 추가 및 MIDDLEWARE 추가**
  
  * **CorsMiddleware는 가능한 CommonMiddleware 보다 먼저 정의해야 함**❗

```python
# my_api/settings.py

INSTALLED_APPS = [
    ...
    # CORS policy
    'corsheaders',
    ...
]

MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]
```

* **CORS_ALLOWED_ORIGINS**에 교차 출처 자원 공유를 허용할 Domain 등록

```python
# my_api/settings.py

# 특정 Origin만 선택적으로 허용 
CORS_ALLOWED_ORIGINS = [
    'http://127.0.0.1:8000',
]
```

* 만약 모든 Origin을 허용하고자 한다면

```python
# my_api/setting.py

# 모든 Origin 허용 
CORS_ALLOWED_ALL_ORIGINS = True
```

# Vue with DRF (feat.CORS)

## ▶ Article Read

* **응답 받은 데이터 구조 확인**
  
  * **`data Array` 에 각 게시글 객체**
  
  * 각 게시글 객체는 다음으로 구성
    
    * 1️⃣ id
    
    * 2️⃣ title
    
    * 3️⃣ content

* **`store/index.js` 수정**
  
  * 기존 articles 데이터 삭제
  
  * Mutations 정의
    
    * 응답 받아온 데이터를 state에 저장

```javascript
// Django 서버의 주소
const API_URL = 'http://127.0.0.1:8000'

export default new Vuex.Store({
  state: {
    articles: [],
  },
  mutations: {
    GET_ARTICLES(state, articles) {
      state.articles = articles
    },
  },
  actions: {
    getArticles(context) {
      axios({
        method: 'get',
        // 전체 게시글 조회 페이지 주소 (Django url에서 확인 필요!)
        url: `${API_URL}/api/v1/articles/`,
      })
        .then((res) => {
          // console.log(res, context)
          // console.log(res.data)
          context.commit('GET_ARTICLES', res.data)
        })
        .catch((err) => {
          console.log(err)
        })
    },
})
```

* 결과 확인 = 정상적으로 데이터 출력 확인
  
  * 사전에 ArticleList.vue 에서 state로 화면을 구성하도록 설정

## ▶ Article Create

* **`views/CreateView.vue`**
  
  * 게시글 생성을 위한 form 을 제공
  
  * **`v-model.trim`을 활용해 사용자 입력 데이터에서 공백 제거**
  
  * **`.prevent`를 활용해 form의 기본 이벤트 동작 막기**
  
  * **title, content가 비었다면 `alert`를 통해 경고창 띄우기**
  
  * **AJAX 요청을 보내지 않도록 `return` 시켜 함수를 종료**
  
  * axios를 사용해 server에 게시글 생성 요청
    
    * *actions를 사용하지 않는 이유❓*
      
      * **state를 변화시키는 것이 아닌 <mark>DB에 게시글 생성 후 ArticleView로 이동</mark>할 것이므로 methods에서 직접 처리**❗

```javascript
<template>
  <div>
    <h1>게시글 작성</h1>
    <form @submit.prevent="createArticle">
      <label for="title">제목 : </label>
      <input type="text" id="title" v-model.trim="title"><br>
      <label for="content">내용 : </label>
      <textarea id="content" cols="30" rows="10" v-model.trim="content"></textarea><br>
      <input type="submit" id="submit">
    </form>
  </div>
</template>

<script>
import axios from 'axios'

const API_URL = 'http://127.0.0.1:8000'

export default {
  name: 'CreateView',
  data() {
    return {
      title: null,
      content: null,
    }
  },
  methods: {
    // 게시글 작성 안됨 = 401 응답 = 매 요청마다 토큰 보내줘야 함
    createArticle() {
      const title = this.title
      const content = this.content
      if (!title) {
        alert('제목을 입력해주세요')
        return
      } else if (!content) {
        alert('내용을 입력해주세요')
      }
      axios({
        method: 'post',
        url: `${API_URL}/api/v1/articles/`,
        data: {
          title: title,
          content: content,
        },
        header: {
          Authorization: `Token ${this.$store.state.token}`
        }
      })
        .then((res) => {
          console.log(res)
          this.$router.push({ name: 'ArticleView'})
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>
```

* **`router/index.js`**

```javascript
import CreateView from '@/views/CreateView'

Vue.use(VueRouter)

const routes = [
  ...,
  {
    path: '/create',
    name: 'CreateView',
    component: CreateView
  },
]
```

* **`views/ArticleView.vue`**
  
  * router-link 를 통해 CreateView로 이동

```html
<template>
  <div>
    <h1>Article Page</h1>
    <router-link :to="{ name: 'CreateView' }">[CREATE]</router-link>
    <hr>
    <ArticleList/>
  </div>
</template>
```

* 게시글 작성 요청 결과 확인 = 정상 작동 확인

* **`views/CreateView.vue`**
  
  * **createArticle method 수정 = 게시글 생성 완료 후 ArticleView로 이동**
  
  * 응답 확인을 위해 정의한 **인자 `res` 제거**

```javascript
export default {
  ...,
  methods: {
    // 게시글 작성 안됨 = 401 응답 = 매 요청마다 토큰 보내줘야 함
    createArticle() {
      ...
      axios({
        method: 'post',
        url: `${API_URL}/api/v1/articles/`,
        data: {
          title: title,
          content: content,
        },
      })
        .then(() => {
          this.$router.push({ name: 'ArticleView'})
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
```

* 게시글 작성 요청 결과 재확인
  
  * 게시글 생성 후 ArticleView로 이동
  
  * 새로 생성된 게시글 확인 가능

* **어떻게 router로 이동만 했는데 보이는 걸까**❓
  
  * **ArticleView가 create될 때 마다 server에 게시글 전체 데이터를 요청하고 있기 때문**❗

### 📌 [참고] 지금의 요청 방식은 효율적인가❓

* *비효율적인 부분이 존재*
  
  * **전체 게시글 정보를 요청해야 새로 생성된 게시글을 확인할 수 있음**
  
  * 만약 vuex state를 통해 전체 게시글 정보를 관리하도록 구성한다면❓
    
    * 내가 새롭게 생성한 게시글은 확인할 수 있지만...
  
  * 나 이외의 유저들이 새롭게 생성한 게시글은 언제 불러와야 할까❓
  
  * 무엇을 기준으로 새로운 데이터가 생겼다는 것을 확인할 수 있을까❓

* 내가 구성하는 서비스에 따라 데이터 관리 방식을 고려해 보아야 함

## ▶ Article Detail

* **`views/DetailView.vue`**
  
  * 게시글 상세 정보를 표현할 컴포넌트
  
  * AJAX 요청으로 응답 받아올 article의 상세 정보들을 표현

* **`router/index.js`**
  
  * id를 동적 인자로 입력 받아 특정 게시글에 대한 요청

```javascript

```

* **`components/ArticleListItem.vue`**
  
  * router-link를 통해 특정 게시글의 id값을 동적 인자로 전달
  
  * 게시글 상세 정보를 server에 요청

```html

```

* **`views/DetailView.vue`**
  
  * **`this.$route.params`를 활용해 컴포넌트가 create될 때, 넘겨받은 id로 상세 정보 AJAX 요청**

```javascript

```

* 게시글 상세 정보 요청 결과 확인
  
  * 정상 작동 확인
  
  * 넘겨 받은 데이터 구조 확인 후 적절하게 화면 구성

* **`views/DetailView.vue` 수정**
  
  * 응답 받은 정보를 data에 저장
  
  * data에 담기까지 시간이 걸리므로 optional chaining을 활용해 데이터 표기

```javascript

```

* 최종 결과 확인

이미지

# DRF Auth System

## ▶ Authentication - 인증, 입증

> 자신이라고 주장하는 사용자가 누구인지 확인하는 행위

* **모든 보안 프로세스의 첫 번째 단계 = 가장 기본 요소**
  
  * 내가 누구인지를 확인하는 과정

* **401 Unauthorized**
  
  * 비록 HTTP 표준에서는 미승인(unauthorized)을 명확히 하고 있지만, 의미상 이 응답은 비인증(unauthenticated)을 의미

## ▶ Authorization - 권한 부여, 허가

> **사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정(절차)**

* 보안 환경에서는 **권한 부여는 <mark>항상 인증이 먼저 필요</mark>함**
  
  * 사용자는 조직에 대한 액세스 권한을 부여받기 전에 **먼저 자신의 ID가 진짜인지 먼저 확인해야 함**

* 서류의 등급, 웹 페이지에서 글을 조회 & 삭제 & 수정할 수 있는 방법, 제한 구역 등
  
  * *인증이 되었어도 모든 권한을 부여 받는 것은 아님*

* **403 Forbidden**
  
  * *401과 다른 점은 서버는 클라이언트가 누구인지 알고 있음 = 권한만 없을 뿐*

## ▶ Authentication and Authorization work together

* 회원가입 후 로그인 시 서비스를 이용할 수 있는 권한 생성
  
  * 인증 이후에 권한이 따라오는 경우가 많음

* 단, 모든 인증을 거쳐도 권한이 동일하게 부여되는 것은 아님
  
  * Django에서 로그인을 했더라도 다른 사람의 글까지 수정/삭제가 가능하진 않음

* 세션, 토큰, 제 3자를 활용하는 등의 다양한 인증 방식이 존재

# How to authentication determined

## ▶ 인증 여부 확인 방법

* DRF 공식문서에서 제안하는 인증 절차 방법
  
  * https://www.django-rest-framework.org/api-guide/authentication/
  
  ```python
  REST_FRAMEWORK = {
      'DEFAULT_AUTHENTICATION_CLASSES': [
          'rest_framework.authentication.BasicAuthentication',
          'rest_framework.authentication.SessionAuthentication',
      ]
  }
  ```

* `settings.py`에 작성해야 할 설정
  
  * **기본적인 인증 절차를 어떠한 방식으로 둘 것이냐를 설정하는 것**
  
  * 예시의 2가지 방법 외에도 각 framework마다 다양한 인증 방식이 있음

* 우리가 사용할 방법은 DRF가 기본으로 제공해주는 인증 방식 중 하나인 <mark>**`TokenAuthentication`**</mark>

* **모든 상황에 대한 인증 방식을 정의하는 것**이므로, *각 요청에 따라 다른 인증 방식을 거치고자 한다면 다른 방식이 필요*

* view 함수마다(각 요청마다) 다른 인증 방식을 설정하고자 한다면 decorator 활용
  
  ```python
  @api_view(['GET'])
  @authentication_classes([SessionAuthentication, BasicAuthentication])
  @permission_classes([IsAuthenticated])
  def example_view(request, format=None):
      content = {
          'user': str(request.user),  # `django.contrib.auth.User` instance.
          'auth': str(request.auth),  # None
      }
      return Response(content)
  ```

* [참고] permission_classes
  
  * 권한 관련 설정
  
  * 권한 역시 특정 view 함수마다 다른 접근 권한을 요구할 수 있음

## ▶ 다양한 인증 방식

* **`BasicAuthentication`**
  
  * 가장 기본적인 수준의 인증 방식
  
  * 테스트에 적합

* **`SessionAuthentication`**
  
  * Django에서 사용하였던 session 기반의 인증 시스템
  
  * DRF와 Django의 session 인증 방식은 보안적 측면을 구성하는 방법에 차이가 있음

* **`RemoteUserAuthentication`**
  
  * Django의 Remote user 방식을 사용할 때 활용하는 인증 방식

* **`TokenAuthentication`**
  
  * 매우 간단하게 구현할 수 있음
  
  * 기본적인 보안 기능 제공
  
  * 다양한 외부 패키지가 있음

* **(중요) settings.py 에서 `DEFAULT_AUTHENTICATION_CLASSES`를 정의**
  
  * **`TokenAuthentication` 인증 방식을 사용할 것임을 명시**

## ▶ TokenAuthentication 사용 방법

* INSTALLED_APPS에 `rest_framework.authtoken` 등록

```python
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```

* 각 User 마다 고유 Token 생성

```python
from rest_framework.authtoken.models import Token

token = Token.objects.create(user=...)
print(token.key)
```

* 생성한 Token을 각 User에게 발급
  
  * User는 발급 받은 Token을 요청과 함께 전송
  
  * Token을 통해 User 인증 및 권한 확인

* User는 발급 받은 Token을 headers에 담아 요청과 함께 전송
  
  * **단, <mark>반드시 Token 문자열 함께 삽입</mark>**
    
    * 삽입해야 할 문자열은 각 인증 방식마다 다름
  
  * **주의❗) Token 문자열과 발급받은 실제 token 사이를 `' '(공백)`으로 구분**

* Authorization HTTP headers 작성 방법 (예시)

```python
Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b
```

## ▶ 토론 생성 및 관리 문제점

* 기본 제공 방식에서 고려하여야 할 사항들
  
  1️⃣ **Token 생성 시점**
  
  2️⃣ **생성한 Token 관리 방법**
  
  3️⃣ **User와 관련된 각종 기능 관리 방법**
  
  * 회원가입
  
  * 로그인
  
  * 회원 정보 수정
  
  * 비밀번호 변경 등

# dj-rest-auth

> 회원가입, 인증(소셜미디어 인증 포함), 비밀번호 재설정, 사용자 세부정보 검색, 회원정보 수정 등을 위한 REST API end point 제공

* 주의❗) *`django-rest-auth`는 더 이상 업데이트를 지원하지 않음*
  
  * **`dj-rest-auth` 사용❗**

## ▶ dj-rest-auth 사용 방법

1️⃣ 패키지 설치 = `pip install dj-rest-auth`

2️⃣ App 등록

```python
INSTALLED_APPS = [
    ...,
    'rest_framework',

    # Auth
    'rest_framework.authtoken',
    'dj_rest_auth',',
```

3️⃣ url 등록

```python
urlpatterns = [
    path('dj-rest-auth/', include('dj_rest_auth.urls')),
]
```

## ▶ 시작하기 전...

* 시작하기 전, auth.User를 accounts.User로 변경 필요
  
  * auth.User로 설정된 DB 제거

* `my_api/settings.py`

```python
INSTALLED_APPS = [
    # Django Apps
    'accounts',
    'articles',
    ...,
]
```

## ▶ dj-rest-auth 사용하기

* `dj-rest-auth` 설치

* `my_api/settings.py`

```python
INSTALLED_APPS = [
    ...,
    'rest_framework',

    # Auth
    'rest_framework.authtoken',
    'dj_rest_auth',',
```

* migrate = `pip manage.py migrate`

* `my_api/urls.py`

```python
urlpatterns = [
    ...,
    path('accounts/', include('dj_rest_auth.urls')),
]
```

* 결과 확인
  
  * `/accounts/`로 이동
  
  * 회원가입 기능 없음

* Github 재확인
  
  * 상세 옵션은 공식 문서를 참고

* 공식문서로 이동
  
  * Registration (optional) 확인

## ▶ Registration

* Registration 기능을 사용하기 위해 추가 기능 등록 및 설치 필요
  
  * dj-rest-auth는 소셜 회원가입을 지원
  
  * **dj-rest-auth의 회원가입 기능을 사용하기 위해서는 `django-allauth` 필요**

* **django-allauth 설치 = `pip install 'dj-rest-auth[with_social]'`**

* **my_api/settings.py**
  
  * App 등록 및 SITE_ID 설정

* [참고] SITE_ID는 무엇인가❓
  
  * Django는 하나의 컨텐츠를 기반으로 여러 도메인에 컨텐츠를 게시 가능하도록 설계됨
  
  * 다수의 도메인이 하나의 데이터베이스에 등록
  
  * 현재 프로젝트가 첫 번째 사이트임을 나타냄

```python
INSTALLED_APPS = [
    ...,
    # registration
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'dj_rest_auth.registration',
    ...,
]
```

* **`my_api/urls.py`**

```python
urlpatterns = [
    ...,
    path('accounts/signup/', include('dj_rest_auth.registration.urls'))
]
```

* allauth 추가에 대한 migrate

* **`/accounts/signup/` 페이지 확인**
  
  * GET method는 접근 불가
  
  * 회원가입 POST 요청 양식 제공
    
    * email 은 생략 가능

## ▶ Sign up & Login

* 회원가입 요청 후 결과 확인
  
  * **요청에 대한 응답으로 Token 발급**

* 로그인 시에도 동일한 토큰 발급
  
  * 정상적인 로그인 가능

* **발급 받은 토큰은 테스트를 위해 기록**

예) `91f8f0e07920570613c9a2784d479d8de7457b58`

## ▶ Password change

* **`/accounts/password/change/` 기능 확인**
  
  * 로그인 되어 있거나, 인증이 필요한 기능
  
  * DRF 자체 제공 HTML form에서는 토큰을 입력할 수 있는 공간이 없음
  
  * Postman에서 진행

* [참고] Raw data에서 직접 headers 추가 기능

```python
{{
  "ㅗ"
}
  "ㅗ"
}
```

* Postman으로 양식에 맞춰 POST 요청
  
  * body/form-data에 값 입력

* headers에 Token 입력
  
  * `Authorization: Token { your token }` 형식에 맞춰 입력

* 그럼에도 실패한 이유는❓
  
  * 인증 방법이 입증되지 않음

* **`my_api/settings.py`**

```python
REST_FRAMEWORK = {
    # Authentication
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
}
```

* 최종 결과 확인 = 정상적으로 비밀번호 변경 완료

# Permission setting

* 권한 설정 방법 확인
  
  * DRF 공식 문서 > API Guide > Permissions 확인

* 권한 세부 설정
  
  1️⃣ 모든 요청에 대해 인증을 요구하는 설정
  
  2️⃣ 모든 요청에 대해 인증이 없어도 허용하는 설정

* 설정 위치 == 인증 방법을 설정한 곳과 동일
  
  * 우선 모든 요청에 대해 허용 설정

* **`my_api/settings.py`**

```python
REST_FRAMEWORK = {
    # Authentication
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],

    # permission
    'DEFAULT_PERMISSION_CLASSES': [
        # 'rest_framework.permissions.IsAuthenticated',
        # 인증만 하면 모두 허용
        'rest_framework.permissions.AllowAny',
    ],
}
```

## ▶ Aritcle List Read

* **`articles/views.py`**

* 게시글 조회 및 생성 요청 시 인증된 경우만 허용하도록 권한 부여 = decorator 활용❗

```python
# permission Decorators
from rest_framework.decorators import permission_classes
from rest_framework.permissions import IsAuthenticatedent

@api_view(['GET', 'POST'])
@permission_classes([IsAuthenticated])
def article_list(request):
    if request.method == 'GET':
        # articles = Article.objects.all()
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            # serializer.save()
            serializer.save(user=request.user)
            return Response(serializer.data, status=status.HTTP_201_CREATED)
```

* `/articles/` 조회 요청 확인

* 게시글 조회 시 로그인 필요

## ▶ Article Create

* `/articles/` 생성 요청 확인
  
  * Postman으로 진행

* 결과 확인 = 게시글 생성 성공

## ▶ Article Detail Read

* **`articles/1/` 상세 조회 요청 확인**
  
  * headers에 token을 담지 않아도 조회 가능
  
  * 인증 필요 권한 설정을 따로 하지 않았기 때문

## ▶ 정리

1️⃣ 인증 방법 설정

* **`DEFAULT_AUTHENTICATION_CLASSES`**

2️⃣ 권한 설정하기

* **`DEFAULT_PERMISSION_CLASSES`**

3️⃣ 인증 방법, 권한 세부 설정도 가능

* **`@authentication_classes`**

* **`@permission_classes`**

4️⃣ 인증 방법은 다양한 방법이 있으므로 내 서비스에 적합한 방식을 선택

# DRF Auth with Vue

## ▶ Vue server 요청 정상 작동 여부 확인

* 정상 작동하던 게시글 전체 조회 요청이 작동하지 않음
  
  * 401 status code 확인
  
  * *인증되지 않은 사용자이므로 조회 요청이 불가능해진 것*

# SignUp Request

## ▶ SignUp Page

## ▶ SignUp Request

* 회원가입을 완료 시 응답 받을 정보 Token을 store에서 관리할 수 있도록 actions를 활용하여 요청 후, state에 

# drf-spectacular

## ▶ swagger란?

* 스웨거(Swagger)는 개발자가 REST 웹 서비스를 설계, 빌드, 문서화, 소비하는 일을 도와주는 오픈 소스 소프트웨어 프레임워크
  
  * 즉, API를 설계하고 문서화 하는데 도움을 주는 라이브러리

## ▶ 다양한 DRF API

* 스웨거(Swagger)를 생성할 수 있도록 도움을 주는 라이브러리
  
  * drf-spectacular
  
  * https://github.com/tfranzel/drf-spectacular

* 과거에는 다양한 라이브러리가 있었으나 OpenAPI Specification이 3.0으로 업데이트 되며 새 버전을 지원하지 않는 라이브러리들이 있으니 사용 시 유의
  
  * django rest swagger
  
  * drf-yasg (drf-swagger)

## ▶ drf-spectacular

* Open API 3.0을 지원하는 DRF API OpenAPI 생성기

* 지속적인 업데이트와 관리로 최신 Django, DRF 버전 지원

* 설치
  
  * `pip install drf-spectacular`
  
  * `pip freeze > requirements.txt`

* 등록

```python
# my_api/settings.py

INSTALLED_APPS = [
    'drf_spectacular',
]
```

* 기본 설정

```python
# my_api/settings.py

REST_FRAMEWORK = {
    ...,
    # YOUR SETTINGS
    'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
}

SPECTACULAR_SETTINGS = {
    'TITLE': 'Your Project API',
    'DESCRIPTION': 'Your project description',
    'VERSION': '1.0.0',
    'SERVE_INCLUDE_SCHEMA': False,
    # OTHER SETTINGS
```

* URL 설정

```python
# articles/urls.py

from drf_spectacular.views import SpectacularAPIView, SpectacularSwaggerView


urlpatterns = [
    # 필수 작성
    path('schema/', SpectacularAPIView.as_view(), name='schema'),
    # optional UI
    path('swagger/', SpectacularSwaggerView.as_view(url_name='schema'), name='swagger-ui'),
]
```

* `/api/v1/swagger/` 결과 확인 = 정상 작동 확인
