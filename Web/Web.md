# Web

## ✅ 웹 사이트의 구성 요소

* 웹 사이트란 웹 페이지(문서)들의 모음

* **웹 페이지는 글, 그림, 동영상 등 여러가지 정보**를 담고 있으며, **클릭하면 다른 웹 페이로 이동하는 '링크'들이 있음. 웹사이트는 '링크'를 통해 여러 웹 페이지를 연결한 것**

* HTML(구조) + CSS(표현) + Javascript(동작)

## ✅ 웹 사이트와 브라우저

* 웹 사이트는 브라우저를 통해 동작

* 브라우저마다 동작이 약간씩 달라서 문제가 발생 (예. 크롬, 사파리, 웨일, 엣지 등)

* 문제를 해결하기 위해 웹 표준이 등장

* 웹 표준 - 웹에서 표준적으로 사용되는 기술이나 규칙

* 어떤 브라우저든 웹 페이지가 동일하게 보이도록 함(크로스 브라우징)

# HTML

> Hyper Text Markup Language
> 
> **웹 페이지를 작성(구조화)하기 위한 언어**

* **Hyper Text?**
  
  * 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 접근 가능한 텍스트

* **Markup Language?**
  
  * 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어(HTML, Markdown)

## ✅ 기본 구조

* `<html>` - 문서의 최상위 요소

* `<head>` - 문서 메타데이터 요소
  
  * **문서 제목, 인코딩, 스타일, 외부 파일 로딩 등**
  
  * 일반적으로 브라우저에 나타나지 않는 내용

* **`<body>` - 문서 본문 요소**
  
  * **실제 화면 구성과 관련된 내용**

* **들여쓰기(2칸) 중요!!**

### ✔ head 예시

* `<title>` - 브라우저 상단 타이틀

* `<meta>` - 문서 레벨 메타데이터 요소

* **`<link>` - 외부 리소스 연결 요소 (CSS 파일, favicon 등)**

* **`<script>` - 스크립트 요소 (JavaScript 파일/코드 등)**

* **`<style>` - CSS 직접 작성**

* Open Graph Protocol
  
  * 메타 데이터를 표현하는 새로운 규약
    
    * HTML 문서의 메타 데이터를 통해 문서의 정보를 전달
    
    * 메타정보에 해당하는 제목, 설명 등을 쓸 수 있도록 정의

### ✔ 요소(element)

> `<h1>`contents`</h1>`
> 
> 태그와 내용(contents)으로 구성

* **HTML 요소는 시작 태그와 종료 태그 그리고 태그 사이에 위치한 내용으로 구성**
  
  * **내용이 없는 태그들도 존재(닫는 태그 없음)**
    
    * 예) `br`, `hr`, `img`, `input`, `link`, `meta`

* 요소는 중첩될 수 있음
  
  * 여는 태그와 닫는 태그의 쌍을 잘 확인해야 함 - 오류를 반환해주지 않고 그냥 레이아웃이 깨진 상태로 출력된다.

### ✔ 속성(attribute)

> `<a href="https://google.com"></a>`
> 
> **공백은 NO! ""(쌍따옴표) 사용!**

* **태그별로 사용할 수 있는 속성은 다르다.**

* **요소의 시작 태그에 작성하며 보통 이름과 값이 하나의 쌍으로 존재**

* 태그와 상관없이 사용 가능한 속성(HTML Global Attribute)들도 있음
  
  * **`id` - 문서 전체에서 유일한 고유 식별자 지정**
  
  * **`class`** - **공백으로 구분된 해당 요소의 클래스의 목록 (CSS, JS에서 요소를 선택하거나 접근)**
  
  * `data-*` - 페이지에 개인 사용자 정의 데이터를 저장하기 위해 사용
  
  * `style` - inline 스타일
  
  * `title` - 요소에 대한 추가 정보 지정
  
  * `tabindex` - 요소의 탭 순서

### ✔ 시맨틱 태그

> **HTML5에서 의미론적 요소를 담은 태그 (div 태그를 대체하여 사용)**

* 대표적인 태그 목록
  
  * **`header`** - 문서 전체나 섹션의 헤더(머리말 부분)
  
  * **`nav`** - 내비게이션
  
  * **`aside`** - 사이드에 위치한 공간, 메인 컨텐츠와 관련성이 적은 컨텐츠
  
  * **`section`** - 문서의 일반적인 구분, 콘텐츠의 그룹을 표현
  
  * **`article`** - 문서, 페이지, 사이트 안에서 독립적으로 구분되는 영역
  
  * **`footer`** - 문서 전체나 섹션의 푸터(마지막 부분)
    
    ```html
    <header>
        <nav></nav>
    </header>
    <section>
        <article></article>
        <article></article>
    </section>
    <footer></footer>
    ```

* **시맨틱 태그 사용 이유?**
  
  * 개발자 및 사용자 뿐만 아니라 검색엔진 등에 의미 있는 정보의 그룹을 태그로 표현
  
  * 요소의 의미가 명확해지므로 **코드의 가독성을 높이고 유지보수가 용이**
  
  * **검색엔진최적화(SEO)를 위해 메타태그, 시맨틱 태그 등을 통한 마크업을 효과적으로 활용해야 함!**

### ✔ 텍스트로 작성된 코드가 어떻게 웹 사이트가 되는가?

> 렌더링은 웹 사이트 코드를 사용자가 보게 되는 웹 사이트로 바꾸는 과정

* **DOM(Document Object Model) 트리**
  
  * 텍스트 파일인 HTML 문서를 브라우저에서 렌더링하기 위한 구조

## ✅ 문서 구조화

### ✔ 인라인/블록 요소/텍스트 요소

* **인라인 요소 - 글자처럼 취급** (단, 인라인 요소 != 글자)

* **블록 요소 - 한 줄 모두 사용**

* 텍스트 요소 - 마크다운 사용과 유사하다고 생각 (태그 적극 이용)

💥 **인라인 태그**

| 태그                               | 설명                                           |
| -------------------------------- | -------------------------------------------- |
| **`<a></a>`**                    | **링크 - href 속성을 활용하여 다른 URL로 연결하는 하이퍼링크 생성** |
| `<b></b>`<br>`<strong></strong>` | 굵은 글씨 요소                                     |
| `<i></i>`<br/>`<em></em>`        | 기울임 글씨 요소                                    |
| **`<br>`**                       | **텍스트 내에 줄 바꿈 생성**                           |
| **`<img>`**                      | **src 속성을 활용하여 이미지 표현**                      |
| **`<span></span>`**              | **의미없는 인라인 컨테이너**                            |

💥 **그룹 태그**

| 태그                              | 설명                                 |
| ------------------------------- | ---------------------------------- |
| **`<p></p>`**                   | **하나의 문단**                         |
| `<hr>`                          | 문단 레벨 요소에서의 주제의 분리를 의미하며 수평선으로 표현됨 |
| **`<ol></ol>` <br>`<ul></ul>`** | **순서가 있는 리스트 <br>순서가 없는 리스트**      |
| `<pre></pre>`                   | HTML에 작성한 내용을 그대로 표현               |
| `<blockquote></blockquote>`     | 텍스트가 긴 인용문. 주로 들여쓰기를 한 것으로 표현      |
| **`<div></div>`**               | **의미 없는 블록 레벨 컨테이너**               |

### ✔ form

* **`<form>`은 정보(데이터)를 서버에 제출하기 위해 사용하는 태그**
  
  * 예) 로그인, 게시글 정보 등의 데이터를 서버에 보내기 위함

* 기본 속성
  
  * **action - form 을 처리할 서버의 URL (데이터를 보낼 곳)**
  
  * **method - form 을 제출할 때 사용할 HTTP 메서드 (GET or POST)**
  
  * **enctype: method 가 post 인 경우 데이터의 유형**
    
    * 기본값(텍스트) - `application/x-www-form-urlencoded`
    
    * 파일 전송 시 - `multipart/form-data`

### ✔ input

* **다양한 타입을 가지는 입력 데이터 유형과 위젯이 제공됨**

* **대표적인 속성**
  
  * `name` - form control 에 적용되는 이름 (이름/값 페어로 전송)
  
  * `value` - form control 에 적용되는 값 (이름/값 페어로 전송)
  
  * 단일 속성 - `required`, `readonly`, `autofocus`, `autocomplete`, `disabled` 등

* **input label**
  
  * `label`을 클릭하여 input 자체의 초점을 맞추거나 활성화 가능
    
    * 사용자는 선택할 수 있는 영역이 늘어나 웹/모바일 환경에서 편하게 사용
    
    * label과 input 입력의 관계가 시각적 뿐만 아니라 화면리더기에도 label을 읽어 쉽게 내용을 확인 할 수 있도록 함
  
  * **`<input>` 에 id 속성을, `<label>` 에는 for 속성을 활용하여 상호 연관을 시킴**

* **1️⃣ 일반 유형**
  
  * `text` - 일반 텍스트 입력
  
  * `password` - 입력 시 값이 보이지 않고 문자를 특수기호(*)로 표현
  
  * `email` - 이메일 형식이 아닌 경우 form 제출 불가
  
  * `number` - min, max, step 속성을 활용하여 숫자 범위 설정 가능
  
  * `file` - accept 속성을 활용하여 파일 타입 지정 가능

* **2️⃣ 항목 중 선택 유형**
  
  * **일반적으로 `<label>` 과 함께 사용하여 선택 항목을 작성**
  
  * **동일 항목에 대하여는 name을 지정하고 선택된 항목에 대한 value를 지정해야 함**
    
    * `checkbox` - 다중 선택
    
    * `radio` - 단일 선택

* **3️⃣ 기타 유형**
  
  * 다양한 종류의 input을 위한 picker 제공
    
    * `color` - color picker
    
    * `date` - date picker
  
  * `hidden input` - 사용자 입력을 받지 않고 서버에 전송되어야 하는 값을 설정

# CSS

> **Cascading Style Sheets - 스타일을 지정하기 위한 언어**

## ✅ CSS 구문

```css
h1 {
    color: blue;
    front-size: 15px;
}
```

* **선택자를 통해 스타일을 지정할 HTML 요소를 선택**

* 선택한 요소의 속성, 속성에 부여할 값(쌍)을 부여!
  
  * **속성 - 어떤 스타일 기능을 변경할지 결정**
  
  * **값 - 어떻게 스타일 기능을 변경할지 결정**

## ✅ CSS 정의 방법

### 1️⃣ 인라인(inline)

* 해당 태그에 직접 style 속성을 활용

```html
<h1 style="color: blueviolet; font-size: 100px;">오오오오오</h1>
```

### 2️⃣ 내부 참조

* **`<head>` 태그 내에 `<style>` 에 지정**

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h1 {
            color: blue;
            font-size: 20px;
        }
    </style>
</head>
```

### 3️⃣ 외부 참조

* **외부 CSS 파일을 `<head>` 내 `<link>` 를 통해 불러오기**

```html
<link rel="stylesheet" href="mystyle.css">
```

```css
p {
    color: pink;
    font-size: 16px;
}
```

## ✅ 선택자

* 기본 선택자
  
  * 전체 선택자 `*`
  
  * **요소 선택자 - HTML 태그를 직접 선택**
  
  * **클래스 선택자 - 마침표(`.`) 문자로 시작하며 해당 클래스가 적용된 항목을 선택**
  
  * **아이디(id) 선택자**
    
    * **`#` 문자로 시작하며 해당 아이디가 적용된 항목을 선택**
    
    * 일반적으로 하나의 문서에 1번만 사용 = 단일 id 사용을 권장

```html
<style>
      * {
        color: red;
      }

      h2 {
        color: orange;
      }

      h3,
      h4 {
        font-size: 10px
      }

      .green {
        color: green;
      }

      #puple {
        color: purple;
      }

      .box > p {
        font-size: 30px;
      }

      .box p {
        color: blue;
      }
    </style>
</head>
<body>
  <h1 class="green">SSAFY</h1>
  <h2>선택자 연습</h2>
  <div class="green box">
    box contents
    <div>
        <p>지역 목록</p>
        <ul>
            <li>서울</li>
            <li id="puple">인천</li>
            <li>강원</li>
            <li>경기</li>
        </ul>
    </div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Itaque reiciendis voluptatem quas nulla odit deserunt, necessitatibus doloribus! Velit alias culpa mollitia asperiores, sapiente est nobis sint illum beatae voluptas veritatis.</p>
  </div>
  <h3>HELLO</h3>
  <h4>CSS</h4>  
</body>
```

### ✔ 적용 우선순위 (cascading order)

* **범위가 좁을수록 적용 우선순위가 높다‼**

① 중요도 - `!importatnt`

**② 우선순위 - 인라인 > id > class > 속성 > pseudo class > 요소, pseudo element**

③ CSS 파일 로딩 순서 (아래에 있는 것부터 우선순위가 높다.)

* **실습!**

```html
<p>1</p>
<p class="blue">2</p>
<p class="blue green">3</p>
<p class="green blue">4</p>
<p id="red" class="blue">5</p>
<h2 id="red" class="blue">6</h2>
<p id="red" class="blue" style="color: yellow;">7</p>
<h2 id="red" class="blue" style="color: yellow;">8</h2>
```

```css
h2 {
    color: darkviolet !important;
}

p {
    color: orange;
}

.blue {
    color: blue;
}

.green {
    color: green;
}

#red {
    color: red;
}
```

### ✔ 상속

> CSS는 상속을 통해 **부모 요소의 속성을 자식에게 상속한다❗**

* **상속 되는 것 예시**
  
  * **Text 요소**(font, color, text-align), **opacity, visibility** 등

* **상속 되지 않는 것 예시**
  
  * **Box model 요소**(width, height, margin, padding, border, box-sizing, display), **position 요소**(position, top, right, bottom, left, z-index)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    p {
      /* 상속됨 */
      color: red; 
      /* 상속 안됨 */
      border: 3px solid black;
    }

    span {
      /* border: 3px solid blue; */
    }
  </style>
</head>
<body>
  <p>안녕하세요 <span>테스트</span> 입니다.</p>
</body>
</html>
```

## ✅ CSS 기본 스타일

### ✔ 크기 단위

* `px` - 픽셀 (고정적인 단위)

* `%` - 백분율 (가변적인 레이아웃에서 사용)

* **`em`**
  
  * **(바로 위 부모 요소에 대한) 상속의 영향을 받음**
  
  * 배수 단위, 요소에 지정된 사이즈에 상대적인 사이즈를 가짐

* **`rem`**
  
  * **상속의 영향을 받지 않음**
  
  * **최상위 요소(html)의 사이즈를 기준으로 배수 단위 사이즈를 가짐**
    
    ```html
    <body>
      <ul class="font-big">
        <li class="em">2em</li>
        <li class="rem">2rem</li>
        <li>no class</li>
      </ul>    
    </body>
    ```
    
    ```css
    .font-big {
      font-size: 36px
    }
    .em {
      font-size: 2em;
    }
    .rem {
      font-size: 2rem;
    }
    ```

* **viewport (크기 단위)**
  
  * 웹 페이지를 **방문한 유저에게 바로 보이게 되는 웹 컨텐츠의 영역(디바이스 화면)**
  
  * **디바이스의 viewport를 기준으로 상대적인 사이즈가 결정됨**
    
    * `px` - 브라우저의 크기를 변경해도 그대로
    
    * **`vw` - 브라우저의 크기에 따라 크기가 변함**

### ✔ 색상 단위

* 색상 키워드 - red, blue, black 과 같은 특정 색을 직접 글자로 표현

* **RGB 색상 - 16진수 표기법 or 함수형 표기법을 사용해서 특정 색을 표현**
  
  **`rgb(0, 255, 0);`** **`rgba(0, 0, 0, 0.5);`** a는 투명도(alpha)를 의미

* HSL 색상 - 색상, 채도, 명도를 통해 특정 색을 표현
  
  `hsl(0, 100%, 50%);`

### ✔ 결합자 (combinators)‼

* **자손 결합자 (`공백`) - 선택자A 하위의 모든(자손 전체) 선택자B 요소**

* **자식 결합자 (`>`) - 선택자A 바로 아래의(자식만) 선택자B 요소**

* **일반 형제 결합자 (`~`) - 선택자A의 형제 요소 중 뒤에 위치하는 선택자B 요소를 모두 선택**

* **인접 형제 결합자 (`+`) - 선택자A의 형제 요소 중 바로 뒤에 위치하는 선택자B 요소를 선택**

```html
<body>
  <!-- 일반 형제 결합자 -->
  <span>1</span>
  <p>2</p>
  <b>3</b>
  <span>4</span>
  <b>5</b>
  <span>6</span>
</body>
```

```css
/* p 요소의 형제 요소 중에서 p 요소 뒤에 위치하는 span 요소를 모두 선택 */
p ~ span {
  color: red;
}
```

```html
<body>
  <!-- 인접 형제 결합자 -->
  <p>1</p> 
  <p>2</p>
  <p>3</p>
  <div>4</div>
  <div class="title">
    <p>5</p>
  </div>
  <b>얘가 있으면 아래 리스트가 파랗게 안됨!!!</b>
  <ul>
    <li>6</li>
    <li>7</li>
    <li>8</li>
  </ul>
</body>
```

```css
/* p 의 형제 요소 중 p 바로 뒤에 위치하는 p 요소를 선택 */
    p + p {
      color: red;
    }

    /* title 클래스 요소 중 title 클래스 바로 뒤에 위치하는 ul 요소 선택 */
    .title + ul {
      color: blue;
    }
```

## ✅ Box model

> 모든 요소는 네모(박스 모델)이고,
> 
> **위에서부터 아래로(block), 왼쪽(좌측 상단)에서 오른쪽으로(inline) 쌓인다.**

* 하나의 박스는 네 부분(영역)으로 이루어진다.
  
  * **margin - 테두리 바깥의 외부 여백 배경색을 지정할 수 없다.**
  
  * **border - 테두리 영역**
  
  * **padding - 테두리 안쪽의 내부 여백 요소에 적용된 배경색, 이미지는 padding까지 적용**
  
  * **content - 글이나 이미지 등 요소의 실제 내용**

* **box sizing‼**
  
  * 기준에 따라 사이즈의 크기는 달라진다.
  
  * **box-sizing의 기준을 border-box로 설정하면 content 크기로 width 설정!**

* **margin/padding shorthand❗**
  
  * magin: 10px; => 상하좌우 전체 적용
  
  * margin: 10px 20px; => 상하 / 좌우 적용
  
  * **margin: 10px 20px 30px; => 상 / 좌우 / 하 적용**
  
  * **margin: 10px 20px 30px 40px; => 상 / 우 / 하 / 좌 적용**

## ✅ CSS Display

> 모든 요소는 네모(박스모델)이고, 좌측상단에 배치
> 
> **display에 따라 크기와 배치가 달라진다.**

* **display: block**
  
  * 줄 바꿈이 일어나는 요소
  
  * **화면 크기 전체의 가로 폭을 차지**
  
  * block 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있음
    
    * **`div`, `ul`, `ol`, `li`, `p`, `hr`, `form` 등**

* **display: inline**
  
  * 줄 바꿈이 일어나지 않는 행의 일부 요소
  
  * **content 너비만큼 가로 폭을 차지**
  
  * **width, height, margin-top, margin-bottom 지정 불가❗**
    
    = 상하여백은 `line-height`로 지정
    
    * **`span`, `a`, `img`, `input`, `label`, `b`, `em`, `i`, `strong` 등**

* **display: inline-block**
  
  * block과 inline 레벨 요소의 특징을 모두 가짐
  
  * inline 처럼 한 줄에 표시 가능. block처럼 width, height, margin 속성을 모두 지정

* **display: none**
  
  * **해당 요소를 화면에 표시하지 않고, 공간조차 부여하지 않는다.**
  * **visibility: hidden은 해당 요소가 공간은 차지하나 화면에 표시만 하지 않는다.**

## ✅ CSS Position‼

> 문서 상에서 요소의 위치를 지정

* **`static`** - 모든 태그의 기본 값(기준 위치)
  
  * **일반적인 요소의 배치 순서에 따름**(좌측 상단)
  
  * 부모 요소 내에서 배치될 때는 부모 요소의 위치를 기준으로 배치

* **좌표 프로퍼티(top, bottom, left, right)를 사용하여 이동**
  
  * **`relative`** - 상대 위치
    
    * **자기 자신의 static 위치를 기준으로 이동**
    
    * normal position 대비 offset
  
  * **`absolute`** - 절대 위치
    
    * 요소를 일반적인 **문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음**
    
    * static이 아닌 가장 가까이 있는 **부모/조상 요소를 기준으로 이동** (없으면 브라우저 화면(body 태그) 기준으로 이동)
    
    * 사용법 - 가까이 있는 부모를 relative로 만든 후 이동시키는 것이 일반적
  
  * **`fixed`** - 고정 위치
    
    * 요소를 일반적인 **문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음**
    
    * 부모 요소와 관계없이 **viewport(화면)를 기준으로 이동**
    
    * **스크롤 하더라도 항상 같은 곳에 위치**
  
  * **`sticky`** - **스크롤 이동에 따라 static -> fixed로 변경**
    
    * 평소에 문서 안에서 position: static 상태와 같이 일반적인 흐름이지만 스크롤 위치가 임계점에 이르면 position: fixed와 같이 박스를 화면에 고정할 수 있음
    * **relative처럼 원래 자리 남겨두면서 화면을 벗어나면 fixed처럼 동작!**
