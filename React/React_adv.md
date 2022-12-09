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
  
  * Why? 처음 mount 될 때 초기값이 빈 배열인 상태에서 한 번 실행 + data가 변경되면서 getDiaryAnalysis() 호출로 한 번 실행
  
  * *수정하기를 클릭하면 감정 점수는 변경할 수 없는데 수정할 때 마다 계속 일기 분석 함수를 호출하는 필요없는 연산이 발생!* → Memoization 이 필요하다는 의미❗️
    
    * App Component의 리렌더링으로 인해 일기 분석 함수도 재호출

```javascript
  // 감정 점수를 분석: 최적화가 필요한 상황
  const getDiaryAnalysis = useMemo(
    // callback 함수 형태로 실행됨!
    () => {
      console.log("일기 분석 시작");
      // 점수별 기분 개수
      const goodCount = data.filter((it) => it.emotion >= 3).length;
      const badCount = data.length - goodCount;
      // 좋은 감정 비율
      const goodRatio = (goodCount / data.length) * 100;
      return { goodCount, badCount, goodRatio }; // 객체형으로 전달
      // 2번째 인자로 dependency array : data.length 가 변화할 때만 함수 실행!
    },
    [data.length]
  );

  const { goodCount, badCount, goodRatio } = getDiaryAnalysis();
```

* **`getDiaryAnalysis is not a function`** 이라는 에러 발생!
  
  * *함수가 아니라 콜백함수의 값을 리턴받기 때문에 값으로 사용해야 함*
  
  * **`const { goodCount, badCount, goodRatio } = getDiaryAnalysis;`**

## ▶️ React.memo - 컴포넌트 재사용

> **부모 컴포넌트인 App에서 하나의 값이 변경되더라도 리렌더링이 진행되는데 값이 변경되지 않은 자식 컴포넌트는 리렌더링 될 필요가 없음**

* *기본적으로 부모 컴포넌트가 리렌더링 되면 자식 컴포넌트도 자동으로 리렌더링 된다.*
  
  * 리렌더링 될 필요없는 부분이 있다면 최적화를 해줘야 함

* **사용법**
  
  * **자식 컴포넌트에서 해당 데이터가 변경될 때만 리렌더링 되도록 업데이트 조건을 생성**
  
  * 연산의 낭비를 막아 성능을 올려준다.

* **`React.memo`: 함수형 컴포넌트에게 업데이트 조건을 걸자**
  
  * 고차 컴포넌트다. (HOC, Higher Order Component)
  
  * 동일한 props 로 동일한 결과를 렌더링한다면 `React.memo` 를 호출하고 결과를 메모이징하도록 래핑하여 경우에 따라 선능 향상 할 수 있음

```javascript
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```

```javascript
import React, { useState, useEffect } from "react";

// 2개의 자식 컴포넌트 만들기: 데이터 prop도 진행
const TextView = ({ text }) => {
  useEffect(() => {
    console.log(`Update :: Text : ${text}`);
  });
  return <div>{text}</div>;
};

const CountView = ({ count }) => {
  useEffect(() => {
    console.log(`Update :: Count : ${count}`);
  });
  return <div>{count}</div>;
};

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [text, setText] = useState("");

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>count</h2>
        <CountView count={count} />
        <button onClick={() => setCount(count + 1)}>+</button>
      </div>
      <div>
        <h2>text</h2>
        <TextView text={text} />
        <input value={text} onChange={(e) => setText(e.target.value)} />
      </div>
    </div>
  );
};

export default OptimizeTest;
```

* *`count` 나 `text` 중 하나만 변경되더라도 자식 컴포넌트 둘 다 리렌더링 되는 낭비 발생*
  
  * 자신들 것만 Update 되면 해당 자식 컴포넌트만 리렌더링 되도록 최적화 필요!
  
  * **`React.memo` 를 통해 고차 컴포넌트로 만들면 해결**

```javascript
// 데이터가 변화하면 해당 자식 컴포넌트만 리렌더링 되도록 함!
const TextView = React.memo(({ text }) => {
  useEffect(() => {
    console.log(`Update :: Text : ${text}`);
  });
  return <div>{text}</div>;
});

const CountView = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`Update :: Count : ${count}`);
  });
  return <div>{count}</div>;
});
```

* **상태 변화가 이전과 동일한 데이터를 반환하는 상태라면?**
  
  * *`React.memo` 를 통해 리렌더링 되지 않아야 하는데 객체의 경우에는 리렌더링 됨*
  
  * **Why? 얕은 비교를 하기 때문!**

```javascript
import React, { useState, useEffect } from "react";

// 자식 컴포넌트 2개 생성 - prop으로 데이터 전달
const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CounterA Update - count: ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = React.memo(({ obj }) => {
  useEffect(() => {
    console.log(`CounterB Update - count: ${obj.count}`);
  });
  return <div>{obj.count}</div>;
});

const OptimizeTest = () => {
  // 카운트와 객체 - 버튼 클릭 시 이전과 동일한 값을 받는 예시
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({
    count: 1,
  });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <CounterB obj={obj} />
        <button
          onClick={() =>
            setObj({
              count: obj.count,
            })
          }
        >
          B button
        </button>
      </div>
    </div>
  );
};

export default OptimizeTest;
```

* **`areEqual` 함수를 사용하여 얕은 비교를 하지 않도록 할 수 있음!**
  
  * **`const areEqual = (prevProps, nextProps) => { return true }`**
  
  * 이전 데이터와 현재 데이터가 같다는 것을 의미 → 리렌더링 발생 X
  
  * 반대로 `return false` 로 실행하면 리렌더링 발생

```javascript
const CounterB = ({ obj }) => {
  useEffect(() => {
    console.log(`CounterB Update - count: ${obj.count}`);
  });
  return <div>{obj.count}</div>;
};

// 객체형인 경우 얕은 비교를 못하게 하기 위해서 별도로 areEqual 함수를 이용!
const areEqual = (prevProps, nextProps) => {
  if (prevProps.obj.count === nextProps.obj.count) {
    return true;
  }
  return false;
};

const MemoizedCounterB = React.memo(CounterB, areEqual);
```

* 별도로 Memoization 된 값을 렌더링 시켜줘야 함
  
  * **`<MemoizedCounterB obj={obj} />`**

```javascript
return (
    <div style={{ padding: 50 }}>
      ...
      <div>
        <h2>Counter B</h2>
        <MemoizedCounterB obj={obj} />
        <button
          onClick={() =>
            setObj({
              count: obj.count,
            })
          }
        >
          B button
        </button>
      </div>
    </div>
  );
};

export default OptimizeTest;
```

### 📌 [참고] 객체를 비교하는 방법

```javascript
let a = { count: 1 }
let b = { count: 1 }

if (a === b) {
    console.log("EQUAL")
} else {
    console.log("NOT EQUAL")
}
// NOT EQUAL
```

* **객체의 주소에 의한 비교 = 얕은 비교**
  
  * *각각 할당한 것이기 때문에 메모리 주소가 서로 다름*

* 그럼 얕은 비교에서 객체가 같다는 것은?

```javascript
let a = { count: 1 }
let b = a

if (a === b) {
    console.log("EQUAL")
} else {
    console.log("NOT EQUAL")
}
// EQUAL
```

## ▶️ useCallback - 컴포넌트 & 함수 재사용하기

> **일기 리스트에서 데이터를 삭제한다고 해서 일기 작성 폼이 리렌더링 될 필요는 없음**

* 개발자 도구의 Highlight 기능을 통해 어떤 컴포넌트가 리렌더링 되고 있으며 낭비되는지 확인

* (현재) `onCreate` 를 prop으로 받고 있기 때문에 저장하기 버튼 클릭 시 DiaryEditor가 리렌더링
  
  - **`React.memo` 를 통해 컴포넌트 최적화 시도**
  
  - **해당 컴포넌트 전체가 최적화 대상이면 `export default React.memo(DiaryEditor);`** 같이 표현 가능

```javascript
import React, { useEffect, useRef, useState } from "react";

// onCreate 함수를 가져오기
const DiaryEditor = ({ onCreate }) => {
  ...
};

export default React.memo(DiaryEditor);
```

* 렌더링 언제되는지 체크하기 위해 `useEffect` 사용
  * 삭제하기를 눌렀을 때 DiaryEditor 렌더링이 2번 찍히는 것을 확인
  * **Why?**
    * `App.js` 에서 초기값이 빈 배열 → 1번 렌더링 발생
    * `getData` 와 `setData` 를 통해 데이터의 변화 발생 → 1번 더 렌더링

```javascript
import React, { useEffect, useRef, useState } from "react";

// onCreate 함수를 가져오기
const DiaryEditor = ({ onCreate }) => {
  useEffect(() => {
    console.log("DiaryEditor 렌더링");
  });
  ...
```

* `useMemo`를 사용하면 안되는 이유?
  
  * 함수가 아니라 값을 반환
  
  * 추가 Hooks 의 `useCallback` 을 사용!

```javascript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

* **메모이제이션된 콜백을 반환한다!**
  
  * `onCreate` 함수를 다시 생성되지 않도록 하기
  
  * `useCallback` + `Dependency array`

```javascript
// 새로운 일기를 생성하는 함수 - 다음에 재사용할 수 있도록 useCallback 사용!
  const onCreate = useCallback((author, content, emotion) => {
    const created_date = new Date().getTime();
    // 새로운 객체로 추가
    const newItem = {
      author,
      content,
      emotion,
      created_date,
      id: dataId.current,
    };
    // 생성될 때마다 id는 +1 되어야 함
    dataId.current += 1;
    setData([newItem, ...data]); // 기존 데이터에 새로운 데이터 추가(최신순)
  }, []); // Dependency array
```

* *그러나, 저장하기 버튼을 누르면 작성한 데이터만 저장되며 모두 사라짐*
  
  * **`useCallback` 을 사용하면서 dependency array 를 빈배열로 사용했기 때문!**
  
  * 빈 배열에서 새로운 데이터 하나만 저장되었기 때문에 이러한 현상이 발생
  
  * callback 함수에 갇혀서 이전 데이터를 불러오지 못함

* 정상 작동하려면 `Dependency array` 에 미리 data를 넣어주어야 함!
  
  * *단, onCreate 함수는 한 번만 렌더링이 되어야 하는데 그러면 계속 같은 값만 가지게 됨*
  
  * 그러면 딜레마에 빠지게 되는데... 해결책은?
  
  * **함수형 업데이트(상태변화 함수에 함수를 사용)를 활용해야 함!!**
    
    * **새로운 데이터와 기존데이터를 합치는 배열을 반환하는 콜백 함수를 사용**
    
    * **`setData((data) => [newItem, ...data]);`**

```javascript
// 새로운 일기를 생성하는 함수 - 다음에 재사용할 수 있도록 useCallback 사용!
  const onCreate = useCallback((author, content, emotion) => {
    ...
    // 생성될 때마다 id는 +1 되어야 함
    dataId.current += 1;
    setData((data) => [newItem, ...data]); // 기존 데이터에 새로운 데이터 추가(최신순)
  }, []); // Dependency array
```

* (결과) DiaryEditor 가 한 번만 렌더링 되는 것을 확인할 수 있음
  
  * 삭제하기 버튼을 눌러도 다시 렌더링 되지 않음

## ▶️ 최적화 완성하기

> **최적화가 필요한 나머지 부분 모두 최적화 진행**

* (현재) 삭제하기 버튼 클릭 시 나머지 모든 데이터를 리렌더링하는 현상 발생
  
  * 삭제가 되지 않는 데이터는 리렌더링 할 필요 없음 → 최적화가 필요함!
  
  * DiaryItem 컴포넌트의 최적화가 필요
    
    * 최적화 대상: `onEdit`, `onRemove`, `content`

* **① 우선, `React.memo`로 DiaryItem 컴포넌트 설정**
  
  * **`export default React.memo(DiaryItem);`**

```javascript
import React, { useRef, useState } from "react";
// prop 된 데이터 가져오기
const DiaryItem = ({
  onEdit,
  onRemove,
  author,
  content,
  created_date,
  emotion,
  id,
}) => {
  ...
};

export default React.memo(DiaryItem);
```

* **② `useEffect` 를 활용하여 어떤 컴포넌트가 리렌더링 되는지 확인**

```javascript
import React, { useEffect, useRef, useState } from "react";
// prop 된 데이터 가져오기
const DiaryItem = ({
  onEdit,
  onRemove,
  author,
  content,
  created_date,
  emotion,
  id,
}) => {
  useEffect(() => {
    console.log(`${id}번째 아이템 렌더!`);
  });
  ...
```

* **③ `onRemove`, `onEdit` 은 onCreate 와 마찬가지로 `useCallback`을 통한 최적화 필요!**
  
  * (결과) 삭제하기 클릭 시 렌더링이 일어나지 않음
  
  * (결과2) 일기 추가 시 추가되는 데이터만 렌더링 발생

```javascript
// 배열 내 데이터 삭제 함수
  const onRemove = useCallback((targetId) => {
    // 최신 데이터를 적용하기 위해 함수형 업데이트의 인자 부분에 사용해야 함!
    setData((data) => data.filter((it) => it.id !== targetId));
  }, []);

  // 배열 내 데이터 수정 함수 - 수정 대상 id 와 수정된 데이터를 받아야 함
  const onEdit = useCallback((targetId, newContent) => {
    setData((data) => 
      // 수정할 컨텐츠의 id가 일치하면 수정 & 일치하지 많으면 it 반환 (기존 그대로)
      data.map((it) =>
        it.id === targetId ? { ...it, content: newContent } : it
      )
    );
  }, []);
```

# 복잡한 상태변화 로직 분리하기 - useReducer

> **상태 변화 처리 함수가 많은 경우 분리해보기**

* **`App.js` Component 의 상태 변화 처리함수가 3가지나 들어있음**
  
  * **`const [data, setData] = useState([]);`** 의 data 를 가져다 써야 하기 때문!
  
  * `onCreate`: 데이터 생성 상태변화 로직
  
  * `onEdit`: 데이터 수정 상태변화 로직
  
  * `onRemove`: 데이터 삭제 상태변화 로직

* **`useReducer` - 컴포넌트에서 상태변화 로직을 분리시켜준다.**
  
  * 필요성? *`useState` 를 사용할 경우 함수를 모두 표기해줘야 함*
  
  * `useReducer` 사용 시 상태 변화 로직을 외부로 분리 가능
    
    * switch ~ case 문법처럼 다양한 형태로 사용 가능

* **사용법**
  
  * **`const [count, dispatch] = useReducer(reducer, 1);`**
  
  * 0번째 인자(`count`) : state 와 동일
  
  * 1번째 인자(`dispatch`): 상태변화를 일으킬 함수
    
    * Action 객체(= 상태변화를 설명할 객체)와 함께 사용됨
  
  * `reducer`: 반드시 첫 번째 인자로 사용되며 상태변화에 대한 처리를 담당
    
    * 첫 번째 인자 `state` : 가장 최신의 데이터
    
    * 두 번째 인자 `action` : dispatch 를 통해 전달된 action 객체를 전달
  
  * `1`: count의 초기값을 의미

* **① 상태변화 함수를 `useReducer` 로 변경**

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import { useMemo, useRef, useEffect, useCallback, useReducer } from "react";

function App() {
  // 상태 변화 함수 생성
  // const [data, setData] = useState([]);

  const [data, dispatch] = useReducer(reducer, []);
```

* **② `reducer` 함수를 외부에다가 생성**
  
  * 각 함수들이 어떤 `action` type을 가지는지 체크
    
    * `getData`: initData를 API 호출 후 데이터 가공 후 한 번에 초기화 = `INIT`
    
    * `onCreate`: 생성 = `CREATE`
    
    * `onRemove`: 삭제 = `REMOVE`
    
    * `onEdit`: 수정 = `EDIT`

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import { useMemo, useRef, useEffect, useCallback, useReducer } from "react";

// reducer 함수
const reducer = (state, action) => {
  switch (action.type) {
    case "INIT":
    case "CREATE":
    case "REMOVE":
    case "EDIT":
    // default 인 경우 상태변화가 일어나지 않는 기존 데이터 반환
    default:
      return state;
  }
};


function App() {
  // 상태 변화 함수 생성
  const [data, dispatch] = useReducer(reducer, []);
```

* **③ `setData`가 했던 역할을 `reducer`에게 전달**

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import { useMemo, useRef, useEffect, useCallback, useReducer } from "react";

// reducer 함수
const reducer = (state, action) => {
  switch (action.type) {
    case "INIT": {
      return action.data;
    }
    case "CREATE": {
      // 생성 시각은 별도로 받아주기
      const created_date = new Date().getTime();
      const newItem = {
        ...action.data,
        created_date,
      };
      return [newItem, ...state];
    }
    case "REMOVE": {
      return state.filter((it) => it.id !== action.targetId);
    }
    case "EDIT": {
      return state.map((it) =>
        it.id === action.targetId ? { ...it, content: action.newContent } : it
      );
    }
    // default 인 경우 상태변화가 일어나지 않는 기존 데이터 반환
    default:
      return state;
  }
};

function App() {
  // 상태 변화 함수 생성
  const [data, dispatch] = useReducer(reducer, []);

  // 데이터의 id를 자동 생성
  const dataId = useRef(0);
  // API 호출 함수
  const getData = async () => {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    // 20개씩 나눈 뒤 각각 하나씩
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
    // 데이터 넣어주기 (type과 data를 전달)
    dispatch({ type: "INIT", data: initData });
  };
  // mount 됐을 때 API 호출
  useEffect(() => {
    getData();
  }, []);

  // 새로운 일기를 생성하는 함수 - 다음에 재사용할 수 있도록 useCallback 사용!
  const onCreate = useCallback((author, content, emotion) => {
    dispatch({
      type: "CREATE",
      data: { author, content, emotion, id: dataId.current },
    });
    // 생성될 때마다 id는 +1 되어야 함
    dataId.current += 1;
  }, []); // Dependency array

  // 배열 내 데이터 삭제 함수
  const onRemove = useCallback((targetId) => {
    // 어떤 데이터를 지울지 알려주면 되기 때문에 targetId를 전달
    dispatch({ type: "REMOVE", targetId });
  }, []);

  // 배열 내 데이터 수정 함수 - 수정 대상 id 와 수정된 데이터를 받아야 함
  const onEdit = useCallback((targetId, newContent) => {
    dispatch({ type: "EDIT", targetId, newContent });
  }, []);

  ...
}

export default App;
```

# 컴포넌트 트리에 데이터 공급 - Context

> **전역적으로 데이터를 전달하는 context API**

* `App.js` 에서 `DiaryList.js` 에 데이터를 넘겨줄 때 *`onRemove()`, `onEdit()` 함수는 그냥 거쳐가기만 하는 prop*
  
  * *React는 데이터의 흐름이 단방향이므로 어쩔 수 없이 전달만 하는 부분이 생기는 것이 원인*

* **그럼 어떤 방법으로 해결할 것인가?**
  
  * **① `App.js`가 가진 모든 데이터를 자식인 `Provider.js` 에게 전달함**
  
  * **② `Provider.js` 가 자손에 해당하는 모든 컴포넌트에게 직접 데이터를 전달할 수 있음**
  
  * **③ 자식 컴포넌트들은 모두 직통으로 데이터를 전달 받음**

* **`Provider.js` 아래 자식 컴포넌트와 데이터 흐름의 전체를 `context(문맥)`이라고 부름**
  
  * *당연히, `counter.js` 처럼 `Provider.js` 의 자손으로 속해 있지 않는다면  데이터를 직접 받을 수 없음*

* **Context 생성**
  
  * **`const MyContext = React.createContext(defaultValue);`**

* **Context Provider를 통한 데이터 공급**

```javascript
<MyContext.Provider value={전역으로 전달하고자 하는 값}>
  {/*이 context 안에 위치할 자식 컴포넌트들*/}
</Mycontext.Provider>
```

* **현재 일기 프로젝트에서 context 생성 및 내보내기**

```javascript
// context 생성 및 외부로 내보내기 (for 사용하기 위함)
export const DiaryStateContext = React.createContext();
```

* `provider`라는 컴포넌트를 사용
  
  * **return 의 최상위 태그를 `<DiaryStateContext.Provider>`를 생성**

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import React, {
  useMemo,
  useRef,
  useEffect,
  useCallback,
  useReducer,
} from "react";

...

// context 생성 및 외부로 내보내기 (for 사용하기 위함)
export const DiaryStateContext = React.createContext();

function App() {
  ...

  return (
    <DiaryStateContext.Provider>
      <div className="App">
        <DiaryEditor onCreate={onCreate} />
        <div>전체 일기: {data.length}</div>
        <div>기분 좋은 일기: {goodCount}</div>
        <div>기분 나쁜 일기: {badCount}</div>
        <div>기분 좋은 일기의 비율: {goodRatio}%</div>
        <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
      </div>
    </DiaryStateContext.Provider>
  );
}

export default App;
```

* **데이터를 공급하려면 어떻게?**
  
  * **`value` 로 prop하여 전달**
  
  * 언제든지 가져다가 쓸 수 있다는 것을 의미

```javascript
return (
    <DiaryStateContext.Provider value={data}>
      <div className="App">
        <DiaryEditor onCreate={onCreate} />
        ...
        <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
      </div>
    </DiaryStateContext.Provider>
  );
```

* DiaryList 에서 이제는 diaryList 를 prop 으로 받을 필요가 없으며 provider에서 한 번에 가져와 사용 가능

```javascript
import { useContext } from "react";
import DiaryItem from "./DiaryItem.js";
import { DiaryStateContext } from "./App";

// prop 된 데이터 받기
const DiaryList = ({ onEdit, onRemove }) => {
  // context에서 데이터 받아오기
  const diaryList = useContext(DiaryStateContext);
  return (
    <div className="DiaryList">
      <h2>일기 리스트</h2>
      <h4>{diaryList.length}개의 일기가 있습니다.</h4>
      <div>
        {/* 더미리스트의 데이터를 모두 전달 */}
        {diaryList.map((it) => (
          <DiaryItem key={it.id} {...it} onRemove={onRemove} onEdit={onEdit} />
        ))}
      </div>
    </div>
  );
};

...

export default DiaryList;
```

* onEdit, onRemove 는 거쳐가기만 하는 데이터이므로 이 부분을 최적화해보자.
  
  * *`Provider` 도 컴포넌트이므로 prop 데이터가 변경되면 재생성된다.*
  
  * **최적화가 풀리지 않도록 하려면 context를 중첩하여 사용해야 함!**
  
  * **context 파일을 하나 더 생성**

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import React, {
  useMemo,
  useRef,
  useEffect,
  useCallback,
  useReducer,
} from "react";

// reducer 함수
const reducer = (state, action) => {
  ...
};

// context 생성 및 외부로 내보내기 (for 사용하기 위함)
export const DiaryStateContext = React.createContext();
// 최적화를 위배하지 않기 위해 새로운 context를 만들어준다.
export const DiaryDispatchContext = React.createContext();

function App() {
  const [data, dispatch] = useReducer(reducer, []);

  ...

  return (
    <DiaryStateContext.Provider value={data}>
      <DiaryDispatchContext.Provider>
        <div className="App">
          ...
        </div>
      </DiaryDispatchContext.Provider>
    </DiaryStateContext.Provider>
  );
}

export default App;
```

* **`onCreate`, `onRemove`, `onEdit` 함수를 전달**
  
  * `useMemo`를 활용하여 하나의 값으로 묶어서 prop으로 전달 (재생성을 방지)
  
  * *Why? App 컴포넌트가 생성이 될 때 해당 변수도 재생성되므로 useMemo를 사용*

```javascript
import "./App.css";
import DiaryEditor from "./DiaryEditor";
import DiaryList from "./DiaryList";
import React, {
  useMemo,
  useRef,
  useEffect,
  useCallback,
  useReducer,
} from "react";

// reducer 함수
const reducer = (state, action) => {
  ...
};

// context 생성 및 외부로 내보내기 (for 사용하기 위함)
export const DiaryStateContext = React.createContext();
// 최적화를 위배하지 않기 위해 새로운 context를 만들어준다.
export const DiaryDispatchContext = React.createContext();

function App() {
  const [data, dispatch] = useReducer(reducer, []);

  ...

  // 3개의 함수를 하나로 묶어서 데이터를 전달 & 재생성 될 일 없도록 빈 배열
  const memoizedDispatches = useMemo(() => {
    return { onCreate, onRemove, onEdit };
  }, []);

  ...

  return (
    <DiaryStateContext.Provider value={data}>
      <DiaryDispatchContext.Provider value={memoizedDispatches}>
        ...
      </DiaryDispatchContext.Provider>
    </DiaryStateContext.Provider>
  );
}

export default App;
```

* **DiaryEditor 와 DiaryList 컴포넌트에서 각 함수를 prop 할 필요가 없어짐**
  
  * prop 데이터 삭제한 뒤 별도로 함수 불러와야 함

```javascript
import React, { useContext, useEffect, useRef, useState } from "react";
import { DiaryDispatchContext } from "./App";

const DiaryEditor = () => {
  // onCreate 함수를 가져오기 - 비구조화 할당으로 받기
  const { onCreate } = useContext(DiaryDispatchContext);

  ...

export default React.memo(DiaryEditor);
```

* **DiaryList 컴포넌트에서도 마찬가지로 prop 데이터 삭제 후 DiaryItem 컴포넌트 수정**

```javascript
import React, { useContext, useRef, useState } from "react";
import { DiaryDispatchContext } from "./App";

// prop 된 데이터 가져오기
const DiaryItem = ({ author, content, created_date, emotion, id }) => {
  const { onRemove, onEdit } = useContext(DiaryDispatchContext);
  ...

  return (
    <div className="DiaryItem">
      ...
    </div>
  );
};

export default React.memo(DiaryItem);
```
