# 홈 화면 구현

> **구현 순서: 최상단 헤더 > 일기 리스트 > 필터링 기능**

## ▶️ 최상단 헤더

**① 현재 연도와 월 표시**

* **날짜를 저장하는 state 생성 후 연도와 월을 불러오기**
  
  * `getMonth()` 메서드는 0월부터 시작하므로 + 1 해야 함

```javascript
import { useState } from "react";

import MyHeader from "../components/MyHeader";

const Home = () => {
  // 날짜를 저장하는 state
  const [curDate, setCurDate] = useState(new Date());
  console.log(curDate);
  // 년월 표기
  const headText = `${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월`;

  return (
    <div>
      <MyHeader headText={headText} />
    </div>
  );
};

export default Home;
```

**② 양쪽 버튼 생성**

* 전월과 다음 월로 넘기는 기능도 추가

## ▶️ 일기 리스트

> **일기 작성 기능이 없기 때문에 처음에는 더미 데이터를 만들자**

**① 일기 리스트 역할인 더미 데이터 생성**

* App 컴포넌트의 data state의 기본값을 빈 배열이 아닌 **더미 데이터로 사용**
  
  * 더미 데이터를 Home 컴포넌트에 넣기
  
  * **일기를 하나씩 펼치기 위해 컴포넌트를 별도 생성하여 map 함수로 가공**

```javascript
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
  // dummyDate 생성
  const [data, dispatch] = useReducer(reducer, dummyData);

  ...
  };

  ...

export default App;
```

```javascript
const DiaryList = ({ diaryList }) => {
  return (
    <div>
      {diaryList.map((it) => (
        <div key={it.id}>{it.content}</div>
      ))}
    </div>
  );
};

DiaryList.defaultProps = {
  diaryList: [],
};

export default DiaryList;
```

```javascript
...
import DiaryList from "../components/DiaryList";

const Home = () => {
  // 일기 더미데이터 가져오기
  const diaryList = useContext(DiaryStateContext);

  const [data, setData] = useState([]);
  ...
  }, [diaryList, curDate]);

  ...

  return (
    <div>
      <MyHeader
        headText={headText}
        leftChild={<MyButton text={"<"} onClick={decreaseMonth} />}
        rightChild={<MyButton text={">"} onClick={increaseMonth} />}
      />
      <DiaryList diaryList={data} />
    </div>
  );
};

export default Home;
```

## ▶️ 필터 기능

> **DiaryList 의 정렬 기능(최신순)**

**① 최신순/오래된순 필터**

* DiaryList 컴포넌트에 정렬 기능을 하는 컴포넌트를 추가

```javascript
import { useState } from "react";

// 최신순, 오래된순 정렬
const sortOptionList = [
  { value: "latest", name: "최신순" },
  { value: "oldest", name: "오래된순" },
];

// 정렬 기능을 할 컴포넌트
const ControlMenu = ({ value, onChange, optionList }) => {
  return (
    <select value={value} onChange={(e) => onChange(e.target.value)}>
      {optionList.map((it, idx) => (
        <option key={idx} value={it.value}>
          {it.name}
        </option>
      ))}
    </select>
  );
};

const DiaryList = ({ diaryList }) => {
  // 정렬 기준을 저장할 state
  const [sortType, setSortType] = useState("latest");

  return (
    <div>
      <ControlMenu
        value={sortType}
        onChange={setSortType}
        optionList={sortOptionList}
      />
      {diaryList.map((it) => (
        <div key={it.id}>{it.content}</div>
      ))}
    </div>
  );
};

...

export default DiaryList;
```

**② 일기 리스트가 필터링에 맞게 정렬**

```javascript
import { useState } from "react";

...

const DiaryList = ({ diaryList }) => {
  // 정렬 기준을 저장할 state
  const [sortType, setSortType] = useState("latest");

  // 필터링에 따른 diaryList 정렬된 데이터를 반환하는 함수
  const getProcessedDiaryList = () => {
    // 정렬을 위한 비교 함수
    const compare = (a, b) => {
      if (sortType === "latest") {
        return parseInt(b.date) - parseInt(a.date);
      } else {
        return parseInt(a.date) - parseInt(b.date);
      }
    };
    // JSON 형태로 변환 후 다시 배열로 분해하여 깊은 복사 진행 = 원본 배열을 그대로 두기 위해
    const copyList = JSON.parse(JSON.stringify(diaryList));
    const sortedList = copyList.sort(compare);
    return sortedList;
  };

  return (
    <div>
      ...
      {getProcessedDiaryList().map((it) => (
        <div key={it.id}>{it.content}</div>
      ))}
    </div>
  );
};

...

export default DiaryList;
```

**③ 감정 필터 생성 및 필터링에 맞게 정렬**

* 모두(기본) / 좋은 감정만 / 안좋은 감정만
  
  * 기준
    
    * 좋은 감정: 1~3 점
    
    * 안좋은 감정: 4~5 점

```javascript
import { useState } from "react";

// 최신순, 오래된순 선택 가능
const sortOptionList = [
  ...
];

// 모두, 좋은 감정만, 안좋은 감정만을 선택 가능
const filterOptionList = [
  { value: "all", name: "모두" },
  { value: "good", name: "좋은 감정만" },
  { value: "bad", name: "안좋은 감정만" },
];

// 정렬 기능 컴포넌트 (감정 필터 기능도 똑같이 사용 가능)
const ControlMenu = ({ value, onChange, optionList }) => {
  return (
    <select value={value} onChange={(e) => onChange(e.target.value)}>
      {optionList.map((it, idx) => (
        <option key={idx} value={it.value}>
          {it.name}
        </option>
      ))}
    </select>
  );
};

const DiaryList = ({ diaryList }) => {
  // 정렬 기준을 저장할 state
  const [sortType, setSortType] = useState("latest");
  // 감정 필터를 저장할 state
  const [filter, setFilter] = useState("all");

  ...

  return (
    <div>
      ...
      <ControlMenu
        value={filter}
        onChange={setFilter}
        optionList={filterOptionList}
      />
      ...
    </div>
  );
};

...

export default DiaryList;
```

## ▶️ 버튼 생성 - 새로운 일기 쓰기

> **새로운 일기를 쓰는 버튼을 생성한 후 클릭 시 New 페이지로 이동**

* **`useNavigate` : 버튼 클릭 시 이동해줘야 할 페이지로 이동**

```javascript
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import MyButton from "./MyButton";

...

...

const DiaryList = ({ diaryList }) => {
  // 일기 생성 버튼 클릭 시 페이지 이동시키기 위함
  const navigate = useNavigate();
  ...

  ...
  };

  return (
    <div className="DiaryList">
      <div className="menu_wrapper">
        <div className="left_col">
          <ControlMenu
            value={sortType}
            onChange={setSortType}
            optionList={sortOptionList}
          />
          <ControlMenu
            value={filter}
            onChange={setFilter}
            optionList={filterOptionList}
          />
        </div>
        <div className="right_col">
          <MyButton
            type={"positive"}
            text={"일기 쓰기"}
            onClick={() => navigate("/new")}
          />
        </div>
      </div>
      ...
      ))}
    </div>
  );
};

...
export default DiaryList;
```

## ▶️ 일기 아이템의 컴포넌트 만들기

> **하나의 일기를 담당하는 컴포넌트**

* **DiaryItem 컴포넌트 생성 후 적용**

```javascript
// 모든 데이터를 가져오기
const DiaryItem = ({ id, emotion, content, date }) => {
  return <div className="DiaryItem"></div>;
};

export default DiaryItem;
```

```javascript
...
import DiaryItem from "./DiaryItem";

...

...

const DiaryList = ({ diaryList }) => {
  ...
  };

  return (
    <div className="DiaryList">
      ...
      {getProcessedDiaryList().map((it) => (
        <DiaryItem key={it.id} {...it} />
      ))}
    </div>
  );
};

...
export default DiaryList;
```

**① 감정 이미지 넣기**

* 이미지 클릭 시 상세 페이지로 이동

```javascript
// 모든 데이터를 가져오기
const DiaryItem = ({ id, emotion, content, date }) => {
  // 클릭 시 원하는 페이지로 이동
  const navigate = useNavigate();

  // 상세 페이지로 이동
  const goDetail = () => {
    navigate(`/diary/${id}`);
  };
  ...

  return (
    <div className="DiaryItem">
      {/* 감정 이미지 불러오기 */}
      <div
        onClick={goDetail}
        className={[
          "emotion_img_wrapper",
          `emotion_img_wrapper_${emotion}`,
        ].join(" ")}
      >
        <img src={process.env.PUBLIC_URL + `assets/emotion${emotion}.png`} />
      </div>
      <div></div>
      <div></div>
    </div>
  );
};

export default DiaryItem;
```

**② 일기 작성일과 내용 프리뷰**

* ms로 표현된 시간을 년.월.일. 형태로 바꿔주기

* 클릭 시 상세 페이지로 이동

```javascript
// 모든 데이터를 가져오기
const DiaryItem = ({ id, emotion, content, date }) => {
  // 클릭 시 원하는 페이지로 이동
  const navigate = useNavigate();
  // ms로 표현된 시간을 년. 월. 일. 로 나타내기
  const strDate = new Date(parseInt(date)).toLocaleDateString();

  // 상세 페이지로 이동
  const goDetail = () => {
    navigate(`/diary/${id}`);
  };

  return (
    <div className="DiaryItem">
      ...
      {/* 작성일과 내용 보여주기 */}
      <div onClick={goDetail} className="info_wrapper">
        <div className="diary_date">{strDate}</div>
        <div className="diary_content_preview">{content.slice(0, 25)}</div>
      </div>
      <div></div>
    </div>
  );
};

export default DiaryItem;
```

**③ 수정하기 버튼**

* 버튼 클릭 시 수정하기로 이동

```javascript
import { useNavigate } from "react-router-dom";
import MyButton from "./MyButton";

// 모든 데이터를 가져오기
const DiaryItem = ({ id, emotion, content, date }) => {
  // 클릭 시 원하는 페이지로 이동
  const navigate = useNavigate();
  ...

  // 상세 페이지로 이동
  const goDetail = () => {
    navigate(`/diary/${id}`);
  };
  // 수정 페이지로 이동
  const goEdit = () => {
    navigate(`/edit/${id}`);
  };

  return (
    <div className="DiaryItem">
      ...
      {/* 수정하기 버튼 */}
      <div className="btn_wrapper">
        <MyButton onClick={goEdit} text={"수정하기"} />
      </div>
    </div>
  );
};

export default DiaryItem;
```

# 새로운 일기 생성 페이지 - Create

> **일기 쓰기 버튼 클릭 시 이동하는 페이지 (/new)**

## ▶️ 최상단 헤더

> **MyHeader + MyButton 활용!**

**① MyHeader 를 불러와 제목으로 사용 + MyButton 을 불러와 뒤로가기 버튼 사용**

```javascript
import { useNavigate } from "react-router-dom";
import MyHeader from "../components/MyHeader";
import MyButton from "../components/MyButton";

const New = () => {
  const navigate = useNavigate();

  return (
    <div>
      <MyHeader
        headText={"새 일기 쓰기"}
        leftChild={
          <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
        }
      />
    </div>
  );
};

export default New;
```

## ▶️ 오늘은 언제인가요? - section

> **일기 생성 시 가장 먼저 작성할 날짜를 받을 예정**

**① 날짜를 입력할 수 있는 월 입력창 생성**

* 원하는 형태로 출력하기 위해 **`toISOString().slice(0, 10)`** 이용

```javascript
import { useState } from "react";
...

// 기본값으로 오늘의 날짜를 년-월-일 형태로 반환하는 함수
const getStringDate = (date) => {
  return date.toISOString().slice(0, 10);
};

const New = () => {
  // 연도-월-일 매핑할 state
  const [date, setDate] = useState(getStringDate(new Date()));
  const navigate = useNavigate();

  return (
    <div>
      ...
      <div>
        <section>
          <h4>오늘은 언제인가요?</h4>
          <div className="input-box">
            {/* 연도-월-일을 받을 수 있는 입력창 생성 */}
            <input
              className="input-date"
              value={date}
              onChange={(e) => setDate(e.target.value)}
              type="date"
            />
          </div>
        </section>
      </div>
    </div>
  );
};

export default New;
```

**② 수정하기 페이지와 같은 형태를 띄고 있기에 별도의 컴포넌트로 만들어 준다!** 

* components/`DiaryEditor.js` 생성
  
  * 위에서 작성한 코드 옮기기

```javascript
import { useState } from "react";
import { useNavigate } from "react-router-dom";

import MyHeader from "./MyHeader";
import MyButton from "./MyButton";

// 기본값으로 오늘의 날짜를 년-월-일 형태로 반환하는 함수
const getStringDate = (date) => {
  return date.toISOString().slice(0, 10);
};

const DiaryEditor = () => {
  // 연도-월-일 매핑할 state
  const [date, setDate] = useState(getStringDate(new Date()));
  const navigate = useNavigate();

  return (
    <div>
      <MyHeader
        headText={"새 일기 쓰기"}
        leftChild={
          <MyButton text={"< 뒤로가기"} onClick={() => navigate(-1)} />
        }
      />
      <div>
        <section>
          <h4>오늘은 언제인가요?</h4>
          <div className="input-box">
            {/* 연도-월-일을 받을 수 있는 입력창 생성 */}
            <input
              className="input-date"
              value={date}
              onChange={(e) => setDate(e.target.value)}
              type="date"
            />
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

```javascript
import DiaryEditor from "../components/DiaryEditor";

const New = () => {
  return (
    <div>
      <DiaryEditor />
    </div>
  );
};

export default New;
```

## ▶️ 오늘의 감정 - section2

> **일기 작성 시 오늘의 감정 1 ~ 5 를 선택하는 부분**

**① 오늘의 감정 5가지 이미지와 문구 불러오기**

* 감정은 하나씩 선택될 수 있고 각 상태별로 저장해야 하므로 컴포넌트로 분리

```javascript
const EmotionItem = ({ emotion_id, emotion_img, emotion_descript }) => {
  return (
    <div>
      <img src={emotion_img} />
      <span>{emotion_descript}</span>
    </div>
  );
};

export default EmotionItem;
```

```javascript
...
import EmotionItem from "./EmotionItem";

// 오늘의 감정을 받기 위한 배열 생성
const emotionList = [
  {
    emotion_id: 1,
    emotion_img: process.env.PUBLIC_URL + `/assets/emotion1.png`,
    emotion_descript: "완전 좋음",
  },
  {
    emotion_id: 2,
    emotion_img: process.env.PUBLIC_URL + `/assets/emotion2.png`,
    emotion_descript: "좋음",
  },
  {
    emotion_id: 3,
    emotion_img: process.env.PUBLIC_URL + `/assets/emotion3.png`,
    emotion_descript: "그럭저럭",
  },
  {
    emotion_id: 4,
    emotion_img: process.env.PUBLIC_URL + `/assets/emotion4.png`,
    emotion_descript: "나쁨",
  },
  {
    emotion_id: 5,
    emotion_img: process.env.PUBLIC_URL + `/assets/emotion5.png`,
    emotion_descript: "끔찍함",
  },
];

...

const DiaryEditor = () => {
  // 연도-월-일 매핑할 state
  const [date, setDate] = useState(getStringDate(new Date()));
  const navigate = useNavigate();

  return (
    <div className="DiaryEditor">
      ...
        <section>
          <h4>오늘의 감정</h4>
          <div className="input_box emotion_list_wrapper">
            {emotionList.map((it) => (
              <EmotionItem key={it.emotion_id} {...it} />
            ))}
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

**② 각 감정마다 클릭 활성화**

* 감정이 활성화된 것은 같은 색상으로 배경색 표기

* 비활성화된 것은 회색

```javascript
const EmotionItem = ({
  emotion_id,
  emotion_img,
  emotion_descript,
  onClick,
  isSelected,
}) => {
  return (
    <div
      onClick={() => onClick(emotion_id)}
      className={[
        "EmotionItem",
        isSelected ? `EmotionItem_on_${emotion_id}` : `EmotionItem_off`,
      ].join(" ")}
    >
      <img src={emotion_img} />
      <span>{emotion_descript}</span>
    </div>
  );
};

export default EmotionItem;
```

```javascript
...
import EmotionItem from "./EmotionItem";

// 오늘의 감정을 받기 위한 배열 생성
const emotionList = [
  ...
];

...

const DiaryEditor = () => {
  // 오늘의 감정을 매핑할 state
  const [emotion, setEmotion] = useState(3);
  ...

  // 감정을 클릭했을 때 매핑시켜줄 함수
  const handleClickEmote = (emotion) => {
    setEmotion(emotion);
  };

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
     ...
        <section>
          <h4>오늘의 감정</h4>
          <div className="emotion_list_wrapper">
            {emotionList.map((it) => (
              <EmotionItem
                key={it.emotion_id}
                {...it}
                onClick={handleClickEmote}
                isSelected={it.emotion_id === emotion}
              />
            ))}
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

## ▶️ 오늘의 일기 - section 3

> **작성할 일기의 내용**

* **텍스트를 받을 수 있게 세팅**

```javascript
import { useState, useRef } from "react";
...

...

const DiaryEditor = () => {
  // 오늘의 일기를 매핑할 state
  const contentRef = useRef(); // focus 기능을 위함
  const [content, setContent] = useState("");
  ...

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      ...
        <section>
          <h4>오늘의 일기</h4>
          <div className="input_box text_wrapper">
            <textarea
              placeholder="오늘은 어땠나요?"
              ref={contentRef}
              value={content}
              onChange={(e) => setContent(e.target.value)}
            />
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

## ▶️ 버튼 2개 - section 4

> **취소하기 + 작성완료 버튼**

```javascript
...

...

const DiaryEditor = () => {
  ...

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      ...
        <section>
          <div className="control_box">
            <MyButton text={"취소하기"} onClick={() => navigate(-1)} />
            <MyButton text={"작성완료"} type={"positive"} onClick={() => {}} />
          </div>
        </section>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

* 작성완료 버튼 클릭 시 유효성 검사 먼저 진행
  
  * 컨텐츠의 길이가 적절한가?

* **앱 컴포넌트의 데이터에 새로운 일기를 추가 = `onCreate` 함수 실행이 필요!**
  
  * 작성 완료 시 뒤로가기가 안먹히도록 옵션 필수 = `{ replace: true }`

```javascript
import { useState, useRef, useContext } from "react";
...
import { DiaryDispatchContext } from "../App";

...

...

const DiaryEditor = () => {
  ...

  // onCreate 함수 받아오기
  const { onCreate } = useContext(DiaryDispatchContext);

  ...

  // 작성 완료 버튼 클릭 시 실행할 함수
  const handleSubmit = () => {
    if (content.length < 1) {
      contentRef.current.focus();
      return;
    }

    onCreate(date, content, emotion);
    navigate("/", { replace: true });
  };

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      ...
        <section>
          <div className="control_box">
            <MyButton text={"취소하기"} onClick={() => navigate(-1)} />
            <MyButton
              text={"작성완료"}
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
