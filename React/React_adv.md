# Lifecycle 제어하기 - useEffect

> **React의 컴포넌트는 생명주기를 가진다.**

* **생명 주기에 따른 제어를 추가적으로 할 수 있음**

* Class React Component Only: *클래스형 컴포넌트에서만 사용 가능! (함수형 컴포넌트는 X)*

## ▶️ Reacat 컴포넌트의 생명 주기

1️⃣ **Mount : 화면에 나타나는 것**

* mehod = **`ComponentDidMount`**

* 예) 초기화 작업

2️⃣ **Update : 화면에 변화가 일어나는 것 (업데이트, 리렌더)**

* mehod = **`ComponentDidUpdate`**

* 예) 예외 처리 작업

3️⃣ **UnMount : 화면에서 사라지는 것**

* mehod = **`ComponentWillUnmount`**

* 예) 메모리 정리 작업

## ▶️ React Hooks

> **클래스형 컴포넌트에서만 사용할 수 있는 기능을 함수형 컴포넌트에서도 사용할 수 있도록 함**

* 예) `useState`, `useEffect`, `useRef`

* *Class 형 컴포넌트의 코드가 길어지는 문제가 발생*
  
  * *중복 코드, 가독성 문제 등을 해결하기 위해 등장한 개념*

## ▶️ useEffect

> **Lifecycle 을 제어하기 위한 React Hooks**

* **사용법**
  
  * **`//todo...` : callback 함수를 넣어 하고자 하는 동작을 표현**
  
  * **`[]`** : Dependency Array (의존성 배열)
    
    * **이 배열 내에 들어있는 값이 변화하면 콜백 함수가 수행됨**

```javascript
import React, { useEffect } from "react";

useEffect(() => { 
  // todo...
}, [])
```

* **Mount: 화면이 나타날 때 특정 행동을 실행**
  
  * 콜백함수의 2번째 인자로 `[]` 빈 배열을 넣으면 mount 시점을 의미

```javascript
import React, { useEffect, useState } from "react";

const Lifecycle = () => {
  // Test용 (카운트와 input)
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // Lifecycle 실행 - mount 시점에 특정 행동
  useEffect(() => {
    console.log("Mount!");
  }, []);

  return (
    <div style={{ padding: 20 }}>
      <div>
        {count}
        <button onClick={() => setCount(count + 1)}>+</button>
      </div>
      <div>
        <input value={text} onChange={(e) => setText(e.target.value)} />
      </div>
    </div>
  );
};

export default Lifecycle;
```

* **Update: 화면이 업데이트 될 때 마다 특정 행동을 실행**
  
  * state가 변경되거나 부모에서 내려받는 props 데이터가 바뀌거나 부모 컴포넌트가 리렌더링 되는 경우를 의미
  
  * 2번째 인자에 아무것도 적지 않으면 됨
  
  * **배열 내 변화되는 값을 넣으면 그 값이 변화할 때마다 함수 호출함**
    
    * **변화를 감지하고 싶은 것만 설정할 수 있다는 의미**

```javascript
import React, { useEffect, useState } from "react";

const Lifecycle = () => {
  // Test용 (카운트와 input)
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  useEffect(() => {
    console.log("Update!");
  });

  useEffect(() => {
    console.log(`count is update : ${count}`);
  }, [count]);

  useEffect(() => {
    console.log(`text is update : ${text}`);
  }, [text]);

  return (
    ...
  );
};

export default Lifecycle;
```

* **Unmount: 화면이 꺼지기 전에 특정 행동을 실행**
  
  * mount에서 return 함수를 하나 더 넣어주면 Unmount 시점에 실행

```javascript
import React, { useEffect, useState } from "react";

const UnmountTest = () => {
  useEffect(() => {
    console.log("Mount!");
    // Unmount 시점에 실행됨
    return () => {
      console.log("Unmount!");
    };
  }, []);
  return <div>Unmount Testing Component</div>;
};

const Lifecycle = () => {
  const [isVisible, setIsVisible] = useState(false);
  const toggle = () => setIsVisible(!isVisible);

  return (
    <div style={{ padding: 20 }}>
      <button onClick={toggle}>ON/OFF</button>
      {/* 단락회로평가 = isVisible이 True일 때만 실행 */}
      {isVisible && <UnmountTest />}
    </div>
  );
};

export default Lifecycle;
```

# API 호출하기

> **useEffect를 이용하여 컴포넌트 Mount 시점에 API를 호출하고 해당 API의 결과값을 일기 데이터의 초기값으로 이용하기!**

* **API 호출 함수**

```javascript
// API 호출 함수
  const getData = async () => {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    console.log(res);
  };
```

* **mount 되면 바로 API 호출하기**
  
  * 데이터 500개 받아옴

```javascript
// mount 됐을 때 API 호출
  useEffect(() => {
    getData();
  }, []);
```

* **일기 데이터의 초기값으로 활용**

```javascript
import { useRef, useState, useEffect } from "react";

function App() {
  ...
  // API 호출 함수
  const getData = async () => {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    // 20개씩 나눈 뒤 각각 하나씩 접근 
    const initData = res.slice(0, 20).map((it) => {
      return {
        author: it.email,
        content: it.body,
        // 0~4까지 랜덤으로 실수형 숫자가 나오면 floor한 뒤 +1 (1~5까지 나옴)
        emotion: Math.floor(Math.random() * 5) + 1,
        created_data: new Date().getTime(),
        id: dataId.current++,
      };
    });
    // 데이터 넣어주기
    setData(initData);
  };
```

# React developer tools

> **Chrome의 확장도구**

* 개발자 도구 내 `Components` 와 `Profiler` 생성되어 React로 만든 page를 쉽게 확인 가능
  
  * **`Components`: State, Ref, Effect, props 등 많은 내용을 알 수 있음!**
  
  * **settings - Highlight update~ 부분 체크: rendering 되는 부분이 어디인지 확인 가능**

# 최적화

## ▶️ useMemo

> **연산 결과를 재사용 하는 방법**

* **현재 일기 데이터를 분석하는 함수를 제작하고 해당 함수가 일기 데이터의 길이가 변화하지 않을 때 값을 다시 계산하지 않도록 하기**
  
  * +Memoization 이해하기!

* **Memoization**?
  
  * **이미 계산 해 둔 연산 결과를 기억해 두었다가 동일한 계산을 시키면, 다시 연산하지 않고 기억해 두었던 데이터를 변환시키게 하는 방법** → 연산 과정을 최적화

```javascript
// 감정 점수를 분석: 최적화가 필요한 상황
  const getDiaryAnalysis = () => {
    console.log("일기 분석 시작");
    // 점수별 기분 개수
    const goodCount = data.filter((it) => it.emotion >= 3).length;
    const badCount = data.length - goodCount;
    // 좋은 감정 비율
    const goodRatio = (goodCount / data.length) * 100;
    return { goodCount, badCount, goodRatio }; // 객체형으로 전달
  };

  const { goodCount, badCount, goodRatio } = getDiaryAnalysis();

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <div>전체 일기: {data.length}</div>
      <div>기분 좋은 일기: {goodCount}</div>
      <div>기분 나쁜 일기: {badCount}</div>
      <div>기분 좋은 일기의 비율: {goodRatio}</div>
      <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
    </div>
  );
}

export default App;
```

* *"일기 분석 시작" 이라는 것이 2번 출력된다.*
  
  * Why? 처음 mount 될 때 한 번 + getDiaryAnalysis() 호출로 한 번
  
  * *수정하기를 클릭하면 감정 점수는 변경할 수 없는데 수정할 때 마다 계속 일기 분석 함수를 호출하는 필요없는 연산이 발생!* → Memoization 이 필요!

```java

```


