# Truthy & Falsy

> **Boolean 값을 넣지 않아도 자동으로 True/False 를 구분하여 `!`를 통해 반대되는 조건을 만들 수 있음**

* **true 로 판단**
  
  * [ ], { }, 숫자, "0", "false", Infinity 

* **false 로 판단**
  
  * "", undefined, null, 0, 

```javascript
const getName = (person) => {
  return person.name; // undefined는 .으로 속성 접근 불가 
};

let person = {
  name: "이동욱"
};

const name = getName(person);
console.log(name); // 이동욱
```

```javascript
let person // undefined

const name = getName(person);
console.log(name); // TypeError!!
```

```javascript
const getName = (person) => {
  // false NOT = True 로 변경됨 
  if (!person) {
    return "객체가 아닙니다";
  }
  return person.name;
};

let person = null;

const name = getName(person);
console.log(name); // 객체가 아닙니다.
```

# 삼항 연산자

> **조건문을 한 줄로 작성하기**

- **가장 앞의 조건식이 참이면 `:` 앞의 값이 반환, 반대인 경우 `:` 뒤의 값이 반환**

- 삼항 연산자의 결과 값이기 때문에 변수에 할당 가능

```javascript
let a = 3;

if (a >= 0) {
  console.log("양수");
} else {
  console.log("음수");
}
// 삼항연산자 
a >= 0 ? console.log("양수") : console.log("음수");
```

```javascript
let a = [];

if (a.length === 0) {
  console.log("빈 배열");
} else {
  console.log("안 빈 배열");
}
// 삼항연산자 
a.length === 0 ? console.log("빈 배열") : console.log("안 빈 배열");
```

* **값이 참일 때와 거짓일 때를 구분하여 반환**

```javascript
let a = [1, 23];

const arrayStatus = a.length === 0 ? "빈 배열" : "안 빈 배열";
console.log(arrayStatus); // 안 빈 배열 
```

* **Truthy & Falsy 활용**

```javascript
let a; // undefined
// truthy & falsy 활용
const result = a ? true : false;
console.log(result); // false

let b = []; // 빈 배열은 true
const ans = b ? true : false;
console.log(ans); // true
```

* **중첩 삼항연산자**
  
  * *가급적 사용 안하는 것을 권장 = 가독성이 떨어짐*
  
  * 사용한다면 if 문으로 표현하는 것을 권장함

```javascript
// TODO = 학점 계산 프로그램
// 90점 이상 A+
// 50점 이상 B+
// 둘 다 아니면 F

let score = 100;

score >= 90
  ? console.log("A+")
  : score >= 50
  ? console.log("B+")
  : console.log("F");
```

# 단락 회로 평가

> **논리연산자의 특성을 활용**

* **단축 평가 = `&&`, `||`, `!`의 특성에 따라 앞에 조건 하나만 보고도 결과를 바로 확인 가능**

```javascript
console.log(false && true);

console.log(true || false);

console.log(!true);
```

```javascript
const getName = (person) => {
  // if (!person) {
  //   return "객체가 아닙니다."
  // }
  // 단락 회로 평가
  // person이 ture일 때만 뒤에 person.name도 평가
  // peorson이 falsy면 뒤는 확인 안함
  return person && person.name;
};

let person; // undefined
const name = getName(person);
console.log(name); // undefined
```

```javascript
const getName = (person) => {
  // 단락 회로 평가
  // person이 ture일 때만 뒤에 person.name도 평가
  // peorson이 falsy면 뒤는 확인 안함
  const name = person && person.name;
  return name || "객체가 아닙니다.";
};

let person = null;
const name = getName(person);
console.log(name); // 객체가 아닙니다. 
```

# 조건문 Upgrade

* 기존

```javascript
function isKoreanFood(food) {
  if (food === "불고기" || food === "비빔밥" || food === "떡볶이") {
    return true;
  }
  return false;
}

const food1 = isKoreanFood("불고기");
console.log(food1); // true
```

* **Upgarde : 배열 내 인자가 포함되어 있는지 체크**

```javascript
function isKoreanFood(food) {
  if (["불고기", "떡볶이", "비빔밥"].includes(food)) {
    return true;
  }
  return false;
}
const food = isKoreanFood("파스타");
console.log(food); // false
```

* 기존

```javascript
const getMeal = (mealType) => {
  if (mealType === "한식") return "불고기";
  if (mealType === "양식") return "파스타";
  if (mealType === "중식") return "멘보샤";
  if (mealType === "일식") return "초밥";
  return "굶기";
};

console.log(getMeal("한식"));
console.log(getMeal("중식"));
```

* **Upgrade = 객체로 표현하여 [ ] 표기법을 통한 값 불러오기**

```javascript
const meal = {
  한식: "불고기",
  중식: "멘보샤",
  일식: "초밥",
  양식: "스테이크",
  인도식: "카레"
};

const getMeal = (mealType) => {
  // 객체의 괄호표기법으로 값을 불러오기
  return meal[mealType] || "굶기";
};

console.log(getMeal("한식")); // 불고기
console.log(getMeal()); // 굶기
```

# 비구조화 할당

> **배열과 객체를 더 세련되게 사용**

* 기존

```javascript
let arr = ["one", "two", "three"];

let one = arr[0];
let two = arr[1];
let three = arr[2];

console.log(one, two, three); // one two three
```

* **비구조화 할당**: 순서대로 원소가 할당됨

```javascript
let [one, two, three] = ["one", "two", "three"];
console.log(one, two, three); // one two three
```

* *변수와 해당 원소의 개수가 맞지 않아도 에러 X* : undefined 로 호출

```javascript
let [one, two, three, four] = ["one", "two", "three"];
console.log(one, two, three, four); // one two three undefined
```

* 개수가 맞지 않을 때 기본값 설정 가능

```javascript
let [one, two, three, four="4"] = ["one", "two", "three"];
console.log(one, two, three, four); // one two three 4
```

* **Swap : 값을 서로 맞바꾸는 것**
  
  * 기존
    
    ```javascript
    let a = 10;
    let b = 20;
    let tmp = 0;
    
    tmp = a;
    a = b;
    b = tmp;
    console.log(a, b);
    ```
  
  * **비구조화 할당**
    
    ```javascript
    let a = 10;
    let b = 20;
    
    [a, b] = [b, a];
    console.log(a, b);
    ```

* **객체의 비구조화 할당: 키 값을 기준으로 할당**
  
  * 기존
    
    ```javascript
    let object = { one: "one", two: "two", three: "three" };
    let one = object.one;
    let two = object.two;
    let three = object.three;
    
    console.log(one, two, three);
    ```
  
  * **비구조화 할당** - 해당 값이 없으면 undefined
    
    ```javascript
    let object = { one: "one", two: "two", three: "three" };
    
    let { one, two, three } = object;
    console.log(one, two, three);
    ```

```javascript
let object = { one: "one", two: "two", three: "three" name: "ukey" };

let { name, one, two, three } = object;
console.log(one, two, three, name); // one two three ukey
```

* 변수의 이름을 바꿔서 할당 가능!

```javascript
let object = { one: "one", two: "two", three: "three", name: "이동욱" };

// key 이름 변경 가능
let { name: myName, one: oneOne, two, three } = object;
console.log(oneOne, two, three, myName);
```

# Spread 연산자 (`...`)

> **배열과 객체를 한 줄로 펼치기 (중복된 프로퍼티를 하나로!)**

* *동일한 프로퍼티를 계속해서 적어야하는 불편함* = base, madeIn 속성

```javascript
const cookie = {
  base: "cookie",
  madeIn: "korea"
};

const chocochipCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "chocochip"
};

const blueberryCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "blueberry"
};

const strawberryCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "strawberry"
};
```

```javascript
const cookie = {
  base: "cookie",
  madeIn: "korea"
};

const chocochipCookie = {
  ...cookie,
  toping: "chocochip"
};

const blueberryCookie = {
  ...cookie,
  toping: "blueberry"
};

const strawberryCookie = {
  ...cookie,
  toping: "strawberry"
};

console.log(chocochipCookie); // base, madeIn 포함해서 객체가 출력
console.log(blueberryCookie);
console.log(strawberryCookie);
```

```javascript
const noTopingCookies = ["촉촉한쿠키", "안촉촉한쿠키"];
const topingCookies = ["바나나쿠키", "블루베리쿠키", "딸기쿠기", "초코칩쿠키"];

const allCookies = [...noTopingCookies, "행운쿠키", ...topingCookies];
console.log(allCookies); // ["촉촉한쿠키", "안촉촉한쿠키", "행운쿠키", "바나나쿠키", "불루베리쿠키", "딸기쿠키", "초코칩쿠키"]
```
