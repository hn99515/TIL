# 일기 수정 페이지 구현 - EDIT

> **원본 일기의 데이터를 가져온 후 수정**

**① 원본 일기의 데이터를 불러온 후 수정이 진행**

* `App.js` 에서 **path variable 을 설정 `/edit/:id`**

```javascript
import { useState, useContext, useEffect } from "react";
import { useNavigate, useParams } from "react-router-dom";
import { DiaryStateContext } from "../App";
import DiaryEditor from "../components/DiaryEditor";

const Edit = () => {
  // targetDiary 가 존재할 때 state
  const [originData, setOriginData] = useState();

  const navigate = useNavigate();
  // id 가져오기
  const { id } = useParams();

  // data 가져오기 - 배열 전체
  const diaryList = useContext(DiaryStateContext);
  // Mount 되는 순간 해당되는 data 만 보여주기
  useEffect(() => {
    if (diaryList.length >= 1) {
      const targetDiary = diaryList.find(
        (it) => parseInt(it.id) === parseInt(id)
      );
      // 없는 페이지로 접근 시 수정 페이지 접근 X, 값이 있으면 originData 로 초기화
      if (targetDiary) {
        setOriginData(targetDiary);
      } else {
        alert("없는 일기입니다.")
        navigate("/", { replace: true });
      }
    }
    // id 와 diaryList 가 변할 때만 리렌더링
  }, [id, diaryList]);

  return (
    <div>
      {/* originData 가 있으면 DiaryEditor 를 렌더 */}
      {originData && <DiaryEditor isEdit={true} originData={originData} />}
    </div>
  );
};

export default Edit;
```

**② `DiaryEditor.js` 에서 일기 생성이 아닌 수정 페이지인 것을 표기**

* **기존 데이터 불러오기**

* **`headText`도 일기 수정 페이지로 변경**

```javascript
import { useState, useEffect, useRef, useContext } from "react";
...

...
  useEffect(() => {
    // 기존 데이터 불러오기
    if (isEdit) {
      setDate(getStringDate(new Date(parseInt(originData.date))));
      setEmotion(originData.emotion);
      setContent(originData.content);
    }
  }, [isEdit, originData]);

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      <MyHeader
        headText={isEdit ? "일기 수정하기 " : "새 일기 쓰기"}
        leftChild={
          <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
        }
      />
      ...
    </div>
  );
};

export default DiaryEditor;
```

**③ 수정 완료 버튼 클릭 시 `onEdit` 함수가 실행**

* 수정 완료 버튼 클릭 시 한 번 더 물어본 후 진행
  
  * 생성과 수정은 분기를 통해 나눌 수 있음

```javascript
import { useState, useEffect, useRef, useContext } from "react";
...

...

const DiaryEditor = ({ isEdit, originData }) => {
  ...

  // onCreate, onEdit 함수 받아오기
  const { onCreate, onEdit } = useContext(DiaryDispatchContext);

  ...

  // 작성 완료 버튼 클릭 시 실행할 함수
  const handleSubmit = () => {
    if (content.length < 1) {
      contentRef.current.focus();
      return;
    }

    if (
      window.confirm(
        isEdit ? "일기를 수정하시겠습니까?" : "새로운 일기를 작성하시겠습니까?"
      )
    ) {
      // 수정이 아닐 때는 create
      if (!isEdit) {
        onCreate(date, content, emotion);
      } else {
        // 수정 중일 때는 onEdit
        onEdit(originData.id, date, content, emotion);
      }
    }
    navigate("/", { replace: true });
  };

  ...

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      <MyHeader
        headText={isEdit ? "일기 수정하기 " : "새 일기 쓰기"}
        leftChild={
          <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
        }
      />
      <div>
        ...
        <section>
          <div className="control_box">
            <MyButton text={"취소하기"} onClick={() => navigate(-1)} />
            <MyButton
              text={isEdit ? "수정완료 " : "작성완료"}
              type={"positive"}
              onClick={handleSubmit}
            />
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

# 일기 상세 페이지 - detail

> **일기를 하나 선택했을 때 상세 페이지를 의미**

* **2번 이상 중복으로 사용되는 날짜 변환 함수는 `src/util/date.js` 를 생성하여 `import` 후 사용하는 것이 바람직**

## ▶️ 버튼 2개 & 제목 - Header

> **뒤로가기, 수정하기 버튼 / 기록할 제목 표기(연-월-일)**

```javascript
import { useState, useContext, useEffect } from "react";
import { useNavigate, useParams } from "react-router-dom";
import { DiaryStateContext } from "../App";
import { getStringDate } from "../util/date";

import MyHeader from "../components/MyHeader";
import MyButton from "../components/MyButton";

const Diary = () => {
  const { id } = useParams();
  // 일기 데이터 가져오기
  const diaryList = useContext(DiaryStateContext);
  const navigate = useNavigate();
  const [data, setData] = useState();

  useEffect(() => {
    if (diaryList.length >= 1) {
      const targetDiary = diaryList.find(
        (it) => parseInt(it.id) === parseInt(id)
      );
      // 없는 일기 페이지로 이동 시 접근 X
      if (targetDiary) {
        // 일기가 존재할 때
        setData(targetDiary);
      } else {
        alert("없는 일기입니다.");
        navigate("/", { replace: true });
      }
    }
  }, [id, diaryList]);

  if (!data) {
    return <div className="DiaryPage">로딩중입니다...</div>;
  } else {
    return (
      <div className="DiaryPage">
        <MyHeader
          headText={`${getStringDate(new Date(data.date))} 기록`}
          leftChild={
            <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
          }
          rightChild={
            <MyButton
              text={"수정하기"}
              onClick={() => navigate(`/edit/${data.id}`)}
            />
          }
        />
      </div>
    );
  }
};

export default Diary;
```

## ▶️ 오늘의 감정 - Section

> **해당 점수에 맞는 이미지 불러오기**

* 감정의 설명은 일기 데이터에 들어 있지 않음
  
  * **`emotionList` 도 2번 중복 사용되므로 `src/util/emotion.js`를 생성하여 import 하여 사용하기**

```javascript
import { useState, useContext, useEffect } from "react";
import { useNavigate, useParams } from "react-router-dom";
import { DiaryStateContext } from "../App";
import { getStringDate } from "../util/date";
import { emotionList } from "../util/emotion";

import MyHeader from "../components/MyHeader";
import MyButton from "../components/MyButton";

const Diary = () => {
  const { id } = useParams();
  // 일기 데이터 가져오기
  const diaryList = useContext(DiaryStateContext);
  const navigate = useNavigate();
  const [data, setData] = useState();

  useEffect(() => {
    if (diaryList.length >= 1) {
      const targetDiary = diaryList.find(
        (it) => parseInt(it.id) === parseInt(id)
      );
      // 없는 일기 페이지로 이동 시 접근 X
      if (targetDiary) {
        // 일기가 존재할 때
        setData(targetDiary);
      } else {
        alert("없는 일기입니다.");
        navigate("/", { replace: true });
      }
    }
  }, [id, diaryList]);

  if (!data) {
    return <div className="DiaryPage">로딩중입니다...</div>;
  } else {
    const curEmotionData = emotionList.find(
      (it) => parseInt(it.emotion_id) === parseInt(data.emotion)
    );

    return (
      <div className="DiaryPage">
        ...
        <article>
          <section>
            <h4>오늘의 감정</h4>
            <div
              className={[
                "diary_img_wrapper",
                `diary_img_wrapper_${data.emotion}`,
              ].join(" ")}
            >
              <img src={curEmotionData.emotion_img} />
              <div className="emotion_descript">
                {curEmotionData.emotion_descript}
              </div>
            </div>
          </section>
        </article>
      </div>
    );
  }
};

export default Diary;
```

## ▶️ 오늘의 일기 - Section2

> **작성된 오늘의 일기를 보여주는 부분**

```javascript
import { useState, useContext, useEffect } from "react";
...

const Diary = () => {
  const { id } = useParams();
  // 일기 데이터 가져오기
  const diaryList = useContext(DiaryStateContext);
  const navigate = useNavigate();
  const [data, setData] = useState();

  ...

  if (!data) {
    return <div className="DiaryPage">로딩중입니다...</div>;
  } else {
    const curEmotionData = emotionList.find(
      (it) => parseInt(it.emotion_id) === parseInt(data.emotion)
    );

    return (
      <div className="DiaryPage">
        ...
        <article>
          ...
          <section>
            <h4>오늘의 일기</h4>
            <div className="diary_content_wrapper">
              <p>{data.content}</p>
            </div>
          </section>
        </article>
      </div>
    );
  }
};

export default Diary;
```

# 흔히 발생하는 버그 수정하기

> **React 사용 시 자주 만날 수 있는 버그**

1️⃣ **2개의 일기 작성 후 Error 발생 = 테스트 데이터 생성할 경우 항상 조심!**

* *console 창에 `Encountered two children with the same key`*
  
  * 두 개가 같은 키를 가지고 있다는 의미
  
  * **더미 데이터의 id 는 1부터 시작하며, 초기값은 `const dataId = useRef(0)` 로 설정**
    
    * 데이터의 id를 6번부터 시작하도록 설정
    
    * `const dataId = useRef(6)`

```javascript
import React, { useReducer, useRef } from "react";

...

...

// 일기 리스트 dummy data 만들기
const dummyData = [
  {
    id: 1,
    emotion: 1,
    content: "오늘의 일기 1번",
    date: 1670747737741,
  },
  {
    id: 2,
    emotion: 2,
    content: "오늘의 일기 2번",
    date: 1670747839883,
  },
  {
    id: 3,
    emotion: 3,
    content: "오늘의 일기 3번",
    date: 1670747922786,
  },
  {
    id: 4,
    emotion: 4,
    content: "오늘의 일기 4번",
    date: 1670747936736,
  },
  {
    id: 5,
    emotion: 5,
    content: "오늘의 일기 5번",
    date: 1670747985168,
  },
];

function App() {
  // state 생성
  const [data, dispatch] = useReducer(reducer, dummyData);

  // 현재 시간(ms)를 구하기 위해 출력해보기
  // console.log(new Date().getTime());

  const dataId = useRef(6);
  ...

  return (
    ...
  );
}

export default App;
```

2️⃣**오타가 난 경우 - TypeScript 는 오타를 방지함**

* *원하던 대로 작동이 되지 않는 기능이 있다.*
  
  * 관련 코드로 이동한 후 오타가 없는지 체크

3️⃣ **12월 31일에 일기를 쓴 경우에는 화면에 뜨지 않음**

* 데이터 배열에는 들어가 있음!
  
  * Why? `lastDay` 에는 시간도 적어주어야 함
  
  * `23, 59, 59` ( 23시 59분 59초)

```javascript
import { useContext, useEffect, useState } from "react";
import { DiaryStateContext } from "../App";

...

const Home = () => {
  ...
  // 날짜를 저장하는 state
  const [curDate, setCurDate] = useState(new Date());
  // 년월 표기
  const headText = `${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월`;

  // 월이 바뀔 때마다 해당 일기만 불러와야 함
  useEffect(() => {
    // 일기 리스트가 있을 때만 보여줘
    if (diaryList.length >= 1) {
      // 해당 월의 가장 첫 날과 마지막날을 호출
      const firstDay = new Date(
        curDate.getFullYear(),
        curDate.getMonth(),
        1
      ).getTime();

      const lastDay = new Date(
        curDate.getFullYear(),
        curDate.getMonth() + 1,
        0,
        23,
        59,
        59
      ).getTime();
      // 해당 월에 있는 일기만 필터
      setData(
        diaryList.filter((it) => firstDay <= it.date && it.date <= lastDay)
      );
    }
    // diaryList 가 추가/수정/삭제 시 리렌더링 되어야 하므로 추가
  }, [diaryList, curDate]);

  ...
};

export default Home;
```

# LocalStorage를 일기 데이터베이스로 사용하기

> ***새로고침 시 작성된 일기 데이터는 모두 사라지고 기존 더미 데이터만 남음***

* **휘발성 메모리 : 데이터가 초기화 되기 때문에 DB에 저장이 되어야 함**
  
  * **DB 없이 Web Storage API 를 사용하여 브라우저에서 키/값 쌍을 쿠키보다 훨씬 직관적으로 저장할 수 있는 방법**

1️⃣ **`sessionStorage`**

* 각각의 출처에 대해 독립적인 저장 공간을 페이지 세션이 유지되는 동안(브라우저가 열려있는 동안) 제공
  
  * *세션에 한정해, 즉 브라우저 또는 탭이 닫힐 때까지만 데이터를 저장*
  
  * *데이터를 절대 서버로 전송하지 않음*
  
  * 저장 공간이 쿠키보다 큼

2️⃣ **`localStorage`**

* **`sessionStorage`와 동일하지만, 브라우저를 닫았다 열어도 데이터가 남아 있음**
  
  * *유효기간 없이 데이터를 저장하고, JavaScript 를 사용하거나 브라우저 캐시 또는 로컬 저장 데이터를 지워야만 사라짐*
  
  * 저장 공간이 셋 중 제일 큼

## ▶️ 생성된 일기 데이터 저장

* **숫자, 문자열, 객체 모두 저장 가능함**
  
  * **단, 객체는 직렬화를 통해 문자열 형으로 변경한 후 저장 가능**

```javascript
useEffect(() => {
    localStorage.setItem("item1", 10);
    localStorage.setItem("item2", "20");
    localStorage.setItem("item3", JSON.stringify({ value: 30 }));
  }, []);
```

* **key 를 통해 값을 불러와 상수에 저장 가능**
  
  * *단, 불러올 때 모두 문자열 형태로 변경됨*
  
  * 숫자의 경우 `parseInt` 를 사용하여 불러와야 하며 객체는 `JSON.parse` 를 통해 복원 필요!

```javascript
useEffect(() => {
    const item1 = localStorage.getItem("item1");
    const item2 = localStorage.getItem("item2");
    const item3 = JSON.parse(localStorage.getItem("item3"));
    console.log(item1, item2, item3);
  }, []);
```

* **실시간으로 생성/수정/삭제된 데이터를 localStorage에 저장!**
  
  * 새로 고침을 눌러도 데이터가 서버 DB에 있는 것처럼 보여야 함

**① `dummyData` 모두 삭제**

* 기본값은 빈 배열로 설정

* **일기 데이터가 변경되는 로직은 `reducer` 를 거쳐가게 되어 있음!**
  
  * 생성, 수정, 삭제 등이 모두 `reducer`를 거쳐감

```javascript
import React, { useReducer, useRef } from "react";

...

// reducer 함수
const reducer = (state, action) => {
  let newState = [];
  switch (action.type) {
    case "INIT": {
      return action.data;
    }
    case "CREATE": {
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
  // localStorage 에 변경되는 데이터 저장
  localStorage.setItem("diary", JSON.stringify(newState));
  return newState;
};

export const DiaryStateContext = React.createContext();
export const DiaryDispatchContext = React.createContext();

function App() {
  // state 생성
  const [data, dispatch] = useReducer(reducer, []);

  ...
}

export default App;
```

**② 수정하기 페이지에 삭제 버튼 생성**

* `DiaryEditor.js` 수정
  
  * `localStorage` 에서도 데이터 삭제됨

```javascript
import { useState, useEffect, useRef, useContext } from "react";
...

const DiaryEditor = ({ isEdit, originData }) => {
  ...
  // onCreate, onEdit, onRemove 함수 받아오기
  const { onCreate, onEdit, onRemove } = useContext(DiaryDispatchContext);

  ...
  // 일기 삭제하기 버튼 클릭 시
  const handleRemove = () => {
    if (window.confirm("정말 삭제하시겠습니까?")) {
      onRemove(originData.id);
      navigate("/", { replace: true });
    }
  };

  ...
  return (
    <div className="DiaryEditor">
      <MyHeader
        headText={isEdit ? "일기 수정하기 " : "새 일기 쓰기"}
        leftChild={
          <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
        }
        rightChild={
          isEdit && (
            <MyButton
              text={"삭제하기"}
              type={"negative"}
              onClick={handleRemove}
            />
          )
        }
      />
      ...
    </div>
  );
};

export default DiaryEditor;
```

**③ 아직 localStorage 에는 데이터가 들어가지만 App component와는 연결이 안되어 있는 상태**

* 연결을 시켜줘야 한다!
  
  * `useEffect()` 를 사용
  
  * 새로 고침을 해도 데이터가 유지되어야 함

```javascript
import React, { useEffect, useReducer, useRef } from "react";

...

...

function App() {
  // state 생성
  const [data, dispatch] = useReducer(reducer, []);

  useEffect(() => {
    const localData = localStorage.getItem("diary");
    // 데이터가 있을 때만 화면에 보여주기
    if (localData) {
      // 내림차순으로 정렬해야 가장 위에 값이 높은 값
      const diaryList = JSON.parse(localData).sort(
        (a, b) => parseInt(b.id) - parseInt(a.id)
      );
      // 그 다음 저장할 id
      dataId.current = parseInt(diaryList[0].id) + 1;
      // 초기값으로 설정
      dispatch({ type: "INIT", data: diaryList });
    }
  });

  ...
}

export default App;
```
