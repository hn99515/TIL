# Node.js

> **React.js 의 기본 환경**

* *JavaScript 코드는 브라우저에 내장된 자바스크립트 엔진을 이용하여 실행된다.*
  
  * 예) safari, firefox, chrome, edge, opera 등의 브라우저는 각각의 JS 엔진이 있음

* **브라우저 내부에서만 JS가 사용되는 것이 아니라 다른 곳에서도 사용할 수 있도록 하기 위함**
  
  * JavaScript를 브라우저가 아닌 곳에서도 실행
  
  * **컴퓨터에서도 실행될 수 있음 = JavaScript로 WEB SERVER 개발도 가능!**

* 자바스크립트의 실행환경 = Javascript's Runtime = Node.js

* 브라우저가 요청(URL을 통해)하면 Web Server(Node.js)가 응답(웹 사이트)하는 구조
  
  * React는 응답 시 웹 사이트의 JavaScript 파일들을 쉽게 만들어내는 프레임워크

# Node.js 기초

* node.js 파일 실행 명령어
  
  * `node <파일명>.js`

* 모듈 내보내기를 통해 다른 파일에서도 만들어 둔 모듈을 사용할 수 있음

```javascript
// 더하기와 빼기가 가능한 파일
const add = (a,b) => a + b
const sub = (a,b) => a - b

// 다른 곳에서 위 함수를 사용하려면 모듈을 내보내야 함
// 객체 단위로 내보낼 수 있음
module.exports = {
  moduleName : "calc module",
  add: add,
  sub: sub,
}
```

```javascript
// require를 통해 다른 파일에서 내보낸 모듈 불러오기
const calc = require("./calc")

console.log(calc.add(1,2)) // 3
console.log(calc.add(4,5)) // 9
console.log(calc.sub(10,2)) // 8
```

# 패키지 생성 및 외부 모듈 사용해보기

## ▶️ npm

> Node.js의 패키지 관리 도구 (Node Package Manager)

* Package?
  
  * 누군가 만들어 둔 기능별 모듈을 의미
  
  * 예) 로그인 모듈, 전화인증 모듈, 메일 발송 모듈 등

* 생성한 모듈을 외부에서 사용하기 위해 내보내야 함 = `module.exports`

```javascript
// 더하기와 빼기가 가능한 파일
const add = (a,b) => a + b
const sub = (a,b) => a - b

// 다른 곳에서 위 함수를 사용하려면 모듈을 내보내야 함
// 객체 단위로 내보낼 수 있음
module.exports = {
  moduleName : "calc module",
  add: add,
  sub: sub,
}
```

* 내보낸 모듈을 불러와 사용해야 함 = `require`

```javascript
// require를 통해 다른 파일에서 내보낸 모듈 불러오기
const calc = require("./calc")

console.log(calc.add(1,2)) // 3
console.log(calc.add(4,5)) // 9
console.log(calc.sub(10,2)) // 8
```


