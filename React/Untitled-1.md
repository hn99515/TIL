# React 를 배우는 이유

1️⃣ 가장 많이 사용하는 프론트 프레임워크

2️⃣ React Native 라는 파생 기술을 사용할 수 있음 (태블릿, Android, IOS) 

# JavaScript

## ▶️ HTML 5

* 요소들의 배치와 내용을 기술하는 언어

* *색이나 크기 등의 디자인 수행 X*

## ▶️ CSS 3

* 색, 크기, 애니메이션 등을 정의하는 스타일링을 위한 언어

## ▶️ JavaScript

* 웹 사이트에 활력을 부여하는 언어

* 실질적으로 웹 사이트를 움직이게 함(동적인 부분)

* *단, 자바스크립트는 브라우저 내 엔진(V8)이 있어야만 실행됨*

# 변수와 상수

## ▶️ 변수 설정 규칙

* 문자, 달러(`$`), 언더스코어(`_`), 숫자로만 구성
  * *단, 숫자가 제일 앞에 나올 순 없다.*
* 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
* 예약어 사용은 불가
  * `if`, `for` 등

## ▶️ 변수 선언 키워드

* `let`
  
  * 변수 선언 시 사용
  
  * 재할당 가능, 재선언 불가능

* `const`
  
  * 상수 선언 시 사용
  
  * 재할당 및 재선언 불가능

# JavaScript 자료형

> Primitive Data Type 과 Non-Primitive Data Type으로 구분

## ▶️ Primitive Type - 원시 타입

* 한 번에 하나의 값만 가질 수 있음
  
  * `let number = 12;`

* 하나의 고정된 저장 공간을 이용

### 1️⃣ Number

> 정수형, 실수형, NaN을 나타냄

```javascript
let age = 25; // 정수
let tall = 170.5; // 실수
let inf = Infinity;
let minusInf = -Infinity;
let nan = NaN;

console.log(age + tall);
console.log(age * tall);
```

**`NaN`** (Not-A-Number)

- 숫자가 아님을 나타냄

- **`Number.isNaN()`** 의 경우
  
  - **주어진 값의 유형이 Number이고 값이 NaN이면 true, 아니면 false를 반환**

- **`NaN`을 반환하는 경우**
  
  - 숫자로서 읽을 수 없음
  
  - 결과가 허수인 수학 계산식 (`Math.sqrt(-1)`)
  
  - 피연산자가 NaN (`7 ** NaN`)
  
  - 정의할 수 없는 계산식 (`0 * Infinity`)
  
  - 문자열을 포함하면서 덧셈이 아닌 계산식

### 2️⃣ String

- 작은 따옴표 또는 큰 따옴표 모두 가능

- **곱셈, 나눗셈, 뺄셈은 안되지만 덧셈을 통해 문자열 붙일 수 있음**

```javascript
let name = "ukey";
let name2 = "이동욱";

// template literal
let name3 = `ukey ${name2}`;
console.log(name3);
```

### 3️⃣ Boolean

> 참과 거짓으로 구분하는 자료형

```javascript
let isSwitchOff = false;
```

* **조건문 또는 반복문에서 유용하게 사용**
- 조건문 또는 반복문에서 `boolean`이 아닌 데이터 타입은 자동 형변환 규칙에 따라 true or false로 반환됨

### ▶️ 형변환 (자동 & 명시적)

```javascript
let numberA = 12;
let numberB = "2";

console.log(numberA * numberB); // 24 (자동)
console.log(numberA + numberB); // 122
console.log(numberA + parseInt(numberB)); // 14 (명시적)
```

### 4️⃣ Empty Value

> 값이 존재하지 않음을 의미

- `null` 과 `undefined`가 존재

- **`null`**
  
  - **변수의 값이 없음을 의도적으로 표현할 때 사용**
  - 개발자가 의도적으로 사용

- **`undefined`**
  
  - **변수 선언 이후 직접 값을 할당하지 않으면 자동으로 할당됨**

- **둘의 차이점**❓ *`null` 이 원시 타입임에도 object로 출력됨(설계상 오류임)*

## ▶️ Primitive Type - 비원시 타입

* 한 번에 여러 개의 값을 가질 수 있음
  
  * `let array = [1,2,3,4];`

* 여러 개의 고정되지 않은 동적 공간 사용

# 연산자

## 1️⃣ 대입 연산자

```javascript
let a = 1;
```

## 2️⃣ 산술 연산자

> **사칙연산이 가능**

```javascript
let a = 1;
let b = 2;

console.log(a + b);
console.log(a * b);
console.log(a - b);
console.log(a / b);
console.log(a % b);
```

## 3️⃣ 연결 연산자

* 문자열끼리 더하기 = 문자열 합치기
  
  * 문자열 + 숫자 = (묵시적 형변환) 문자열 합치기와 동일

```javascript
let a = "1";
let b = "2";

console.log(a + b); // 12
```

## 4️⃣ 복합 연산자

> **산술 연산자와 대입 연산자를 한 번에 사용**

```javascript
let a = 5;
a += 10; // a = a + 10

console.log(a); // 15
```

## 5️⃣ 증감 연산자

> **값이 증가하거나 감소할 때 사용 (number 타입에서만 사용 가능)**

```javascript
let a = 10;
a++; // a = a + 1

console.log(a);
```

* **기호가 변수 앞/뒤에 위치할 때마다 의미가 달라짐**
  
  * 앞: 증감이 바로 적용
  
  * 뒤: 기존의 값 나온 뒤 증감 적용

```javascript
let a = 10;
console.log(a--); // 10 으로 출력한 뒤에 -1 된다. 
console.log(a); // 9
```

## 6️⃣ 논리 연산자

> **참/거짓으로 판별**

- **and = `&&`, or = `||`, not = `!`**

- 단축 평가 지원
  
  - false && true = `false`
  
  - true || false = `true`

```javascript
console.log(!true); // false
console.log(true && true); // true
console.log(true || false); // true
```

## 7️⃣ 비교 연산자

- 피연산자들(숫자, 문자, Boolean 등)을 비교하고 결과값을 true/false로 반환

- 문자열은 유니코드 값을 사용하며 표준 사전 순서를 기반으로 비교
  
  - 예) 알파벳끼리 비교
    
    - 알파벳 순서상 후순위가 더 크다.
    
    - 소문자가 대문자보다 더 크다.

```javascript
let compareA = 1 == "1";
console.log(compareA); // true

let compareB = 1 === "1";
console.log(compareB); // false

let compareC = 1 != "1";
console.log(compareC); // false

let compareD = a < A;
console.log(compareD); // false
```

## 8️⃣ typeof

> **해당 변수의 자료형을 확인**

```javascript
let compareA = 1;
compareA = "1";

console.log(typeof compareA); // string
```

## 9️⃣ null 병합 연산자

```javascript
let a;
a = a ?? 10;

console.log(a); // 10
```

* **`??` : null 이나 undefined 이면 해당 값을 병합하라는 의미**

* *변수를 선언만 할 경우 undefined 값이 나옴*

# 조건문

**연산의 결과는 참/거짓이 나오며, 그에 따른 결과를 다르게 설정 가능**

* **if ~ else if ~ else**

```javascript
let a = 5;

if (a >= 7) {
  console.log("7 이상입니다.");
} else if (a >= 5) {
  console.log("5 이상입니다.");
} else {
  console.log("5 미만입니다.");
}
```

* **switch ~ case ~ default**
  
  - **조건 표현식의 결과값이 어느 값에 해당하는지 판별**
  
  - 주로 특정 변수의 값에 따라 조건을 분기할 때 활용
    
    - 조건이 많아질 경우 if문보다 가독성이 나을 수 있음
  
  - 표현식의 결과값과 case문의 오른쪽 값을 비교
  
  - break 및 default 문은 [선택적]으로 사용 가능
  
  - *break 문이 없는 경우, break 문을 만나거나 default 문을 실행할 때까지 다음 조건문 실행*

```javascript
let country = "ko";

switch (country) {
  case "ko":
    console.log("한국");
    break;
  case "cn":
    console.log("중국");
    break;
  case "jp":
    console.log("일본");
    break;
  case "uk":
    console.log("영국");
    break;
  default:
    console.log("미분류");
    break;
}
```

# 함수

```javascript
let count = 1; // 함수 밖에서 정의 (전역 변수)

// 10개의 직사각형 넓이를 구하는 함수 = 매개변수를 통해 받아오기
// 함수 선언식 = 함수 선언 방식의 함수 생성
function getArea(width, height) {
  let area = width * height;
  console.log(count); // 함수 내부에서 접근 가능
  return area;
}

let area1 = getArea(100, 200); // 함수 호출 200
console.log("area1 : ", area1);

// 함수 내부에 있는 변수(지역변수)를 함수 밖에서는 접근할 수 없음!
console.log(area);
```

* *매개 변수와 인자 개수가 불일치하더라도 허용 = 오류 X*
  
  * 매개변수 개수 < 인자 개수
    
    ```javascript
    const noArgs = function() {
      return 0
    }
    
    noArgs(1, 2, 3) // 0
    
    const twoArgs = function(arg1, arg2) {
      return [arg1, arg2]
    }
    
    twoArgs(1, 2, 3) // [1, 2]
    ```
  
  * 매개변수 개수 > 인자 개수
    
    ```javascript
    const threeArgs = function(arg1, arg2, arg3) {
      return [arg1, arg2, arg3]
    }
    
    threeArgs()      // [undefined, undefined, undefined]
    threeArgs(1)     // [1, undefined, undefined]
    threeArgs(1, 2)  // [1, 2, undefined]
    ```

* **함수 표현식**
  
  * **함수를 값처럼 취급하여 변수에 담아 사용 가능**

```javascript
// 함수를 변수에 저장 가능
let hello = function () {
  return "안녕하세요 여러분";
};

// 해당 함수를 변수로 호출 가능
const helloText = hello();
console.log(helloText);
```

## ▶️ 함수 선언식 vs 함수 표현식

* 함수 선언식은 호이스팅 현상이 발생함

* **즉, 함수 호출 이전에 선언해도 동작**

```javascript
console.log(helloB()); // 호이스팅 현상: hello B

function helloB() {
  return "hello B";
}
```

## ▶️ 화살표 함수

> **함수 표현식을 더 간단하게 표현할 수 있는 방식 = 함축된 표현으로 사용 가능**

```javascript
let helloA = () => {
  return "안녕하세요 여러분";
};

console.log(helloA()); // 안녕하세요 여러분 
```

* 함수 표현식을 화살표 함수로 변경하는 방법
  
  - 1️⃣ (필수) **`function` 키워드 생략 가능 = `=>` 생성**
  
  - 2️⃣ (선택) **함수의 매개변수가 하나뿐이라면 `()` 도 생략 가능**
  
  - 3️⃣ (선택) **함수의 내용이 한 줄이라면 `{}`와 `return`도 생략 가능**

```javascript
const greeting = function (name) {
  return `Hi ${name}`
}

// 1단계 - function 키워드 삭제 + 화살표 추가 
const greeting1 = (name) => {
  return `Hi ${name}`
}

// 2단계 - 함수의 매개 변수가 하나라면 가능 = 보통은 이처럼 표현 
const greeting2 = name => {
  return `Hi ${name}`
}

// 3단계 - 함수의 내용이 한 줄이라면 가능
const greeting3 = name => `Hi ${name}`
```

# 콜백 함수

> **콜백 함수란 매개변수로 함수를 넘겨주는 함수**

* 함수의 파라미터로 함수를 넘겨주는 함수를 콜백 함수라고 한다.
  
  * 유연하게 사용 가능해짐

```javascript
function checkMood(mood, goodCallback, badCallback) {
  if (mood === "good") {
    // 기분 좋을 때 하는 동작
    goodCallback();
  } else {
    // 기분 안좋을 때 하는 동작
    badCallback();
  }
}

function cry() {
  console.log("ACTION :: CRY");
}

function sing() {
  console.log("ACTION :: SING");
}

function dance() {
  console.log("ACTION :: DANCE");
}

checkMood("sad", sing, cry);
```

# 객체

> Non-Primitive Type - 비원시 타입

- **key는 문자열 타입만 가능**
  
  - **key 이름에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현**

- value는 모든 타입(함수 포함) 가능

- **객체 요소 접근은 `.` 또는 `[]`로 가능**
  
  - **key 이름에 띄어쓰기 같은 구분자가 있으면 대괄호 접근만 가능**

```javascript
// 객체 리터럴 방식으로 객체 생성
let person = {
  key: "value", // 객체 프로퍼티(속성)
  key1: "value2",
  key2: true,
  key3: undefined,
  key4: [1, 2],
  key5: function () {}
};

console.log(person);
console.log(person.key1);
console.log(person.key2);
console.log(person["key4"]);
```

```javascript
let person = {
  name: "이동욱",
  age: 31
};

function getPropertyValue(key) {
  return person[key];
}
console.log(getPropertyValue("name")); // 이동욱 
```

* **프로퍼티(속성) 수정/추가 가능**

```javascript
let person = {
  name: "이동욱",
  age: 31
};

person.location = "한국"; // 프로퍼티 추가 가능
person.["gender"] = "남성"
console.log(person);
```

* **프로퍼티 삭제 가능** : `delete`로 삭제할 경우 메모리에는 삭제 안됨
  
  * `person.name = null;` 으로 작성해야만 메모리에서도 삭제된다.

```javascript
delete person.age;
delete person["name"];

console.log(person);
```

* 객체 내 함수도 호출 가능

```javascript
let person = {
  name: "이동욱",
  age: 31,
  // 객체 안의 함수 = 메서드
  say: function () {
    console.log("안녕");
  }
};

person.say();
person["say"]();

```

* **메서드 내 Template leteral 사용 가능**
  
  * `this` 활용!

```javascript
let person = {
  name: "이동욱",
  age: 31,
  // this => person 객체를 의미
  say: function () {
    console.log(`안녕 나는 ${this.name}`);
  }
};

person.say();
```

* **객체 내 없는 프로퍼티를 호출하면 undefined**

* in 연산자를 통해 객체 내 프로퍼티가 있는지 없는지 확인 가능

# 배열
