# Styled-Components

> npm i styled-components

```javascript
import styeld from 'styled-components'

const Father = styled.div`
  display: flex;
`
const BoxOne = styled.div`
  background-color: teal;  width: 100px;  height: 100px;
`

function App() {
  return (
    <Father>
      <BoxOne>
      </BoxOne>
    </Father>
  )
}
```

styled-components 를 사용하려면 **``(Backtick)을 이용하여 CSS를 표현!**  
의미없는 HTML 태그 사용을 줄이고 style 을 최대한 줄여 코드의 일관성을 높일 수 있음

## props를 통해 컴포넌트를 설정

> **props를 통해 동적변수 형태로 CSS를 적용할 수 있음**

```javascript
import styeld from 'styled-components'

const Father = styled.div`
  display: flex;
`
const Box = styled.div`
  background-color: ${(props) => props.bgColor};  width: 100px;  height: 100px;
`

function App() {
  return (
    <Father>
      <Box bgColor='teal'>
      <Box bgColor='tomato'>
    </Father>
  )
}
```

## 컴포넌트 확장

> **기존 컴포넌트의 모든 것을 가져와 새로운 것을 더하고 싶을 때!**
> 
> - const Circle = Styled(Box)`CSS 속성 삽입`

```javascript
import styeld from 'styled-components'

const Father = styled.div`
  display: flex;
`
const Box = styled.div`
  background-color: ${(props) => props.bgColor};  width: 100px;  height: 100px;
`
const Circle = styled(Box)`
  border-radius: 50px;
`

function App() {
  return (
    <Father>
      <Box bgColor='teal'>
      <Circle bgColor='tomato'>
    </Father>
  )
}
```

## HTML 태그만 변경하는 방법

> **속성은 동일한데 HTML 태그만 다른 것으로 사용하고 싶을 때 `as` 사용!**

```javascript
const Btn = styled.button`
  color: white;  background-color: tomato;  border: 0;  border-radius: 15px;
`;

function App() {
  return (
    <Father>
      <Btn />
      <Btn as='a' href='/' />
    </Father>
  );
}
```

## 동일한 속성을 같은 태그에 적용하는 방법

> **동일한 속성을 HTML 태그 전체에 사용하고 싶을 때 `attrs` 사용!**

```javascript
const Input = styled.input.attrs({ required: true, minLength: 10 })`
  background-color: tomato;
`;

function App() {
  return (
    <Father>
      <Input />
      <Input />
      <Input />
      <Input />
      <Input />
    </Father>
  );
}
```

# Animation

> **애니메이션 효과를 넣는 방법**
> 
> - helper method 를 추가 : 예) `keyframes`
> - 마찬가지로 ` keyframes(method명)`` ` 로 애니메이션 효과 지정
> - `${변수명}`으로 사용!

```javascript
import styled, { keyframes } from 'styled-components';

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0% {    transform: rotate(0deg);    border-radius: 0px;  }   50% {    border-radius: 100px;  }  100% {    transform: rotate(360deg);    border-radius: 0px;  }
`;

const Box = styled.div`
  height: 200px;  width: 200px;  background-color: tomato;  animation: ${rotationAnimation} 1s linear infinite;
`;

function App() {
  return (
    <Wrapper>
      <Box />
    </Wrapper>
  );
}

export default App;
```

# styled component 내 HTML 태그에 스타일 주는 방법

> **상위 태그인 styled component를 활용한 내부 HTML 태그에 스타일 주기**
> 
> - 변수 속성 내 `html 태그`를 적고 CSS를 적용
> - `&`를 활용하여 `hover` 나 `active` 등의 효과도 이어서 작성 가능

```javascript
import styled, { keyframes } from 'styled-components';

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0% {    transform: rotate(0deg);    border-radius: 0px;  }   50% {    border-radius: 100px;  }  100% {    transform: rotate(360deg);    border-radius: 0px;  }
`;

const Box = styled.div`
  height: 200px;  width: 200px;  background-color: tomato;  display: flex;  justify-content: center;  align-items: center;  animation: ${rotationAnimation} 1s linear infinite;  span {    font-size: 36px;    &:hover {      font-size: 48px;    }    &:active {      opacity: 0;    }  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <span>😀</span>
      </Box>
    </Wrapper>
  );
}

export default App;
```

# 태그 name에 의존하지 않는 방법

> **별도의 styled component 를 만들어서 사용**
> 
> - styled component 자체를 타켓팅해서도 사용 가능
> - 별도의 컴포넌트 태그를 생성한 뒤
> - 원하는 곳에 `${컴포넌트명}`을 사용하면 HTML 태그와 상관없이 스타일을 적용할 수 있음

```javascript
const Emoji = styled.span`
  font-size: 36px;
`;

const Box = styled.div`
  height: 200px;  width: 200px;  background-color: tomato;  display: flex;  justify-content: center;  align-items: center;  animation: ${rotationAnimation} 1s linear infinite;  ${Emoji}:hover {    font-size: 98px;  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <Emoji as='p'>😀</Emoji>
      </Box>
      <Emoji>😀</Emoji>
    </Wrapper>
  );
}

export default App;
```

# Theme

> **다크모드를 만들 때 사용됨**
> 
> - 다크모드 구현시 50% 정도 필요한 기술
> - `local Estate Management` 가 나머지 50%를 차지하는 기술

- Theme 이란 기본적으로 모든 색상들을 가지고 있는 object
- 모든 색깔을 하나의 object 안에 넣어났기 때문에 유용함

## 실행 방법

> **Index.js 파일에서 Import를 먼저 해야 함**
> 
> - App을 ThemeProvider로 감싸기
> - App component 안에 사용되는 component 가 `theme object에 있는 속성`에 props로 접근 가능!
> - `ThemeProvider` 에서 가져온 색들 중 구체적인 색상을 정하고, `ThemeProvider` 안에 둘러싸인 모든 component는 `ThemeProvider`에 접근할 수 있음!
> - dark/light mode를 가지려면 `property`의 이름이 똑같아야 함!

```javascript
import { ThemeProvider } from "styled-components";
...

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
}

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
}

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

```javascript
import styled from "styled-components";

const Title = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

const Wrapper = styled.div`
  display: flex;  height: 100vh;  width: 100vw;  justify-content: center;  align-items: center;  background-color: ${(props) => props.theme.backgroundColor};
`;

function App() {
  return (
    <Wrapper>
      <Title>Hello</Title>
    </Wrapper>
  )
}
```
