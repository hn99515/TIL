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

* **Component 를 만드는 데 아주 유용한 문법**

* 닫힌 태그가 반드시 필요!
  
  * 단, `a`태그 나 `image` 태그 등 닫힌 태그가 필요없었던 태그들은 self closed tag로 표현
  
  * 예) `<a />`, `<image />`등

* **최상위 태그가 반드시 1개 필요한데 원하지 않는다면 `Fragment`를 활용**

```javascript
import './App.css';
// 생성한 파일 불러오기
import MyHeader from './MyHeader';
// fragment 를 사용하기 위함 
import React from react;

function App() {
  let name = '이동욱'

  return (
  // 최상위 태그를 삭재할 경우 에러가 발생 (반드시 1개의 최상위 태그가 있어야 한다.)
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

* **CSS 적용하기** : `App.css` & `App.js` 수정

```javascript
// CSS 불러오기
import './App.css';
// 생성한 파일 불러오기
import MyHeader from './MyHeader';

function App() {
  let name = '이동욱'

  return (
    // 최상위 태그를 삭재할 경우 에러가 발생 (반드시 1개의 최상위 태그가 있어야 한다.)
    <div className="App">
      {/* 생성한 파일 사용하기 */}
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트 {name}</h2>
        <b id='bold_text'>React.js</b>
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

* **CSS 를 Inline 으로 적용하기**
  
  * CSS 파일 없이도 적용 가능

```javascript
// 생성한 파일 불러오기
import MyHeader from './MyHeader';

function App() {
  let name = '이동욱'

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
  }

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

* **값을 사용할 때 표현 방법** : `{ 값 }`
  
  * **문자열 사용 가능**
  
  * **숫자의 경우 연산으로 표기 가능**
  
  * 함수 호출을 통해 표현 가능
  
  * *단, 숫자나 문자가 아닌 빈 배열이나 boolean 값을 넣으면 렌더가 안된다!*

```javascript
function App() {
  let name = '이동욱'

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
  }

  const number = 5

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

* **값으로 나타낼 때는 삼항 연산자(조건부 렌더링)를 통해 위와 같이 표기도 가능!**
  
  * **`{ number } 는 : { number % 2 === 0 ? "짝수" : "홀수" }`**

# State - 상태

> **계속해서 변화하는(동적으로) 특정 상태**

* 상태에 따라 각각 다른 동작을 설정할 수 있음
  
  * 예) 다크 모드, 카운터 등

* **Component는 자신이 가진 State가 변화하면 화면을 rerender 한다!**
  
  * **`import React, { useState } from 'react`**
  
  * **`const [count, setCount] = useState()`**
  
  * Counter 만들기

```javascript
// 상태를 만들고 사용하기 위해 호출
import React, { useState } from 'react'

const Counter =  () => {
  // Count 상태 = 0에서 출발 / 1씩 증가하고, 1씩 감소

  // useState() 메서드이며 setCount 함수는 +-1씩 상태 변화를 일으킴
  const [count, setCount] = useState(0);
  // +1 증가시키는 함수
  const onIncrease = () => {
    setCount(count + 1)
  }
  // -1 감소시키는 함수
  const onDecrease = () => {
    setCount(count - 1)
  }

  return (
    <div>
      <h2>{ count }</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  )
}

export default Counter;
```

* *새로운 Counter 를 추가하고 싶다면 이름을 모두 변경해주어야 함! (중복 X)*

```javascript
// 상태를 만들고 사용하기 위해 호출
import React, { useState } from 'react'

const Counter =  () => {
  // useState() 메서드이며 setCount 함수는 +-1씩 상태 변화를 일으킴
  const [count2, setCount2] = useState(0);
  // +1 증가시키는 함수
  const onIncrease2 = () => {
    setCount2(count2 + 1)
  }
  // -1 감소시키는 함수
  const onDecrease2 = () => {
    setCount2(count2 - 1)
  }

  return (
    <div>
      <h2>{ count2 }</h2>
      <button onClick={onIncrease2}>+</button>
      <button onClick={onDecrease2}>-</button>
    </div>
  )
}

export default Counter;
```

# Props

> **컴포넌트에 데이터를 전달하는 방법**

* `App` Component의 데이터를 하위인 `Counter` Component에서 사용하려면?
  
  * `Prop` 기능을 통해 자식 컴포넌트에서 데이터를 활용할 수 있음

```javascript
// 생성한 파일 불러오기
import MyHeader from './MyHeader';
import Counter from './Counter';

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

* *단, 자식 컴포넌트에서 사용할 데이터가 많아지면 길어지기 때문에 별도로 변수 생성하여 전달*
  
  * **`spread` 연산자를 활용하면 객체를 모두 펼친 상태로 전달 가능**
    
    * 자식 컴포넌트가 부모의 데이터를 받을 때는 비구조화 상태이므로 바로 접근 가능

```javascript
import MyHeader from './MyHeader';
import Counter from './Counter';

function App() {
  // 자식 컴포넌트에 전달할 객체 생성
  const counterProps = {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: 5,
  }

  return (
    <div>
      <MyHeader />
      {/* spread 연산자 활용 */}
      <Counter  {...counterProps} />
    </div>
  );
}

export default App;
```

```javascript
import React, { useState } from 'react'

// 매개 변수를 통해 부모에서 전달된 데이터를 받아야 함
const Counter =  ({initialValue}) => {

  const [count, setCount] = useState(initialValue);
  
  const onIncrease = () => {
    setCount(count + 1)
  }
  
  const onDecrease = () => {
    setCount(count - 1)
  }

  return (
    <div>
      <h2>{ count }</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  )
}

// 부모에서 데이터를 내려주지 않더라도 아래와 같이 초기값 설정 가능 (for 에러방지)
Counter.defaultProps = {
  initialValue: 0,
}

export default Counter;
```

* **정적인 데이터 뿐만 아니라 동적인 데이터도 전달 가능!**
  
  * `State`

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
const OddEvenResult = ({count}) => {
  // console.log(count)
  return <>{count % 2 === 0 ? "짝수": "홀수"}</>
}

export default OddEvenResult
```

* **부모가 내려주는 props가 변경이되면 재렌더링함** = rerender
  
  * 부모 요소의 state가 변경이되면 자식 요소도 재랜더링 된다! = rerender

* **Rerender 의 기준**
  
  1️⃣ 본인의 state가 변경될 때
  
  2️⃣ 나에게 내려오는 부모의 props 데이터가 변경될 때
  
  3️⃣ 내 부모가 Rerender 되면 나 또한 Rerender 된다.

* **Component 자체도 전달 가능**

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
      <div style={{margin:20, padding:20, border: "1px solid gray"}}>
        {children}
      </div>
  )
}

export default Container
```

* `children` 으로 데이터를 전달받아 그 주변을 CSS inline으로 꾸며줄 수 있음
