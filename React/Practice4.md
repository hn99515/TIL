# 최적화

> **어떤 부분에서 낭비되고 있는지 찾아내야 함**

## ▶️ 낭비되고 있는 부분 찾는 방법

1️⃣ *에디터를 통해 하나씩 체크 = 정적인 방법*

2️⃣ **프로젝트를 실행시켜 놓은 상황에서 프로그램의 힘을 빌리는 것 = 동적인 방법**

* `React Developer Tool`의 Components 를 통해 확인
  
  * **`settings - General - Highlight update ~` 한 후 재렌더링 되는 컴퍼는트 체크!**

## ▶️ 낭비되고 있는 부분

> **낭비란 재렌더링 될 필요없는 부분이 다른 작업으로 인해 재렌더링 될 때**

1️⃣ *좌, 우 버튼을 클릭하여 월별 일기를 확인할 때 아래 필터링 부분이 모두 재렌더링 중*

* **원인**
  
  * `Home.js` 내 `DiaryList.js` 가 자식 컴포넌트로 되어 있기 때문에 부모가 변경될 때 함께 재렌더링 되는 것
  
  * `DiaryList.js`가 리렌더링 되면 자식인 `ControlMenu` 도 결국 리렌더링 되는 중

* **해결책 = `React.memo()` 를 통해 `ControlMenu` 고차화 시키기**
  
  * `onChange` 함수는 항상 유의!

```javascript
import React, { useState } from "react";
...

...

// 정렬 기능 컴포넌트 (감정 필터 기능도 똑같이 사용 가능)
const ControlMenu = React.memo(({ value, onChange, optionList }) => {
  return (
    <select
      className="ControlMenu"
      value={value}
      onChange={(e) => onChange(e.target.value)}
    >
      {optionList.map((it, idx) => (
        <option key={idx} value={it.value}>
          {it.name}
        </option>
      ))}
    </select>
  );
});

const DiaryList = ({ diaryList }) => {
  ...
  };

  return (
    <div className="DiaryList">
      ...
};

DiaryList.defaultProps = {
  diaryList: [],
};

export default DiaryList;
```

2️⃣ 필터링 조건(최신/오래된순)을 바꾸면 아래 일기 데이터들이 모두 리렌더링되는 중

* **일기 데이터 간의 위치만 바뀌면 됨**

* 원인
  
  * `DiaryItem.js` 변경 = `DiaryList.js` 의 자식
  
  * `DiaryList.js` 에서 필터가 변경되면 자식 컴포넌트도 리렌더링

```javascript
import React from "react";
...

// 모든 데이터를 가져오기
const DiaryItem = ({ id, emotion, content, date }) => {
 ...

  return (
    <div className="DiaryItem">
      ...
    </div>
  );
};

export default React.memo(DiaryItem);
```

3️⃣ **수정하기 페이지**

* *오늘의 일기를 수정 시 `EmotionItem.js` 가 전체 리렌더링되는 중*
  
  * `React.memo()` 사용하더라도 `onClick` 함수가 재렌더링되는 상황
  
  * `DiaryEditor.js` 도 수정

```javascript
import { useState, useEffect, useRef, useContext, useCallback } from "react";
...

const DiaryEditor = ({ isEdit, originData }) => {
  // 오늘의 일기를 매핑할 state
  const contentRef = useRef(); // focus 기능을 위함
  const [content, setContent] = useState("");

  ...

  // 감정을 클릭했을 때 매핑시켜줄 함수
  const handleClickEmote = useCallback((emotion) => {
    setEmotion(emotion);
  }, []);

  ...

  const navigate = useNavigate();
  return (
    <div className="DiaryEditor">
      ...
    </div>
  );
};

export default DiaryEditor;
```

# 배포 준비하기

> **DEPLOY**

## ▶️ 프로젝트 점검

1️⃣ 웹 이름이 `React App`으로 되어 있으므로 변경 필요

* **`title` 및 site 성격을 나타내는 `content`(요약문) 변경하기!**

```html
<html lang="ko">
...
<meta>
  name="description"
  content="나만의 감정 일기장"
/>
...
<title>감정 일기장</title>
```

2️⃣**page 이동마다 해당하는 `title`로 변경하기**

* `Diary.js` 수정
  
  * 일기 상세페이지로 이동 시 id 붙여서 표현

```javascript
// mount 시 title element 가져온 후 해당 id번 일기로 변경
  useEffect(() => {
    const titleElement = document.getElementsByTagName("title")[0];
    titleElement.innerHTML = `감정 일기장 - ${id}번 일기`;
  }, []);
```

* `Edit.js` 수정
  
  * 일기 수정 페이지로 이동 시 id 붙여서 표현

```javascript
useEffect(() => {
    const titleElement = document.getElementsByTagName("title")[0];
    titleElement.innerHTML = `감정 일기장 - ${id}번 일기 수정`;
  }, []);
```

* `New.js` 수정
  
  * 새 일기 쓰기 페이지로 이동 시 새 일기로 표현

```javascript
useEffect(() => {
    const titleElement = document.getElementsByTagName("title")[0];
    titleElement.innerHTML = `감정 일기장 - 새 일기`;
  }, []);
```

3️⃣ **사이트의 icon 변경하기**

* `public/favicon.ico` 파일 대체하기 - 파일 변경하기만 하면 끝

## ▶️ 소스 코드 용량 줄이기

> **공백이 많으므로 소스 코드 압축 먼저 진행하기**

* **`build` 실행 = 압축하기**
  
  * **`npm run build`**

* `serve` 패키지 설치 후 서버 실행
  
  * **`npm install -g serve`**
  
  * **`serve -s build`**

* *기존 데이터 모두 삭제 후 새로고침 시 `Cannot read properties of undefined` 에러 발생*
  
  * id 를 읽을 수 없는 이야기 = 빈 배열은 truthy 하기 때문에 Error 발생

```javascript
function App() {
  // state 생성
  const [data, dispatch] = useReducer(reducer, []);

  useEffect(() => {
    const localData = localStorage.getItem("diary");
    // 데이터가 있을 때만 화면에 보여주기
    if (localData) {
      ...
      );
      // 배열 내 일기가 있을 때만 아래 코드 진행
      if (diaryList.length >= 1) {
        // 그 다음 저장할 id
        dataId.current = parseInt(diaryList[0].id) + 1;
        // 초기값으로 설정
        dispatch({ type: "INIT", data: diaryList });
      }
    }
  });
```

* *새로운 일기 생성 완료 후에도 title이 그대로 새 일기로 고정되는 에러*
  
  * 삭제한 이후에도 동일한 현상 발생

# 배포하기

> **Firebase 로 프로젝트 배포하기**

* **`Firebase` console 에서 새로운 프로젝트 추가**
  
  * 프로젝트 이름 지정 후 생성

* **Hosting 페이지로 이동**
  
  * 시작하기 - Firebase CLI 설치
    
    * **`npm install -g firebase-tools`**
  
  * 프로젝트 초기화
    
    * 터미널 창에서 웹/앱의 루트 디렉토리로 이동하거나 루트 디렉토리 생성
    
    * **구글에 로그인 : `firebase login`**
    
    * **프로젝트 시작 : `firebase init`**
  
  * `Firebase` 호스팅에 배포
    
    * 콘솔로 이동한 후 다른 사이트를 추가 : `원하는 Domain 형태로` 추가

# Open Graph

> **메타 데이터를 수정(링크 공유할 때 사용) = index.html 파일만 수정하면 된다.**

1️⃣ **링크 공유 시 썸네일 지정**

```html
<meta property='og:image' content='%PUBLIC_URL%/thumbnail.png' />
```

2️⃣ **링크 공유 시 사이트 이름이 무엇으로 보일지 결정**

```html
<meta property='og:site_name' content='감정 일기장' />
```

3️⃣ **링크 공유 시 상대에게 보여지는 부분을 결정**

```html
<meta property='og:description' content='나만의 작은 감정 일기장' />
```

* 빌드 재 완료 후 **`firebase deploy`** 진행
