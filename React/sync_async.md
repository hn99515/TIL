# 동기 & 비동기

> **순서대로 실행하는 것과 그렇지 않은 것**

## ▶️ Javascript의 작업 수행 방식 = 싱글 스레드

* **코드가 작성된 순서대로 작업을 처리**

* 이전 작업이 진행중 일 때는 다음 작업을 수행하지 않고 기다림

* 먼저 작성된 코드를 먼저 다 실행하고 나서 뒤에 작성된 코드를 실행
  
  → **동기 방식의 처리** (블로킹 방식)

* 단점
  
  * *하나의 작업이 너무 오래 걸리게 될 경우 모든 작업이 오래 걸리는 하나의 작업이 종료되기 전까지 전체가 멈추기 때문에 전반적인 흐름이 느려진다.*

## ▶️ 비동기 작업

> **싱글 스레드 방식을 이용하면서 동기적 작업의 단점을 극복**

* 여러 개의 작업을 동시에 실행시킴

* 먼저 작성된 코드의 결과를 기다리지 않고 다음 코드를 바로 실행

* **방법**
  
  * **콜백 함수를 붙여서 비동기 처리를 할 수 있음**

```javascript
function taskA() {
  // 2초 뒤에 콜백함수 실행하기
  setTimeout(() => {
    console.log("A TASK END");
  }, 2000);
}

taskA();
console.log("코드 끝");
```

* **`코드 끝`이 먼저 실행된 후 `A TASK END`가 호출된다.**

* 콜백함수를 통해 비동기 처리의 결과값을 전달 가능

```javascript
function taskA(a, b, cb) {
  // 3초 뒤에 a+b 의 값을 콜백함수로 실행하기
  setTimeout(() => {
    // 지역변수 생성
    const res = a + b;
    // 콜백함수 호출
    cb(res);
  }, 3000);
}

taskA(3, 4, (res) => {
  console.log("A TASK RESULT : ", res);
});
console.log("코드 끝");
```

```javascript
function taskA(a, b, cb) {
  // 3초 뒤에 a+b 의 값을 콜백함수로 실행하기
  setTimeout(() => {
    // 지역변수 생성
    const res = a + b;
    // 콜백함수 호출
    cb(res);
  }, 3000);
}

function taskB(a, cb) {
  // 1초 뒤에 a * 2의 값을 콜백함수로 실행하기
  setTimeout(() => {
    const res = a * 2;
    cb(res);
  }, 1000);
}

taskA(3, 4, (res) => {
  console.log("A TASK RESULT : ", res);
});

taskB(7, (res) => {
  console.log("B TASK RESULT : ", res);
});

console.log("코드 끝");
```

* **`코드 끝` > `B TASK RESULT : 14` > `A TASK RESULT : 7` 순으로 출력**

## ▶️ JavaScript의 Engine

> **JavaScript가 싱글 스레드이면서 동기와 비동기적으로 구분하여 할 수 있는 이유**

* Heap 과 Call Stack으로 구성
  
  * Heap: 메모리 할당
  
  * Call Stack: 코드 실행

* **예시) 동기 방식으로 작동하는 코드**

```javascript
function one() {
  return 1;
}

function two() {
  return one() + 1;
}

function three() {
  return two() + 1;
}

console.log(three()); // 3
```

* **Call Stack에 쌓이는 호출 순서**
  
  * `Main context` > `three()` > `two()` > `one()`

* **Call Stack에서 나가는 실행 순서**
  
  * `one()` > `two()` > `three()`
  
  * **3 이 출력되면서 `Main context` 가 Call Stack에서 빠져나가면 프로그램이 종료**

* **예시2) 비동기 방식으로 작동하는 코드**

```javascript
function asyncAdd(a, b, cb) {
  // 3초 후 함수 실행
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 3000);
}

asyncAdd(1, 3, (res) => {
  console.log("결과 : ", res); // 결과 : 4
});
```

* Call Stack 에 쌓이는 호출 순서
  
  * `Main context` > `asyncAdd()` > `setTimeout()`, `cb()`
  
  * `setTimeout()`, `cb()` 가 비동기 처리이므로 Web APIs로 이동하고 다음 코드를 실행할 수 있음
    
    * `asyncAdd()` 함수가 Call Stack 에서 빠지게 된다.
    
    * 이후 Callback Queue로 cb()가 이동함
      
      * **Event Loop에 의해 cb()가 Call Stack에 들어간 후 제거될 때 실행된다.**

* **Call Stack에서 나가는 실행 순서**
  
  * `setTimeout()` > `cb()`

* *다만, 콜백함수가 계속 길어지면 콜백 지옥에 빠질 수 있음*

## ▶️ Promise - 콜백 지옥을 탈출하기 위해

> **비동기 처리 함수에 콜백을 사용하지 않고도 비동기 처리 할 수 있는 방법**

* 비동기 작업이 가질 수 있는 3가지 상태
  
  * 1️⃣ **pending (대기 상태)**: 비동기 작업 시작 단계이며 해결하면 성공, 거부하면 실패!
  
  * 2️⃣ **fulfilled (성공)** = 해결(resolve)
  
  * 3️⃣ **rejected (실패)** = 거부(reject)

```javascript
function isPositive(number, resolve, reject) {
  // 2초 뒤 number가 양수인지 아닌지 체크하는 함수
  setTimeout(() => {
    if (typeof number === "number") {
      // 성공 -> resolve
      resolve(number >= 0 ? "양수" : "음수");
    } else {
      // 실패 -> reject
      reject("주어진 값이 숫자형이 아닙니다.");
    }
  }, 2000);
}

// 함수 호출 (성공과 실패를 콜백함수로 전달 받기)
isPositive(
  10,
  (res) => {
    console.log("성공적으로 수행됨 : ", res); // 성공적으로 수행
  },
  (err) => {
    console.log("실패했음 : ", err);
  }
);
```

* **Promise 를 통한 비동기 처리** (아래)

```javascript
function isPositiveP(number) {
  // 비동기 처리 작업을 실행하는 실행자(executor)
  const executor = (resolve, reject) => {
    setTimeout(() => {
      if (typeof number === "number") {
        // 성공 -> resolve
        console.log(number); // 101
        resolve(number >= 0 ? "양수" : "음수");
      } else {
        // 실패 -> reject
        reject("주어진 값이 숫자형이 아닙니다.");
      }
    }, 2000);
  };
  // isPositiveP를 실행시키는 방법
  // Promise 객체를 생성자로 하여 넘겨주면 자동으로 실행됨
  const asyncTask = new Promise(executor);
  return asyncTask;
}

// promise 객체를 res에 저장하면 결과값을 언제든지 사용 가능
const res = isPositiveP([]);
res
  // resolve 값을 가져다가 사용 가능
  .then((res) => {
    console.log("작업 성공 : ", res);
  })
  // reject 값을 가져다가 사용 가능
  .catch((err) => {
    console.log("작업 실패 : ", err);
  });
```

* **콜백 지옥 예시**

```javascript
function taskA(a, b, cb) {
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 3000);
}

function taskB(a, cb) {
  setTimeout(() => {
    const res = a * 2;
    cb(res);
  }, 1000);
}

function taskC(a, cb) {
  setTimeout(() => {
    const res = a * -1;
    cb(res);
  }, 2000);
}

taskA(3, 4, (a_res) => {
  console.log("task A : ", a_res); // task A : 7
  taskB(a_res, (b_res) => {
    console.log("task B : ", b_res); // task B : 14
    taskC(b_res, (c_res) => {
      console.log("task C : ", c_res); // task C : -14
    });
  });
});
```

* **콜백 지옥을 해결하기 위한 Promise 사용**

```javascript
function taskA(a, b) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a + b;
      resolve(res);
    }, 3000);
  });
}

function taskB(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * 2;
      resolve(res);
    }, 1000);
  });
}

function taskC(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * -1;
      resolve(res);
    }, 2000);
  });
}

taskA(5, 1)
  .then((a_res) => {
    console.log("A RESULT : ", a_res);
    return taskB(a_res);
  })
  .then((b_res) => {
    console.log("B RESULT : ", b_res);
    return taskC(b_res);
  })
  .then((c_res) => {
    console.log("C RESULT : ", c_res);
  });
```

## ▶️ Async & Await

> **비동기 처리 함수를 동기적으로 처리하는 방법**

* **함수 앞에 `async` 를 붙이면 비동기 처리를 하는 Promise 객체를 반환**

```javascript
function hello() {
  return "hello";
}

// async 는 자동으로 비동기 처리를 하는 Promise 객체를 반환
async function helloAsync() {
  return "hello Async";
}

console.log(hello()); // hello
console.log(helloAsync()); // Promise <pending>
```

* **비동기 처리가 가능해졌다는 의미 = `then`을 통해 반환된 객체를 사용할 수 있음**

```javascript
// then 을 통해 반환된 객체를 사용할 수 있음
helloAsync().then((res) => {
  console.log(res); // hello Async
});
```

* **`await` + 비동기 함수 = 동기적 함수처럼 처리**
  
  * **비동기 함수가 처리 완료될 때까지 아래 코드로 내려가지 않고 기다림** = 동기적
  
  * **`async` 함수 내에서만 사용 가능**

```javascript
// 시간을 끌게 하는 함수 (비동기 처리)
function delay(ms) {
  return new Promise((resolve) => {
    // resolve 만 있는 경우 콜백 함수 자체로 인자에 넣어서 사용 가능
    setTimeout(resolve, ms);
  });
}

// await + 비동기함수 = 3초 딜레이 후 출력
async function helloAsync() {
  // return delay(3000).then(() => {
  await delay(3000)
  return "hello Async";
  // });
}

helloAsync().then((res) => {
  console.log(res); // hello Async
});
```

* main 함수 안에서도 `async` 와 `await` 을 활용하여 호출 가능

```javascript
async function main() {
  const res = await helloAsync();
  console.log(res); // hello Async
}

main();
```

## ▶️ API & fetch

> **응용 프로그램 프로그래밍 인터페이스, Application Programming Interface**

* 응용 프로그램에서 사용할 수 있도록 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 의미. 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다.

* **API 호출은 Client가 원하는 것을 서버로부터 해당 데이터를 받아 응답하는 것**
  
  * API 호출은 비동기 처리한다.
    
    * 컴퓨터의 상태나 네트워크 연결 정도에 따라 언제 데이터를 받을 수 있을지 알 수 없기 때문

* **웹 구동 방식**
  
  * **<mark>Client가 서버에 요청(Request)하면</mark> 서버는 DB에 Query를 보내 원하는 데이터를 얻는다.**
  
  * **DB는 Query Result를 서버로 보내고 <mark>서버는 Client에게 응답(Response)한다.</mark>**

* **`fetch`**
  
  * API 호출을 할 수 있도록 도와주는 내장함수
  
  * API의 성공 객체 자체를 반환 = `Response` 형태의 날 것 그대로 받아옴 (정제 필요!)

```javascript
async function getData() {
  // 처음에는 날 것 그대로
  let rawResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
  // json 형태의 데이터 불러오기
  let jsonResponse = await rawResponse.json();
  console.log(jsonResponse);
}

getData();
```
