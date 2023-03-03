# Typescript

> Typescript + React.js + styled-component 의 위력
> 
> - javascript를 기반으로 한 프로그래밍 언어
> - `javascript + 새로운 기능` = Typescript
> - **Strongly-typed 언어란?**
>   - 프로그래밍 언어가 작동하기 전에 type을 확인함!
>   - 프로그램이 작동하기 전에 뭐가 잘못됐는지 말해줌

- javascript는 데이터 타입이 무엇인지 신경 안씀
  - a와 b가 어떤 타입이어야 하는지 모름

```javascript
const plus = (a, b) => a + b;
plus(2, 2) // 4
plus(2, "hi") // '2hi'
```

- a, b는 number 자료형이어야 하는 것을 미리 알려줄 순 없을까?
  - TypeScript가 보호 기능 역할을 함 = 프로그램이 작동하기 전 데이터 타입을 확인

```javascript
const user = {
  firstName: "Angela",
  lastName: "Davis",
  role: "Professor",
}

console.log(user.name)
// Property 'name' does not exist on type '{firstName: string; lastName: String; role: string; }'.
```

```javascript
const plus = (a:number, b:number) => a + b;
plus(1, 1) // 2
plus("a", 1) // 미리 에러 발생
```

- TypeScript는 브라우저가 이해하지 못함

# TypeScript 추가하는 방법

> **`npx create-react-app [app name] --template typescript`**
> 
> - 기존 파일에 설치하는 경우  
>   : **`npm install --save typescript @types/node @types/react @types/react-dom @types/jest`**
> - TypeScript와 React 에선 `.tsx` 파일 확장자를 사용함

- 어떤 라이브러리나 패키지는 TypeScript가 아니라 JavaScript 기반이므로 오류가 날 수 있으므로 별도 설치가 필요
  - **`npm install --save-dev @types/styled-components`**
  - 설치하면 TypeScript의 별도 오류가 없어짐
  - TypeScript가 모든 것을 알아야 하므로 충돌하지 않도록 `@types` 를 설치하여 라이브러리나 패키지를 알려주는 것!

# TypeScript에게 React component를 설명

> **component를 type 한다는 것 = component에게 type을 준다는 뜻**
> 
> - TypeScript를 사용하는 이유는 코드가 실행되기 전에 오류를 확인하기 위함!

- `interface` : object shape(객체모양)을 TypeScript에게 설명해주는 개념. object가 어떤 식으로 보일지 설명하는 것.

```javascript
import styled from 'styled-components';

// required 한 bgColor = 필수적인
interface CircleProps {
  bgColor: string;
}

const Container = styled.div<CircleProps>`
  width: 200px;  height: 200px;  background-color: ${(props) => props.bgColor};  border-radius: 100px;
`;

// CircleProps의 타입이 뭔지 component에게 알리기 = bgColor의 타입은 CircleProps의 object다.
function Circle({ bgColor }: CircleProps) {
  return <Container bgColor={bgColor} />;
}

export default Circle;
```

# Optional Props

> 필수적인 것이 아닌 선택적인 props는 어떻게 정의?
> 
> - 어떤 컴포넌트에는 적용하고, 어떤 컴포넌트에는 적용하지 않는 것
> - **`?` 를 붙여서 사용**
> - 예) ContainerProps 에는 둘 다 필수적인 요소로 넣고 CircleProps 에는 하나는 선택적인 요소로 적용할 수 있다. 단, undefined 일 때 기본값(default)을 지정해두어야 오류가 나지 않음

```javascript
import styled from 'styled-components';

interface ContainerProps {
  bgColors: string;
  // required borderColor = 필수적
  borderColor: string;
}

const Container = styled.div<CircleProps>`
  width: 200px;  height: 200px;  background-color: ${(props) => props.bgColor};  border-radius: 100px;  border: 1px solid ${(props) => props.borderColor};
`;

interface CircleProps {
  bgColor: string;
  // optional borderColor = 선택적
  borderColor?: string;
  text?: string;
}

// default 값을 인자에 미리 표현 가능 = javascript 문법
function Circle({ bgColor, borderColor, text="default text" }: CircleProps) {
  // 선택적 & 필수적 둘 다 가능하므로 default 값을 주기
  // `??` = ~이 없다면 다음 걸로 적용해 
  return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>{text}</Container>;
}

export default Circle;
```

```javascript
import Circle from "./Circle";

function App() {
  return (
    <div>
      // 필수적 + 선택적 : 속성별로 모두 다름
      <Circle borderColor="yellow" bgColor="teal" />
      <Circle text="im here" bgColor="tomato" />
    </div>
  );
}

export default App;
```

# Typescript + State

> **useState 의 기본값을 통해 Typescript가 미리 type을 설정함!**
> 
> - state의 type은 대부분 변하지 않기 때문
> - *default 값이 없다면 undefined 로 표시됨*
> - *다만, type을 여러 개로 지정할 수도 있음*

## type 을 여러 개로 지정하는 방법

```javascript
import styled from 'styled-components';

function Circle({ bgColor, borderColor, text="default text" }: CircleProps) {
  // state 의 type 을 여러 개 지정할 순 있음
  const [value, setValue] = useState<number|string>(0);
  // 오류 없음
  setValue("hello")
  setValue(2)
  return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>{text}</Container>;
}

export default Circle;
```

## Typescript + Form 태그

> **'any' type도 type 중 하나이지만 Typescript에게는 하나의 type을 알려주는 것이 바람직**
> 
> - type을 넣어줄 때는 항상 구글링을 통해 찾아보자!
> - **Typescript를 활용하면 사전에 Error를 확인할 수 있다!!**

```javascript
import React, { useState } from "react";

function App() {
  const [value, setValue] = useState("")
  // Typescript는 이 onChange 함수가 InputElement에 의해서 실행될 것을 앎
  const onChange = (e: React.FormEvent<HTMLInputElement>) => { 
    const { 
      currentTarget: { value },
    } = event; 
    setValue(value)
  };

  // 제출했을 때 실행되는 함수
  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log("hello", value)
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input value={value} onChange={onChange} type="text" placeholder="username" />
        <button>Log in</button>
      </form>
    </div>
  );
}

export default App;
```

## TypeScript + Themes

> 테마와 타입스크립트를 연결하는 방법
> 
> - 설치: `npm install @types/styled-components`
> - **기본적으로 DefaultTheme 인터페이스는 비어있으므로 확장해야 함**

<적용하는 방법>

- `styled.d.ts` 파일 생성  
  : 이전에 설치해 놓은 파일을 override(덮어쓰기)

```javascript
// import original module declarations
import 'styled-components';

// and extend them!
declare module 'styled-components' {
  export interface DefaultTheme {
    // 사용할 테마를 여기에 적기
    textColor: string;
    bgColor: string;
  }
}
```

- `theme.ts` 파일 생성

```javascript
import { DefaultTheme } from 'styled-components'

export const lightTheme: DefaultTheme = {
  bgColor: 'white';
  textColor: 'black';
  btnColor: 'tomato';
}

export const darkTheme: DefaultTheme = {
  bgColor: 'black';
  textColor: 'white';
  btnColor: 'teal';
}
```

- `App.tsx` 파일에서 사용 가능

```javascript
import styled from 'styled-components'

const Container = styled.div`
  background-color: ${(props) => props.theme.bgColor}
`
const H1 = styled.h1`
  color: ${(props) => props.theme.textColor}
`

function App() {
  return (
    <Container>
      <H1>protected</H1>
    </Container>
  )
}
```


