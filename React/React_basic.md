# React 가 필요한 이유

## 1️⃣ 중복되는 코드가 많음

* *수정이 필요한 경우 모든 페이지의 중복되는 코드를 수정해야 함*

* 해결 방법
  
  * **헤더, 네브, 풋터 등 중복되는 코드를 미리 컴포넌트로 만들어 사용 = 컴포넌트화 방식**

* **React는 Component 기반의 UI 라이브러리**

## 2️⃣ 명령형 프로그래밍 - 절차를 하나하나 다 나열 해야함

* 결과를 표시할 요소를 가져오고 현재 결과값을 함수를 통해 변형시킴

* 코드가 길어짐

* 해결 방법
  
  * **선언형 프로그래밍 - 그냥 목적을 바로 말하여 간단하게 작성할 수 있도록 해야 함**
  
  * 예) Plus를 누르면, result값에 저장한다. Minus를 누르면 반대로 한다.

## 3️⃣ Virtual DOM 사용

> **브라우저가 실제로 사용하는 객체 = Document Object Model**

* JavaScript의 실행으로 인해 DOM 변화가 급격히 많아질 수 있는데 Virtual DOM을 사용하면 한 번에 업데이트 시켜 연산의 수를 확 줄여줄 수 있음

# React 앱 생성

## ▶️ React.js

> **Node 기반의 JavaScript UI 라이브러리**

* **`Webpack`**
  
  * 다수의 자바스크립트 파일을 하나의 파일로 합쳐주는 모듈 번들 라이브러리

* **`Bable`**
  
  * JSX 등의 쉽고 직관적인 자바스크립트 문법을 사용할 수 있도록 해주는 라이브러리

* **`npx create-react-app <filename>` - 1회 설치만 할 경우 npx 를 사용**

* **`npm start` - react 서버 실행하기**

* **`/src/App.js`** = 메인 페이지

* *이상하게 보이는 문법*
  
  * JavaScript & HTML 문법을 함께 사용하는 것을 `JSX` 라고 한다.

```javascript
import './App.css';

function App() {
  let name = '이동욱'

  return (
    <div className="App">
      <header className="App-header">
        <h2>안녕 리액트 {name}</h2>
      </header>
    </div>
  );
}

export default App;
```

# JSX

> HTML with JavaScript
