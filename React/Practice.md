# Page Routing

> **Routing 이란 어떤 네트워크 내에서 통신 데이터를 보낼 경로를 선택하는 일련의 과정**

* `Router` : 데이터의 경로를 실시간으로 지정해주는 역할

* `Routing` : 경로를 정해주는 행위 자체와 그런 과정들을 다 포함하여 일컫는 말

* **`PAGE ROUTING` : Client 가 웹 서버에 page 를 요청하면 서버가 경로에 알맞는 페이지를 응답**

* **`MPA(Multipage Application)`**
  
  * 웹 서버가 여러 페이지를 준비해두었다가 요청이 들어오면 적절한 페이지를 응답하는 방식

* **`SPA(Singlepage Application)`**
  
  * **웹 서버가 하나의 페이지만을 가지고 있다가 요청이 들어오면 페이지 하나만을 응답하는 방식**
  
  * **다른 페이지로 이동은 React App이 페이지를 업데이트 함**
    
    * 서버와의 통신 시간을 없앴기 때문에 속도가 빠름 = 새로고침 없이 페이지 이동
    
    * 서버한테는 필요한 데이터만 받아서 페이지를 구성함
  
  * **CSR (Client Side Rendering)**
    
    * 클라이언트 측에서 직접 페이지를 렌더링함

## ▶️ React Router

> **React Router 공식 문서 확인하기!**

* **설치**

```bash
npm install react-router-dom localforage match-sorter sort-by
```

* **컴포넌트와 URL 매칭시키기**
  
  * **① `<BrowserRouter> </BrowserRouter>` 로 감싸면 ROUTING 가능함**
  
  * **② URL 이 바뀌는 부분은 `<Routes> <Routes />` 로 감싸주어야 함**
  
  * **③ 컴포넌트와 URL 경로를 매칭하는 방법**
    
    * **`<Route path="url경로" element={<component />} />`**

```javascript
import "./App.css";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
...

function App() {
  return (
    // 브라우저의 URL 과 매핑시키는 방법 = 감싸주기
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        {/* URL이 바뀌는 부분 */}
        <Routes>
          {/* URL 경로와 컴포넌트를 매칭 시키는 방법 */}
          <Route path="/" element={<Home />} />
          <Route path="/new" element={<New />} />
          <Route path="/edit" element={<Edit />} />
          <Route path="/diary" element={<Diary />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

* **CSR 방식의 URL 이동**
  
  * **`<Link to={"URL"}>COMPONENT</Link>`**
  
  * **자식 컴포넌트로 넣어두면 링크를 통한 이동 가능!**

```javascript
import { Link } from "react-router-dom";

// CSR 방식으로 페이지 이동
const RouteTest = () => {
  return (
    <>
      <Link to={"/"}>HOME</Link>
      <br />
      <Link to={"/diary"}>DIARY</Link>
      <br />
      <Link to={"/new"}>NEW</Link>
      <br />
      <Link to={"/edit"}>EDIT</Link>
    </>
  );
};

export default RouteTest;
```

```javascript
import "./App.css";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import RouteTest from "./components/RouteTest";
...

function App() {
  return (
    // 브라우저의 URL 과 매핑시키는 방법 = 감싸주기
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        ...
        <RouteTest />
      </div>
    </BrowserRouter>
  );
}

export default App;
```

## ▶️ React Router V6

> **React 에서 CSR 기반의 페이지 라우팅을 할 수 있게 해주는 라이브러리**

### 📍 React Router Dom의 유용한 기능

1️⃣ **Path Variable : `useParams`**

* 특정 페이지의 상세 페이지를 구현할 때 사용
  
  * 일기 데이터 중 특정 일기를 보여줘야할 때

```javascript
import { useParams } from "react-router-dom";

const Diary = () => {
  const { id } = useParams();

  return (
    <div>
      <h1>Diary</h1>
      <p>이 곳은 일기 상세 페이지 입니다.</p>
    </div>
  );
};

export default Diary;
```

```javascript
import "./App.css";
import { BrowserRouter, Route, Routes } from "react-router-dom";
...
import Diary from "./pages/Diary";

function App() {
  return (
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        {/* URL이 바뀌는 부분 */}
        <Routes>
          {/* URL 경로와 컴포넌트를 매칭 시키는 방법 */}
          ...
          <Route path="/diary/:id" element={<Diary />} />
        </Routes>
        <RouteTest />
      </div>
    </BrowserRouter>
  );
}

export default App;
```

2️⃣ **Query String : `useSearchParams`**

* **Query : 웹 페이지에 데이터를 전달하는 가장 간단한 방법**
  
  * 예) `/edit?id=10&mode=dark`
  
  * **QueryString은 실시간으로 변경할 수 있음**

```javascript
import { useSearchParams } from "react-router-dom";

const Edit = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const id = searchParams.get("id");
  const mode = searchParams.get("mode");

  return (
    <div>
      <h1>Edit</h1>
      <p>이 곳은 일기 수정 페이지 입니다.</p>
      <button onClick={() => setSearchParams({ who: "ukey" })}>
        QueryString 바꾸기
      </button>
    </div>
  );
};

export default Edit;
```

3️⃣ **Page Moving : `useNavigate`**

* **경로를 개발자가 원하는 대로(강제로) 옮길 수 있음**
  
  * HOME 으로 보내기 : `navigate("/home")`
  
  * **뒤로 가기 : `navigate(-1)`**
  
  * 예) 로그인이 필요한 페이지를 사용자가 비로그인 상태로 접속하려고 할 때 로그인 페이지로 강제로 보낼 수 있음

```javascript
import { useNavigate, useSearchParams } from "react-router-dom";

const Edit = () => {
  const navigate = useNavigate();

  ...

  return (
    <div>
      <h1>Edit</h1>
      <p>이 곳은 일기 수정 페이지 입니다.</p>
      ...
      <button
        onClick={() => {
          navigate("/home");
        }}
      >
        HOME으로 가기
      </button>
    </div>
  );
};

export default Edit;
```

# 프로젝트 시작 전 설정 세팅

**① 폰트 세팅**

* Google Web Fonts를 활용하여 프로젝트에 사용되는 폰트 세팅
  
  * 원하는 폰트 선택 후 `App.css` 파일 최상단에 import
  
  * CSS 로 적용하기 : `font-family`

```css
/* fonts 불러오기 */
@import url("https://fonts.googleapis.com/css2?family=Nanum+Pen+Script&family=Yeon+Sung&display=swap");

.App {
  padding: 20px;

  font-family: "Nanum Pen Script", cursive;
  font-family: "Yeon Sung", cursive;
}
```

**② 레이아웃 세팅**

* 모든 페이지에 반영되는 레이아웃 세팅
  
  * 공통으로 갖는 CSS 를 미리 적용하기

* **`@media` : 반응형 웹 사이트 만들 때 많이 사용!**

```css
body {
  background-color: #f6f6f6;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Nanum Pen Script";
  min-height: 100vh;
  margin: 0px;
}

@media (min-width: 650px) {
  .App {
    width: 640px;
  }
}

@media (max-width: 650px) {
  .App {
    width: 90vw;
  }
}

#root {
  background-color: white;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.App {
  min-height: 100vh;
  padding-left: 20px;
  padding-right: 20px;
}
```

**③ 이미지 자료 세팅**

* 감정 이미지들을 프로젝트에서 불러와 사용할 수 있는 환경 세팅
  
  * **`public/assets` 디렉토리에 폴더 생성하여 이미지 파일 넣기**

* 이미지 파일 불러오기 : `<img src={process.env.PUBLIC_URL + '/aseets/<file_name>'} />`
  
  * *안될 경우 새로 변수 지정한 진행*

```javascript
import "./App.css";
...

function App() {
  // 이미지 파일이 제대로 불러오지 못할 경우 
  const env = process.env;
  env.PUBLIC_URL = env.PUBLIC_URL || "";
  return (
    // 브라우저의 URL 과 매핑시키는 방법 = 감싸주기
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        {/* 이미지 불러오기 */}
        <img src={process.env.PUBLIC_URL + `/assets/emotion1.png`} />
        <img src={process.env.PUBLIC_URL + `/assets/emotion2.png`} />
        <img src={process.env.PUBLIC_URL + `/assets/emotion3.png`} />
        <img src={process.env.PUBLIC_URL + `/assets/emotion4.png`} />
        <img src={process.env.PUBLIC_URL + `/assets/emotion5.png`} />
        ...
    </BrowserRouter>
  );
}

export default App;
```

**④ 공통 컴포넌트 세팅**

* 모든 페이지에 공통으로 사용되는 버튼, 헤더(Navi) 컴포넌트 세팅

* **버튼 만들기 - button component 별도 생성**
  
  * 어떤 기준으로 얼마만큼 변화하는가? 패턴을 찾는 것이 중요!
  
  * 예)
    
    * 작성완료
      
      * type: POSITIVE
      
      * text: "작성완료"
      
      * onClick: ?
    
    * 수정하기
      
      * type: DEFAULT (or undefined)
      
      * text: "수정하기"
      
      * onClick: ?
    
    * 삭제하기
      
      * type: NEGATIVE
      
      * text: "삭제하기"
      
      * onClick: ?

```javascript
const MyButton = ({ text, type, onClick }) => {
  // type 이 다른 것이 들어오더라도 default 로 강제
  const btnType = ["positive", "negative"].includes(type) ? type : "default";

  return (
    <button
      className={["MyButton", `MyButton_${btnType}`].join(" ")}
      onClick={onClick}
    >
      {text}
    </button>
  );
};

MyButton.defaultProps = {
  type: "default",
};

export default MyButton;
```

```javascript
import "./App.css";
...
// COMPONENTS
import MyButton from "./components/MyButton";

function App() {
  return (
    // 브라우저의 URL 과 매핑시키는 방법 = 감싸주기
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        <MyButton
          text={"버튼"}
          onClick={() => alert("버튼 클릭")}
          type={"positive"}
        />
        <MyButton
          text={"버튼"}
          onClick={() => alert("버튼 클릭")}
          type={"negative"}
        />
        <MyButton text={"버튼"} onClick={() => alert("버튼 클릭")} />
        ...
      </div>
    </BrowserRouter>
  );
}

export default App;
```

* **헤더 : 왼쪽 자식(버튼) + 헤드 텍스트 + 오른쪽 자식(버튼)**

> **leftChild + headText + rightChild**

```javascript
const MyHeader = ({ headText, leftChild, rightChild }) => {
  return (
    <header>
      <div className="head_btn_left">{leftChild}</div>
      <div className="head_text">{headText}</div>
      <div className="head_btn_right">{rightChild}</div>
    </header>
  );
};

export default MyHeader;
```

```javascript
import "./App.css";
...
// COMPONENTS
import MyButton from "./components/MyButton";
import MyHeader from "./components/MyHeader";

function App() {
  return (
    // 브라우저의 URL 과 매핑시키는 방법 = 감싸주기
    <BrowserRouter>
      <div className="App">
        <MyHeader
          headText={"App"}
          leftChild={
            <MyButton text={"왼쪽 버튼"} onClick={() => alert("왼쪽 클릭")} />
          }
          rightChild={
            <MyButton
              text={"오른쪽 버튼"}
              onClick={() => alert("오른쪽 클릭")}
            />
          }
        />
        <h2>App.js</h2>
        ...
      </div>
    </BrowserRouter>
  );
}

export default App;
```

**⑤ 상태 관리 세팅**

> **프로젝트 전반적으로 사용될 일기 데이터 state 관리 로직 작성**

* **컴포넌트별 상태**
  
  * App : 일기 데이터 state
  
  * Home : 일기 리스트
  
  * New : 일기 생성 로직
  
  * Edit : 일기 수정 로직
  
  * Diary : 일기 하나의 상세 데이터

```javascript
import { useReducer, useRef } from "react";

...

// reducer 함수
const reducer = (state, action) => {
  let newState = [];
  switch (action.type) {
    case "INIT": {
      return action.data;
    }
    case "CREATE": {
      // const newItem = {
      //   ...action.data,
      // };
      newState = [action.data, ...state];
      break;
    }
    case "REMOVE": {
      newState = state.filter((it) => it.id !== action.targetId);
      break;
    }
    case "EDIT": {
      newState = state.map((it) =>
        it.id === action.data.id ? { ...action.data } : it
      );
      break;
    }
    default:
      return state;
  }
  return newState;
};

function App() {
  // state 생성
  const [data, dispatch] = useReducer(reducer, []);

  const dataId = useRef(0);
  // CREATE - 일기 생성 (시간, 내용, 감정)
  const onCreate = (date, content, emotion) => {
    dispatch({
      type: "CREATE",
      data: {
        id: dataId.current,
        // 생성 시간
        date: new Date(date).getTime(),
        content,
        emotion,
      },
    });
    dataId.current += 1;
  };
  // REMOVE
  const onRemove = (targetId) => {
    dispatch({ type: "REMOVE", targetId });
  };
  // EDIT - 일기의 모든 부분을 수정
  const onEdit = (targetId, date, content, emotion) => {
    dispatch({
      type: "EDIT",
      data: {
        id: targetId,
        date: new Date(date).getTime(),
        content,
        emotion,
      },
    });
  };

  return (
    ...
  );
}

export default App;
```

**⑥ 프로젝트 State Context 세팅**

> **일기 데이터 state를 공급할 context를 생성하고 Provider로 공급**

```javascript
import React, { useReducer, useRef } from "react";

...

// reducer 함수
const reducer = (state, action) => {
  ...
};

export const DiaryStateContext = React.createContext();

function App() {
  ...
  };

  return (
    <DiaryStateContext.Provider value={data}>
      {/* 브라우저의 URL 과 매핑시키는 방법 = 감싸주기 */}
      <BrowserRouter>
        ...
      </BrowserRouter>
    </DiaryStateContext.Provider>
  );
}

export default App;
```

**⑦ 프로젝트 Dispatch Context 세팅**

> **일기 데이터 state의 Dispatch 함수들을 공급할 context를 생성하고 Provider로 공급**

```javascript
import React, { useReducer, useRef } from "react";

...

// reducer 함수
const reducer = (state, action) => {
  ...
};

export const DiaryStateContext = React.createContext();
export const DiaryDispatchContext = React.createContext();

function App() {
  ...

  return (
    <DiaryStateContext.Provider value={data}>
      <DiaryDispatchContext.Provider value={{ onCreate, onEdit, onRemove }}>
        {/* 브라우저의 URL 과 매핑시키는 방법 = 감싸주기 */}
        <BrowserRouter>
          <div className="App">
            ...
          </div>
        </BrowserRouter>
      </DiaryDispatchContext.Provider>
    </DiaryStateContext.Provider>
  );
}

export default App;
```
