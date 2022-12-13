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
