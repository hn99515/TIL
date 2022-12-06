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


