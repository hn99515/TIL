# React 가 필요한 이유

> **React JS 는 UI의 Interactive의 원동력
> ReactDOM은 React Element를 가져다가 HTML로 바꾸는 역할**

## 1️⃣ 중복되는 코드가 많음

- _수정이 필요한 경우 모든 페이지의 중복되는 코드를 수정해야 함_

- 해결 방법

  - **헤더, 네브, 풋터 등 중복되는 코드를 미리 컴포넌트로 만들어 사용 = 컴포넌트화 방식**

- **React는 Component 기반의 UI 라이브러리**

  - 화면을 만들기 위한 기능들을 모두 모아둔 것

  - 제어권한이 개발자에게 있는 것이 라이브러리

## 2️⃣ 명령형 프로그래밍 - 절차를 하나하나 다 나열 해야함

- 결과를 표시할 요소를 가져오고 현재 결과값을 함수를 통해 변형시킴

- 코드가 길어짐

- 해결 방법

  - **선언형 프로그래밍 - 그냥 목적을 바로 말하여 간단하게 작성할 수 있도록 해야 함**

  - 예) Plus를 누르면, result값에 저장한다. Minus를 누르면 반대로 한다.

## 3️⃣ Virtual DOM 사용

> **브라우저가 실제로 사용하는 객체 = Document Object Model**

- JavaScript의 실행으로 인해 DOM 변화가 급격히 많아질 수 있는데 Virtual DOM을 사용하면 한 번에 업데이트 시켜 연산의 수를 확 줄여줄 수 있음

# React 의 장점

## 1️⃣ 빠른 업데이트 & 렌더링 속도

- 업데이트란 화면에 나오는 내용들이 빠르게 나오는 정도를 의미

  - Virtual DOM 을 사용 : DOM을 직접 수정하는 것이 아니라 업데이트 해야 할 최소한의 부분만 찾아서 업데이트 함

## 2️⃣ Component-Based

- 레고 블록 조립하듯 컴포넌트들을 모아서 개발 : 하나의 컴포넌트를 여러 위치에 사용 가능

  - 재사용성이 높아짐 : 해당 소프트웨어 or 모듈이 다른 곳에서도 쉽게 사용 가능하다는 의미

    - 개발 기간이 단축됨

    - 유지보수가 용이함

## 3️⃣ 든든한 지원군

- Meta의 오픈 소스로서 꾸준히 유지보수 되고 있음

- 활발한 지식공유 & 커뮤니티 : gitHub & stackoverflow

- Mobile 로 넘어가기도 쉬움 : `React Native`

# React 의 단점

## 1️⃣ 방대한 학습량

- Virtual DOM? JSX? Component? State? Props?

- 계속해서 업데이트되는 중이므로 변화하는게 많고 새로 공부해야 함

## 2️⃣ 높은 상태관리 복잡도

- **`state`** : 성능 최적화를 위해 관리가 필요

- 상태 관리의 기본 개념을 제대로 이해해야 함

# React 앱 생성

## ▶️ React.js

> **Node 기반의 JavaScript UI 라이브러리**

- **`Webpack`**

  - 다수의 자바스크립트 파일을 하나의 파일로 합쳐주는 모듈 번들 라이브러리

- **`Bable`**

  - JSX 등의 쉽고 직관적인 자바스크립트 문법을 사용할 수 있도록 해주는 라이브러리

- **`npx create-react-app <filename>` - 1회 설치만 할 경우 npx 를 사용**

- **`npm start` - react 서버 실행하기**

- **`/src/App.js`** = 메인 페이지

- _이상하게 보이는 문법_

  - JavaScript & HTML 문법을 함께 사용하는 것을 `JSX` 라고 한다.

```javascript
import "./App.css";

function App() {
  let name = "이동욱";

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

> **XML/HTML with JavaScript
> Babel : JSX 코드를 브라우저가 이해할 수 있도록 변환(React JS 언어로)해주는 것**

- **Component 를 만드는 데 아주 유용한 문법**

  - 첫 번째 문자는 반드시 대문자로 작성해야 함! (HTML 요소라면 소문자로 작성해도 됨)

- **`React.createElement()` 의 결과로 객체가 생성됨 = React element**

  - type, [props], [...children] 의 파라미터(유형, 속성, 자식 엘리먼트)를 가짐

- **장점**

  - 간결한 코드 작성 가능

  - 가독성 향상

    - 버그를 발견하기 쉬움

  - Injection Attacks 방어 (해킹 방어에 유리)

- **닫힌 태그가 반드시 필요!**

  - 단, `a`태그 나 `image` 태그 등 닫힌 태그가 필요없었던 태그들은 self closed tag로 표현

  - 예) `<a />`, `<image />`등

- **최상위 태그가 반드시 1개 필요한데 원하지 않는다면 `Fragment`를 활용**

```javascript
import './App.css';
// 생성한 파일 불러오기
import MyHeader from './MyHeader';
// fragment 를 사용하기 위함
import React from react;

function App() {
  let name = '이동욱'

  return (
  // 최상위 태그를 삭제할 경우 에러가 발생 (반드시 1개의 최상위 태그가 있어야 한다.)
  <React.Fragment>
      {/* 생성한 파일 사용하기 */}
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트 {name}</h2>
      </header>
  </React.Fragment>
  );
}

export default App;
```

- **CSS 적용하기** : `App.css` & `App.js` 수정

```javascript
// CSS 불러오기
import "./App.css";
// 생성한 파일 불러오기
import MyHeader from "./MyHeader";

function App() {
  let name = "이동욱";

  return (
    // 최상위 태그를 삭재할 경우 에러가 발생 (반드시 1개의 최상위 태그가 있어야 한다.)
    <div className="App">
      {/* 생성한 파일 사용하기 */}
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트 {name}</h2>
        <b id="bold_text">React.js</b>
      </header>
    </div>
  );
}

export default App;
```

```css
.App {
  background-color: black;
}

h2 {
  color: red;
}

#bold_text {
  color: green;
}
```

- **CSS 를 Inline 으로 적용하기**

  - CSS 파일 없이도 적용 가능

```javascript
// 생성한 파일 불러오기
import MyHeader from "./MyHeader";

function App() {
  let name = "이동욱";

  // CSS 를 inline 으로 적용하는 방법
  const style = {
    App: {
      backgroundColor: "black",
    },
    h2: {
      color: "red",
    },
    bold_text: {
      color: "green",
    },
  };

  return (
    <div style={style.App}>
      <MyHeader />
      <h2 style={style.h2}>안녕 리액트 {name}</h2>
      <b style={style.bold_text}>React.js</b>
    </div>
  );
}

export default App;
```

- **값을 사용할 때 표현 방법** : `{ 값 }`

  - **문자열 사용 가능**

  - **숫자의 경우 연산으로 표기 가능**

  - 함수 호출을 통해 표현 가능

  - _단, 숫자나 문자가 아닌 빈 배열이나 boolean 값을 넣으면 렌더가 안된다!_

```javascript
function App() {
  let name = "이동욱";

  // CSS 를 inline 으로 적용하는 방법
  const style = {
    App: {
      backgroundColor: "black",
    },
    h2: {
      color: "red",
    },
    bold_text: {
      color: "green",
    },
  };

  const number = 5;

  return (
    <div style={style.App}>
      <MyHeader />
      <h2 style={style.h2}>안녕 리액트 {name}</h2>
      <b style={style.bold_text}>
        {number}는 : {number % 2 === 0 ? "짝수" : "홀수"}
      </b>
    </div>
  );
}

export default App;
```

- **값으로 나타낼 때는 삼항 연산자(조건부 렌더링)를 통해 위와 같이 표기도 가능!**

  - **`{ number } 는 : { number % 2 === 0 ? "짝수" : "홀수" }`**

## ▶️ JSX 코드 예시

```javascript
import React from "react";

function Book(props) {
  return (
    <div>
      <h1>{`이 책의 이름은 ${props.name}입니다.`}</h1>
      <h2>{`이 책은 ${props.numOfPage}페이지로 이루어져 있습니다.`}</h2>
    </div>
  );
}

export default Book;
```

```javascript
import React from "react";
import Book from "./Book";

function Library(props) {
  return (
    <div>
      <Book name="처음 만난 파이썬" numOfPage={300} />
      <Book name="처음 만난 AWS" numOfPage={400} />
      <Book name="처음 만난 리액트" numOfPage={500} />
    </div>
  );
}

export default Library;
```

```javascript
import React from "react";
...

import Library from "./chapter_03/Library";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Library />
  </React.StrictMode>
);
```

# State - 상태

> Component의 변경 가능한 데이터를 의미하며, **계속해서 변화하는(동적으로) 특정 상태**
> JavaScript 객체임
> **modifier 함수를 이용해서 state를 바꾸면 해당 컴포넌트가 리렌더링됨!**

- state는 개발자가 정의함

  - **렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 함!**

  - _WHY? 불필요한 경우에 component를 재렌더링하게 되면 성능 저하_

- _state는 직접 수정할 수 없음 (수정하면 안됨!)_

  - setState 를 통해 수정해야 함
  - 현재 state를 바탕으로 다음 state를 계산할 때는 함수를 활용하는 것이 혼란을 줄여줌!

- 상태에 따라 각각 다른 동작을 설정할 수 있음

  - 예) 다크 모드, 카운터 등

- **Component는 자신이 가진 State가 변화하면 화면을 rerender 한다!**

  - **`import React, { useState } from 'react`**

  - **`const [count, setCount] = useState()`**

  - Counter 만들기

```javascript
// 상태를 만들고 사용하기 위해 호출
import React, { useState } from "react";

const Counter = () => {
  // Count 상태 = 0에서 출발 / 1씩 증가하고, 1씩 감소

  // useState() 메서드이며 setCount 함수는 +-1씩 상태 변화를 일으킴
  const [count, setCount] = useState(0);
  // +1 증가시키는 함수
  const onIncrease = () => {
    setCount(count + 1);
  };
  // -1 감소시키는 함수
  const onDecrease = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

export default Counter;
```

- _새로운 Counter 를 추가하고 싶다면 이름을 모두 변경해주어야 함! (중복 X)_

```javascript
// 상태를 만들고 사용하기 위해 호출
import React, { useState } from "react";

const Counter = () => {
  // useState() 메서드이며 setCount 함수는 +-1씩 상태 변화를 일으킴
  const [count2, setCount2] = useState(0);
  // +1 증가시키는 함수
  const onIncrease2 = () => {
    setCount2(count2 + 1);
  };
  // -1 감소시키는 함수
  const onDecrease2 = () => {
    setCount2(count2 - 1);
  };

  return (
    <div>
      <h2>{count2}</h2>
      <button onClick={onIncrease2}>+</button>
      <button onClick={onDecrease2}>-</button>
    </div>
  );
};

export default Counter;
```

# Elements

> **리액트 앱을 구성하는 가장 작은 블록들**

- **React Elements = Virtual DOM**

  - **render 하면 DOM Elements인 Browser DOM**

  - 화면에서 보이는 것들을 기술

  - 자바스크립트 객체 형태로 존재

- **특징**

  - **immutable (불변성) : 한 번 생성된 element는 변하지 않음**

    - _Elements 생성 후에는 children이나 attributes를 바꿀 수 없음!_

    - **변경된 화면을 나타내기 위해서는 새로운 Element를 만든 후 기존과 바꿔야 함**

- **Elements 렌더링하기**

  - 화면에 보여주기 위함 (Virtual DOM => Brower DOM 으로 이동)

  - **렌더링된 Elements를 업데이트 하려면?**

    - 새로운 Element 를 생성하여 기존의 것을 업데이트함

## ▶️ 실습 : 시계 만들기

```javascript
import React from "react";

function Clock(props) {
  return (
    <div>
      <h1>안녕, 리액트</h1>
      <h2>현재 시간: {new Date().toLocaleTimeString()}</h2>
    </div>
  );
}

export default Clock;
```

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
...

import Clock from "./chapter_04/Clock";

setInterval(() => {
  ReactDOM.render(
    <React.StrictMode>
      <Clock />
    </React.StrictMode>,
    document.getElementById("root")
  );
}, 1000);
```

# Components

> **리액트는 컴포넌트 기반 라이브러리이며, 컴포넌트들을 모아서 개발**

- 작은 컴포넌트들이 모여서 하나의 컴포넌트로 구성되며 결국 하나의 페이지로 이어짐

- **React Component는 함수처럼 하나의 입력을 받아 하나의 출력을 보여줌**

  - **입력은 `Props`, 출력은 `React element` 가 되는 것**

  - 그들의 Props에 관해서는 Pure 함수 같은 역할을 해야 함

    - **Props를 직접 바꿀 수 없고, 같은 Props에 대해서는 항상 같은 결과를 보여줌**

- 1️⃣ **Function Component**

  - 지금은 함수 컴포넌트를 대다수 사용

    - **Hook 을 사용할 수 있게 됨**

```javascript
function Welcome(props) {
  return <h1> Hi, {props.name}</h1>;
}
```

- 2️⃣ **Class Component**

  - `React.Component`를 상속받아서 생성

```javascript
class Welcome extends React.Component {
  render() {
    return <h1> Hi, {this.props.name}</h1>;
  }
}
```

- **Component 이름은 항상 대문자로 시작해야 함**

  - _소문자로 할 경우 HTML 의 DOM 태그로 인식_

- Component가 렌더링 되려면 Element를 생성한 후 Element를 렌더링 함

## ▶️ Component 함성

> 여러 개의 컴포넌트를 합쳐 하나의 컴포넌트로 사용

- Component 안에 또 다른 Component를 쓸 수 있음

  - **복잡한 화면을 여러 개의 Component로 나눠서 구현 가능!**

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App(props) {
  return (
    <div>
      <Welcome name="Mike" />
      <Welcome name="Steve" />
      <Welcome name="Jane" />
    </div>
  );
}
```

## ▶️ Component 추출

> 복잡한 컴포넌트를 쪼개서 여러 개로 나눌 수도 있음

- 컴포넌트의 재사용성을 올릴 수 있음

- 개발속도 향상

  - 재사용 가능한 Component를 많이 가지고 있을수록 개발 속도가 빨라진다!!

```javascript
import React from "react";

function Comment(props) {
  return (
    <div>
      <h1>제가 만든 첫 컴포넌트입니다.</h1>
    </div>
  );
}

export default Comment;
```

```javascript
import React from "react";
import Comment from "./Comment";

function CommentList(props) {
  return (
    <div>
      <Comment />
    </div>
  );
}

export default CommentList;
```

# Props

> **컴포넌트에 데이터(입력)를 전달하는 방법** = Component의 속성
>
> **컴포넌트에 전달할 다양한 정보를 담고있는 자바스크립트 객체**

## ▶️ JavaScript 함수의 속성

- 2가지 속성

  - 입력값을 변경하지 않으면 같은 입력값에 대해서는 항상 같은 출력값을 리턴 = pure

  - 입력값을 변경하는 것 = impure

## ▶️ 특징

- **읽기 전용 (Read-Only)**

  - 값을 변경할 수 없다는 의미

  - _새로운 값으로 변경하려면?_

    - **새로운 값을 컴포넌트에 전달하여 새로운 Element를 생성해야 함**

- `App` Component의 데이터를 하위인 `Counter` Component에서 사용하려면?

  - `Prop` 기능을 통해 자식 컴포넌트에서 데이터를 활용할 수 있음
  - `Prop` 으로 데이터를 전송한다고 해서 바로 적용되는 것이 아니라 직접 받아서 사용해야 적용되는 것

```javascript
// 생성한 파일 불러오기
import MyHeader from "./MyHeader";
import Counter from "./Counter";

function App() {
  return (
    <div>
      <MyHeader />
      {/* Prop 기능 사용 = 자식 컴포넌트에 전달 */}
      <Counter a={1} initialValue={5} />
    </div>
  );
}

export default App;
```

- _단, 자식 컴포넌트에서 사용할 데이터가 많아지면 길어지기 때문에 별도로 변수 생성하여 전달_

  - **`spread` 연산자를 활용하면 객체를 모두 펼친 상태로 전달 가능**

    - 자식 컴포넌트가 부모의 데이터를 받을 때는 비구조화 상태이므로 바로 접근 가능

```javascript
import MyHeader from "./MyHeader";
import Counter from "./Counter";

function App() {
  // 자식 컴포넌트에 전달할 객체 생성
  const counterProps = {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: 5,
  };

  return (
    <div>
      <MyHeader />
      {/* spread 연산자 활용 */}
      <Counter {...counterProps} />
    </div>
  );
}

export default App;
```

```javascript
import React, { useState } from "react";

// 매개 변수를 통해 부모에서 전달된 데이터를 받아야 함
const Counter = ({ initialValue }) => {
  const [count, setCount] = useState(initialValue);

  const onIncrease = () => {
    setCount(count + 1);
  };

  const onDecrease = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

// 부모에서 데이터를 내려주지 않더라도 아래와 같이 초기값 설정 가능 (for 에러방지)
Counter.defaultProps = {
  initialValue: 0,
};

export default Counter;
```

- **정적인 데이터 뿐만 아니라 동적인 데이터도 전달 가능!**

  - `State`

```javascript
import React, { useState } from 'react'
// 자식 컴포넌트 불러오기
import OddEvenResult from './OddEvenResult';

// 매개 변수를 통해 부모에서 전달된 데이터를 받아야 함
const Counter =  ({initialValue}) => {

  const [count, setCount] = useState(initialValue);

  ...

  return (
    <div>
      <h2>{ count }</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
      {/* 자식 컴포넌트로 사용 */}
      <OddEvenResult count={count} />
    </div>
  )
}

// 부모에서 데이터를 내려주지 않더라도 아래와 같이 초기값 설정 가능 (for 에러방지)
Counter.defaultProps = {
  initialValue: 0,
}

export default Counter;
```

```javascript
// OddEventResult : 부모에서 내려준 props 를 받기
const OddEvenResult = ({ count }) => {
  // console.log(count)
  return <>{count % 2 === 0 ? "짝수" : "홀수"}</>;
};

export default OddEvenResult;
```

- **부모가 내려주는 props가 변경이되면 재렌더링함** = rerender

  - 부모 요소의 state가 변경이되면 자식 요소도 재랜더링 된다! = rerender

- **Rerender 의 기준**

  1️⃣ 본인의 state가 변경될 때

  2️⃣ 나에게 내려오는 부모의 props 데이터가 변경될 때

  3️⃣ 내 부모가 Rerender 되면 나 또한 Rerender 된다.

- **Component 자체도 전달 가능**

```javascript
// 생성한 파일 불러오기
import MyHeader from './MyHeader';
import Counter from './Counter';
import Container from './Container';

...

  return (
    // Container 컴포넌트의 자식으로 설정 = 감싸주기
    <Container>
      <div>
        <MyHeader />
        <Counter  {...counterProps} />
      </div>
    </Container>
  );
}

export default App;
```

```javascript
// props 를 인자 통해서 받기 = MyHeader & Counter 의 데이터 모두 전달됨
const Container = ({ children }) => {
  // console.log(children)
  return (
    <div style={{ margin: 20, padding: 20, border: "1px solid gray" }}>
      {children}
    </div>
  );
};

export default Container;
```

- `children` 으로 데이터를 전달받아 그 주변을 CSS inline으로 꾸며줄 수 있음

- Prop 종류에 따라 적합한 자료형의 데이터가 들어갈 수 있도록 도와주는 **`PropTypes`라는 패키지가 존재!**

  - 원하는 대로 Prop 종류를 지정하고, 리액트에게 알려주어 혹시라도 다른 자료형이 입력되면 콘솔창에 warning를 띄어줌!

```javascript
<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>;

Btn.propTypes = {
  text: PropTypes.string.isRequired,
  fontSize: PropTypes.number,
};
```
