# React에서 사용자 입력 처리하기

## ▶️ 일기장 만들기

* DiaryEditor 컴포넌트가 필요한 것
  
  * 작성자, 일기 본문, 감정 점수

## ▶️ 사용자 입력 처리

> **`<input />`, `<textarea />`, `<select>`, `<option>` 등의 사용자 입력이 가능한 태그를 처리하는 방법**

* 사용자 입력이 가능한 태그와의 상호작용
  
  * 1️⃣ **일기 작성자 받기: `<input />`**

```javascript
import { useState } from "react"

// 작성자, 일기 본문, 감점줌수를 포함한 답변!
const DiaryEditor = () => {
  // useState 를 통해 input 에 들어갈 값을 컨트롤: 작성자, 상태변화함수
  const [author, setAuthor] = useState("")

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        {/* 일기 작성자 */}
        <div>
          {/* onChange : 사용자가 입력하면 setAuthor 함수를 실행 */}
          <input
            name="author"
            value={author} 
            onChange={(e) => {
              // 사용자 입력값을 화면에 그대로 보여주기
              setAuthor(e.target.value)
            }}
          />
        </div>
      </div>
  )
}

export default DiaryEditor
```

* 2️⃣ **일기 본문 받기: `<textarea />`**

```javascript
import { useState } from "react"

// 작성자, 일기 본문, 감정점수를 포함한 답변!
const DiaryEditor = () => {
  ...
  // 일기의 본문(textarea)을 컨트롤
  const [content, setContent] = useState("")

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        ...
        {/* 일기를 작성할 본문 */}
        <div>
          <textarea 
            value={content}
            onChange={(e) => {
              setContent(e.target.value)
            }}
          />
        </div>
      </div>
  )
}

export default DiaryEditor
```

* **일기 작성자와 본문을 받는 행위가 매우 유사하기 때문에 하나로 묶을 수 있음!**
  
  * **`const [state, setState] = useState({author: "", content: ""})`**
  
  * 상태변화 시 실행되는 함수도 변경해야 함!
    
    * `e.target.value` = 사용자가 입력한 값
    
    * `state.author` or `state.content` = 기본 값 그대로
  
  * *단, 같은 동작을 하는 변수가 많아지면 일일이 코드를 작성하기 어렵다*
    
    * **`...state` 작성해주면 변화되지 않는 값은 한 번에 처리됨!**
    
    * **단! 제일 위에 적어야 함!!** : 그렇지 않으면 다시 업데이트 전 값으로 돌아가기 때문

```javascript
import { useState } from "react"

const DiaryEditor = () => {
  // 같은 동작이므로 작성자와, 일기 본문을 함께 받기 (초기값 설정)
  const [state, setState] = useState({
    author: "",
    content: "",
  })

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        {/* 일기 작성자 */}
        <div>
          <input
            name="author"
            value={state.author} 
            onChange={(e) => {
              setState({
                author: e.target.value,
                content: state.content,
              })
            }}
          />
        </div>
        {/* 일기를 작성할 본문 */}
        <div>
          <textarea 
            value={state.content}
            onChange={(e) => {
              setState({
                author: state.author,
                content: e.target.value,
              })
            }}
          />
        </div>
      </div>
  )
}

export default DiaryEditor
```

* **`setState`도 같은 모양인데 두 번 작성하고 있기 때문에 하나로 합칠 수가 있음!**
  
  * **`handleChangeState = (e) => { setState({ ...state, [e.target.name]: e.target.value, }) }`**

```javascript
import { useState } from "react"

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: "",
    content: "",
  })

  const handleChangeState = (e) => {
    // console.log(e.target.name)
    // console.log(e.target.value)

    setState({
      ...state,
      [e.target.name]: e.target.value,
    })
  }

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        {/* 일기 작성자 */}
        <div>
          <input
            name="author"
            value={state.author} 
            onChange={handleChangeState}
          />
        </div>
        {/* 일기를 작성할 본문 */}
        <div>
          <textarea
            name="content" 
            value={state.content}
            onChange={handleChangeState}
          />
        </div>
      </div>
  )
}

export default DiaryEditor
```

* 3️⃣ **오늘의 감정 점수를 받기: `<select>`, `<option>`**
  
  * 기존에 `handleChangeState`에 작성하여 표현할 수 있음
  
  * 초기 값과 `name`, `value`, `onChange` 값만 잘 적어주면 사용 가능!

```javascript
import { useState } from "react"

const DiaryEditor = () => {
  // 같은 동작이므로 작성자와, 일기 본문을 함께 받기 (초기값 설정)
  const [state, setState] = useState({
    author: "",
    content: "",
    emotion: 1,
  })

  const handleChangeState = (e) => {
    // console.log(e.target.name)
    // console.log(e.target.value)

    setState({
      ...state,
      [e.target.name]: e.target.value,
    })
  }

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        ...
        {/* 오늘의 감정 점수를 받기 */}
        <div>
          <select
            name="emotion"
            value={state.emotion}
            onChange={handleChangeState}
          >
            <option value={1}>1</option>
            <option value={2}>2</option>
            <option value={3}>3</option>
            <option value={4}>4</option>
            <option value={5}>5</option>
          </select>
        </div>
      </div>
  )
}

export default DiaryEditor
```

* **저장 버튼 생성 및 활성화**
  
  * 저장하기 버튼 누를 때 활성화 되는 함수 생성: `handleSubmit`
  
  * 저장하기 버튼 누를 때: `onClick`

```javascript
import { useState } from "react"

const DiaryEditor = () => {
  ...

  // 저장하기 버튼 활성화 함수
  const handleSubmit = () => {
    console.log(state)
    alert('저장 성공!')
  }

  return (
      <div className="DiaryEditor">
        ...
        <div>
          <button onClick={handleSubmit}>일기 저장하기</button>
        </div>
      </div>
  )
}

export default DiaryEditor
```

# DOM 조작하기

> **저장하기 버튼을 클릭했을 때 정상적으로 입력되었는지 확인하고 아니라면 `focus`하기**

* **유효성 검사는 특정 길이 이상 작성했는지 체크**
  
  * *작성자 부분이 비어있거나 본문이 5글자 이상이 아닌 경우 경고창으로 알림*

```javascript
// 저장하기 버튼 활성화 함수
// 유효성 검사: 작성자와 일기의 내용이 비어있으면 focus
  const handleSubmit = () => {
    if (state.author.length < 1) {
      alert("작성자는 최소 1글자 이상 입력해주세요")
      return // 더이상 다음 작업 못하도록 막는 역할
    }
    
    if (state.content.length < 5) {
      alert("일기 본문은 최소 5글자 이상 입력해주세요")
      return
    }
    alert('저장 성공!')
  }
```

* *단, 경고창을 띄우는 것은 UX/UI에서 좋은 경험은 아님*
  
  * **최근 유행하는 `focus` 기능 사용해보기**
  
  * `import { useRef } from "react"`
    
    * **`useRef()` : DOM 요소를 선택할 수 있는 기능**
  
  * DOM 요소를 선택한 후 각 태그에 `ref` 지정
  
  * **`<상수명>.current.focus` 를 통해 조건에 만족하지 않는 경우 focus 하기!**

```javascript
import { useRef, useState } from "react"

const DiaryEditor = () => {

  // DOM 요소를 선택하는 함수를 활용해 반환값을 상수에 담기
  const authorInput = useRef()
  const contentTextarea = useRef()

  const [state, setState] = useState({
    author: "",
    content: "",
    emotion: 1,
  })

  ...
  // 저장하기 버튼 활성화 함수
  // 유효성 검사: 작성자와 일기의 내용이 비어있으면 focus
  const handleSubmit = () => {
    if (state.author.length < 1) {
      // focus
      authorInput.current.focus()
      return // 더이상 다음 작업 못하도록 막는 역할
    }
    
    if (state.content.length < 5) {
      contentTextarea.current.focus()
      return
    }
    alert('저장 성공!')
  }

  return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        {/* 일기 작성자 */}
        <div>
          <input
            ref={authorInput}
            ...
          />
        </div>
        {/* 일기를 작성할 본문 */}
        <div>
          <textarea
            ref={contentTextarea}
            ...
          />
        </div>
        {/* 오늘의 감정 점수를 받기 */}
        ...
      </div>
  )
}

export default DiaryEditor
```

# 리스트(배열) 조회 하기

> **작성된 일기를 배열에 저장하기**

* **배열을 이용하여 LIST를 렌더링 해보고 개별적인 컴포넌트로 만들어보기**
  
  * 게시글, 피드 게시 등의 역할
  
  * 작성된 일기를 보여주기 위해 새로운 컴포넌트를 생성한 후 진행

* 더미 리스트를 생성하여 `props` 기능을 통해 데이터 전달하기: 데이터 흐름을 파악하기 위함
  
  * 추후 실제 데이터 생성한 후 삭제 예정

```javascript
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

const dummyList = [
  {
    id: 1,
    author: "이동욱",
    content: "일기 작성 시작!",
    emotion: 5,
    // 생성 시각 - getTime() 현재 시각을 숫자로 변환
    created_date: new Date().getTime()
  },
  {
    id: 2,
    author: "김종혁",
    content: "쓰기 싫은데!",
    emotion: 1,
    // 생성 시각 - getTime() 현재 시각을 숫자로 변환
    created_date: new Date().getTime()
  },
  {
    id: 3,
    author: "하진우",
    content: "재밌겠는데?",
    emotion: 3,
    // 생성 시각 - getTime() 현재 시각을 숫자로 변환
    created_date: new Date().getTime()
  },
]

function App() {
  return (
    <div className="App">
      <DiaryEditor />
      {/* 더미 리스트를 prop으로 전달 */}
      <DiaryList diaryList={dummyList}/>
    </div>
  );
}

export default App;
```

* `map` : 더미 리스트의 들어있는 객체를 하나씩 사용하기

```javascript
// prop 된 데이터 받기 : diaryList
const DiaryList = ({ diaryList }) => {
  console.log(diaryList)
  return <div className="DiaryList">
    <h2>일기 리스트</h2>
    <h4>{diaryList.length}개의 일기가 있습니다.</h4>
    <div>
      {/* 더미리스트를 하나씩 꺼내서 사용 */}
      {diaryList.map((it) => (
        <div>
          <div>작성자: {it.author}</div>
          <div>일기: {it.content}</div>
          <div>감정: {it.emotion}</div>
          <div>작성 시간(ms): {it.created_date}</div>
        </div>
      ))}
    </div>
  </div>
}

export default DiaryList
```

* *단, 배열이 비어있는 경우(일기 작성을 하지 않은 경우) `undefined`로 전달되는데 에러가 발생*
  
  * `undefined`의 경우 `length` 속성을 가지지 못하기 때문에 에러가 발생
  
  * **`defaultProps` 를 통해 prop할 데이터의 기본값을 빈배열(`[]`)로 설정!**
    
    * 에러 발생 방지

```javascript
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

const dummyList = [
  ...
]

function App() {
  return (
    <div className="App">
      <DiaryEditor />
      {/* prop으로 undefined를 전달 */}
      <DiaryList diaryList={undefined}/>
    </div>
  );
}

export default App;

```

```javascript
// prop 된 데이터 받기
const DiaryList = ({ diaryList }) => {
  ...
}

// undefined의 경우 에러가 발생하므로 기본값을 미리 빈배열로 지정
DiaryList.defaultProps = {
  diaryList: []
}

export default DiaryList
```

* 배열 내 각 아이템들은 유니크한 `"key" prop`을 가져야 함!
  
  * 객체별 id 값을 활용
  
  * **id가 없는 경우 map 콜백함수의 2번째 인자로 index 활용**
    
    * *단, idx 활용 시 데이터 삭제/생성 등으로 인한 꼬일 가능성이 커지므로 주의!*
  
  * **`<div key={it.id}>`** or `<div key={idx}>`

```javascript
// prop 된 데이터 받기
const DiaryList = ({ diaryList }) => {
  console.log(diaryList)
  return <div className="DiaryList">
    <h2>일기 리스트</h2>
    <h4>{diaryList.length}개의 일기가 있습니다.</h4>
    <div>
      {/* 더미리스트를 하나씩 꺼내서 사용 */}
      {diaryList.map((it) => (
        <div key={it.id}>
          <div>작성자: {it.author}</div>
          <div>일기: {it.content}</div>
          <div>감정: {it.emotion}</div>
          <div>작성 시간(ms): {it.created_date}</div>
        </div>
      ))}
    </div>
  </div>
}

// undefined의 경우 에러가 발생하므로 기본값을 미리 빈배열로 지정
DiaryList.defaultProps = {
  diaryList: []
}

export default DiaryList
```

* **각각의 일기를 수정/삭제 등을 진행할 수 있어야 하므로 일기의 또 다른 컴포넌트를 하나 생성하기**
  
  * 생성된 DiaryItem 컴포넌트에 데이터 prop
  
  * **`{diaryList.map((it) => ( <DiaryItem key={it.id} {...it} /> ))}`**

```javascript
import DiaryItem from "./DiaryItem.js"

// prop 된 데이터 받기
const DiaryList = ({ diaryList }) => {
  console.log(diaryList)
  return <div className="DiaryList">
    <h2>일기 리스트</h2>
    <h4>{diaryList.length}개의 일기가 있습니다.</h4>
    <div>
      {/* 더미리스트의 데이터를 모두 전달 */}
      {diaryList.map((it) => (
        <DiaryItem key={it.id} {...it}/>
      ))}
    </div>
  </div>
}

...

export default DiaryList
```

```javascript
// prop 된 데이터 가져오기
const DiaryItem = ({author, content, created_date, emotion, id}) => {
  return <div className="DiaryItem">
    <div className="info">
      <span>작성자 : {author} | 감정점수 : {emotion}</span>
      <br />
      {/* ms 로 표현된 시간을 다시 우리가 알아볼 수 있게 되돌리기 */}
      <span className="date">
        {new Date(created_date).toLocaleString()}
      </span>
    </div>
    <div className="content">{content}</div>
  </div>
}

export default DiaryItem
```

# 리스트 데이터 추가(생성)하기

> **배열을 이용하여 List에 아이템을 동적으로 추가하기**

* 컴포넌트 & 데이터 구조
  
  * **`<App />` 이 최상위에 존재: 하위에 `<DiaryEditor />`, `<DiaryList />` 존재**
  
  * **`<App />` 에서 State 로 `[data, setData]` 를 가지고 있다면**
    
    * data 는 `<DiaryList />` 에 전달: 추가된 data를 prop으로 새로운 배열 전달
    
    * setData 는 `<DiaryEditor />` 에 전달: 새로운 일기 작성 시 배열에 새로운 아이템을 하나씩 추가

* *React는 단방향으로만 데이터가 흐르기 때문에 같은 레벨에서는 데이터를 주고받을 수 없다.*
  
  * `prop` : 위에서 아래로 데이터 전달 가능
  
  * `event` : 아래에서 위로 데이터 전달 가능 (예. 일기 생성)

```javascript
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';
import { useRef, useState } from 'react';

function App() {
  // 상태 변화 함수 생성 (초기=[])
  const [data, setData] = useState([])
  // 데이터의 id를 자동 생성 (초기=0)
  const dataId = useRef(0)
  // 새로운 일기를 생성하는 함수
  const onCreate = (author, content, emotion) => {
    const created_date = new Date().getTime()
    // 새로운 객체로 추가
    const newItem = {
      author,
      content,
      emotion,
      created_date,
      id: dataId.current
    }
    // 생성될 때마다 id는 +1 되어야 함
    dataId.current += 1
    setData([newItem, ...data]) // 기존 데이터에 새로운 데이터 추가(최신순)
  }

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <DiaryList diaryList={data}/>
    </div>
  );
}

export default App;
```

```javascript
import { useRef, useState } from "react"

// onCreate 함수를 가져오기
const DiaryEditor = ({onCreate}) => {

  ...

  // 저장하기 버튼 활성화 함수
  const handleSubmit = () => {
    ...
    // 저장 완료 시 onCreate() 호출: state에 작성된 정보가 저장됨!
    onCreate(state.author, state.content, state.emotion)
    alert('저장 성공!')
  }
```

* **일기가 정상적으로 저장된다면 `setState({})` 를 통해 저장값 초기화**

```javascript
import { useRef, useState } from "react"

// onCreate 함수를 가져오기
const DiaryEditor = ({onCreate}) => {

  ...

  // 저장하기 버튼 활성화 함수
  // 유효성 검사: 작성자와 일기의 내용이 비어있으면 focus
  const handleSubmit = () => {
    ...
    // 저장 완료 시 onCreate() 호출 - state에 작성된 정보가 저장됨!
    onCreate(state.author, state.content, state.emotion)
    alert('저장 성공!')
    // 저장 후 기본값 초기화
    setState({
      author: "",
      content: "",
      emotion: 1,
    })
  }
```

# 리스트 데이터 삭제하기

> **삭제 버튼을 통해 배열 내 데이터 DELETE**

* **해당 id 를 가지고 데이터를 삭제하여 배열의 data를 새로 업데이트 시켜야 함**
  
  * 데이터 삭제 함수를 만들고 `DiaryList`에 함수 전달
  
  * **`<DiaryList onDelete={onDelete} diaryList={data} />`**
  
  * *`DiaryList` 도 `DiaryItem` 에 전달해줘야 함* = prop을 2번 진행한 것!
    
    * **`<DiaryItem key={it.id} {...it} onDelete={onDelete} />`**

```javascript
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';
import { useRef, useState } from 'react';

function App() {
  // 상태 변화 함수 생성
  const [data, setData] = useState([])
  // 데이터의 id를 자동 생성
  const dataId = useRef(0)
  ...

  // 배열 내 데이터 삭제 함수
  const onDelete = (targetId) => {
    console.log(`${targetId}가 삭제되었습니다.`)
  }

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <DiaryList onDelete={onDelete} diaryList={data}/>
    </div>
  );
}

export default App;
```

```javascript
import DiaryItem from "./DiaryItem.js"

// prop 된 데이터 받기
const DiaryList = ({ onDelete, diaryList }) => {
  ...
    <div>
      {/* 더미리스트의 데이터를 모두 전달 */}
      {diaryList.map((it) => (
        <DiaryItem key={it.id} {...it} onDelete={onDelete} />
      ))}
    </div>
  </div>
}
...

export default DiaryList
```

```javascript
// prop 된 데이터 가져오기
const DiaryItem = ({ 
  onDelete, author, content, created_date, emotion, id 
}) => {
  return <div className="DiaryItem">
      ...
    {/* 데이터를 삭제할 버튼 생성 */}
    <button
      onClick={() => {
        // 진짜 삭제할 건지 한 번 더 체크
        if (window.confirm(`${id}번째 일기를 정말 삭제하시겠습니까?`)) {
          onDelete(id)
        }
      }}
    >
      삭제하기
    </button>
  </div>
}

export default DiaryItem
```

* **배열 내 삭제된 데이터를 빼고 새로운 배열을 반환**
  
  * **즉, `it.id` 와 `targetId` 가 다른 데이터로만 배열을 구성**
  
  * **`const newDiaryList = data.filter((it) => it.id !== targetId)`**

```javascript
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';
import { useRef, useState } from 'react';

function App() {
  // 상태 변화 함수 생성
  const [data, setData] = useState([])
  ...

  // 배열 내 데이터 삭제 함수
  const onDelete = (targetId) => {
    console.log(`${targetId}가 삭제되었습니다.`)
    // 삭제된 데이터를 제외한 새로운 배열 반환하기
    const newDiaryList = data.filter((it) => it.id !== targetId)
    setData(newDiaryList)
  }

  ...
}

export default App;
```

# 리스트 데이터 수정하기

> **수정하기 버튼을 통해 배열 내 데이터 Update**

* **배열 내 아이템을 동적으로 수정해보기**

* 수정하기 버튼을 누르면 일기 리스트에서 바로 수정할 수 있어야 함
  
  * **`const [isEdit, setIsEdit] = useState()`**

```javascript
import { useState } from "react";
// prop 된 데이터 가져오기
const DiaryItem = ({
  onRemove,
  ...
}) => {
  // 수정하는 경우 상태변화 함수가 필요 - 기본값으로 false
  // isEdit - 수정 중인지 아닌지 상태 확인용
  const [isEdit, setIsEdit] = useState(false);
  // toggle 함수로서 실행 시 기존 상태에서 반대로 변경됨
  const toggleIsEdit = () => setIsEdit(!isEdit);

  // 수정 시 사용하는 텍스트 폼도 상태변화 확인 (기본값=기존의 수정 전 데이터)
  const [localContent, setLocalContent] = useState(content);

  // 가독성을 위해 삭제버튼 눌렀을 때 삭제할 건지 한 번 더 체크하는 함수를 별도 작성
  const handleRemove = () => {
    if (window.confirm(`${id}번째 일기를 정말 삭제하시겠습니까?`)) {
      onRemove(id);
    }
  };

  return (
    <div className="DiaryItem">
      ...
      <div className="content">
        {/* 삼항연산자 활용 - 참일 때는 수정 폼 / 거짓일 때는 리스트 보여주기 */}
        {isEdit ? (
          <>
            <textarea
              value={localContent}
              onChange={(e) => setLocalContent(e.target.value)}
            />
          </>
        ) : (
          <>{content}</>
        )}
      </div>
      ...
    </div>
  );
};

export default DiaryItem;
```

* **수정 중인지 아닌지에 따라 보여줄 버튼의 내용도 바꿔주기**

```javascript
{/* 수정 상태에 따라서 버튼의 내용도 바꿔주기 */}
      {isEdit ? (
        <>
          <button onClick={toggleIsEdit}>수정 취소</button>
          <button>수정 완료</button>
        </>
      ) : (
        <>
          {/* 데이터를 수정할 버튼 */}
          <button onClick={toggleIsEdit}>수정하기</button>
          {/* 데이터를 삭제할 버튼 */}
          <button onClick={handleRemove}>삭제하기</button>
        </>
      )}
```

* **수정하다가 취소한 뒤 다시 수정하면 기존의 수정하던 데이터가 들어있으므로 초기화 필요**

```javascript
import { useState } from "react";
// prop 된 데이터 가져오기
const DiaryItem = ({
  onRemove,
  ...
}) => {
  ...

  // 수정 취소 후 다시 수정하는 경우 이전의 수정 데이터 초기화 필요
  const handleQuitEdit = () => {
    setIsEdit(false); // 수정 취소했으니 false로 변경
    setLocalContent(content); // 수정 전의 기존값을 가져야 초기화
  };

  ...

  return (
    <div className="DiaryItem">
      ...
      {/* 수정 상태에 따라서 버튼의 내용도 바꿔주기 */}
      {isEdit ? (
        <>
          <button onClick={handleQuitEdit}>수정 취소</button>
          <button>수정 완료</button>
        </>
      ) : (
        <>
          ...
        </>
      )}
    </div>
  );
};

export default DiaryItem;
```

* **마지막으로 수정 완료 클릭 시 수정이 이뤄지는 함수 생성이 필요**
  
  * 데이터 흐름이 단방향이므로 `App.js` 에서 `DiaryItem`까지 내려보내줘야 함

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import { useRef, useState } from "react";

function App() {
  // 상태 변화 함수 생성
  const [data, setData] = useState([]);
  // 데이터의 id를 자동 생성
  const dataId = useRef(0);

  ...

  // 배열 내 데이터 수정 함수 - 수정 대상 id 와 수정된 데이터를 받아야 함
  const onEdit = (targetId, newContent) => {
    setData(
      // 수정할 컨텐츠의 id가 일치하면 수정 & 일치하지 많으면 it 반환 (기존 그대로)
      data.map((it) =>
        it.id === targetId ? { ...it, content: newContent } : it
      )
    );
  };

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
    </div>
  );
}

export default App;
```

```javascript
import { useRef, useState } from "react";
// prop 된 데이터 가져오기
const DiaryItem = ({
  onEdit,
  ...
}) => {
  // 수정하는 경우 상태변화 함수가 필요 - 기본값으로 false
  // isEdit - 수정 중인지 아닌지 상태 확인용
  const [isEdit, setIsEdit] = useState(false);
  // toggle 함수로서 실행 시 기존 상태에서 반대로 변경됨
  const toggleIsEdit = () => setIsEdit(!isEdit);

  // 수정 시 사용하는 텍스트 폼도 상태변화 확인 (기본값=기존의 수정 전 데이터)
  const [localContent, setLocalContent] = useState(content);
  const localContentInput = useRef(); // focus 기능을 사용하기 위해 필요!

  // 수정 취소 후 다시 수정하는 경우 이전의 수정 데이터 초기화 필요
  const handleQuitEdit = () => {
    setIsEdit(false); // 수정 취소했으니 false로 변경
    setLocalContent(content); // 수정 전의 기존값을 가져야 초기화
  };

  // 수정 완료 버튼 누르면 수정된 데이터 전달
  const handleEdit = () => {
    // 수정된 데이터를 유효성 검사 후 전달
    if (localContent.length < 5) {
      localContentInput.current.focus();
      return;
    }
    // 최종 수정 완료됐지는 묻기
    if (window.confirm(`${id}번째 일기를 수정하시겠습니까?`)) {
      onEdit(id, localContent);
      toggleIsEdit(); // 수정 완료 시 상태 변경해줘야 함!
    }
  };

  ...

  return (
    <div className="DiaryItem">
      ...
      <div className="content">
        {/* 삼항연산자 활용 - 참일 때는 수정 폼 / 거짓일 때는 리스트 보여주기 */}
        {isEdit ? (
          <>
            <textarea
              ref={localContentInput}
              value={localContent}
              onChange={(e) => setLocalContent(e.target.value)}
            />
          </>
        ) : (
          <>{content}</>
        )}
      </div>
      {/* 수정 상태에 따라서 버튼의 내용도 바꿔주기 */}
      {isEdit ? (
        <>
          <button onClick={handleQuitEdit}>수정 취소</button>
          <button onClick={handleEdit}>수정 완료</button>
        </>
      ) : (
        <>
          {/* 데이터를 수정할 버튼 */}
          <button onClick={toggleIsEdit}>수정하기</button>
          {/* 데이터를 삭제할 버튼 */}
          <button onClick={handleRemove}>삭제하기</button>
        </>
      )}
    </div>
  );
};

export default DiaryItem;
```


