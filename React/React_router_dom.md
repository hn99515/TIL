# Router version 6~

> **version 6 이후부터 큰 변화가 발생**
> 
> - version 5 와는 다른 부분이 많이 생겼으며, react-router-dom의 철학이 변경되었음

## BrowserRouter

> version 5와 비슷함
> 
> - version 6 부터는 거의 안쓰일 예정
> - **`createBrowserRouter` 라는 새로운 API가 생겼기 때문**
> - Link를 사용하려면 Link를 Router 안에 넣어야 함
>   - Router 밖에서는 Link를 render 할 수 없기 때문
> - `Switch` 대신 `Routes` 를 사용!

```javascript
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Header from './components/Header';
import About from './screens/About';
import Home from './screens/Home';

function Router() {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='/about' element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}

export default Router;
```

## createBrowserRouter

> **Router를 array 형식으로 표현할 수 있음**
> 
> - JSX 컴포넌트를 사용하지 않고, 브라우저를 좀 더 선언적으로 바꿔준다.
> - 첫 번째 route는 전체 route들의 컨테이너와 같은 것으로 지정
> - Root를 render한 후 Root의 자식을 render 함
>   - react-router-dom에게 알려주어야 함
>   - **`Outlet` 생성**  
>     : **Root로 이동 후 자식 컴포넌트도 보여주게 만들어준다.**
> - `/`는 모두의 부모이며, 그 아래 모든 자식을 만들어 render 할 수 있음

```javascript
import { createBrowserRouter, Routes, Route } from 'react-router-dom';
import About from './screens/About';
import Home from './screens/Home';
import Root from './Root';

const router = createBrowserRouter([
  {
    // 부모 설정 후 자식을 설정
    path: '/',
    element: <Root />,
    // 자식 컴포넌트
    children: [
      {
        path: '',
        element: <Home />,
      },
      {
        path: 'about',
        element: <About />,
      },
    ],
  },
]);

export default router;
```

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import router from './Router';
import { RouterProvider } from 'react-router-dom';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

```javascript
import React from 'react';
import { Outlet } from 'react-router-dom';
import Header from './components/Header';

function Root() {
  return (
    <div>
      <Header />
      <Outlet />
    </div>
  );
}

export default Root;
```

## errorElement

> **V6 부터는 route들이 errorElement 라는 것을 가짐**
> 
> - 컴포넌트에 에러가 발생해서 충돌하거나 컴포넌트의 위치를 찾지 못할 때 사용
> - 404 에러 컴포넌트를 표기
> - **Root element path에 에러를 추가할 수 있음!**
>   - 자식을 발견하지 못하면 404 에러 페이지로 넘길 수 있음
>   - 컴포넌트끼리 충돌할 때도 에러 페이지로 넘길 수 있음
>   - 컴포넌트들을 또 다른 컴포넌트에서 발생하는 문제로부터 보호함

```javascript
import { createBrowserRouter, Routes, Route } from 'react-router-dom';
import About from './screens/About';
import Home from './screens/Home';
import Root from './Root';
import NotFound from './screens/NotFound';
import ErrorComponent from './components/ErrorComponent';

const router = createBrowserRouter([
  {
    // 부모 설정 후 자식을 설정
    path: '/',
    element: <Root />,
    // 자식 컴포넌트
    children: [
      {
        path: '',
        element: <Home />,
        // 에러가 발생한 경우 미리 막아줌 : 다른 기능에 영향을 안준다.
        errorElement: <ErrorComponent />,
      },
      ...
    ],
    // 404 에러 표기
    errorElement: <NotFound />,
  },
]);

export default router;
```

## useNavigate

> 유저를 원하는 곳에 보내는 기능
> 
> - 유저의 클릭없이 원하는 위치로 이동시키거나 변경하는 것이 가능
> - `Link`는 유저가 클릭해야 하는 것
> - 로그인 후 redirect 시키거나 어떤 페이지에 접근 권한이 없는 경우 사용!

```javascript
import { Link, useNavigate } from 'react-router-dom';

function Header() {
  const navigate = useNavigate();
  // 클릭할 때 실행되는 함수
  const onAboutClick = () => {
    navigate('/about');
  };

  return (
    <h1>
      <ul>
        ...
        <li>
          <button onClick={onAboutClick}>About</button>
        </li>
      </ul>
    </h1>
  );
}

export default Header;
```

## useParams

> **useParams 안에 내가 원하는 것 무엇이든 넣을 수 있음**
> 
> - 화면에서 URL 정보를 얻을 수 있다는 의미
>   - URL로부터 정보를 얻는다.

```javascript
import { Link } from 'react-router-dom';
import { users } from '../db';

function Home() {
  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <Link to={`/users/${user.id}`}>{user.name}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default Home;
```

```javascript
import { createBrowserRouter, Routes, Route } from 'react-router-dom';
import About from './screens/About';
import Home from './screens/Home';
import Root from './Root';
import NotFound from './screens/NotFound';
import ErrorComponent from './components/ErrorComponent';
import User from './screens/users/User';

const router = createBrowserRouter([
  {
    // 부모 설정 후 자식을 설정
    path: '/',
    element: <Root />,
    // 자식 컴포넌트
    children: [
      ...
      {
        // 동적 변수 삽입 가능
        path: 'users/:userId',
        element: <User />,
      },
    ],
    // 잘못된 url 경로인 경우 404 에러 표기
    errorElement: <NotFound />,
  },
]);

export default router;
```

```javascript
import { useParams } from 'react-router-dom';
import { users } from '../../db';

function User() {
  // url에 있는 데이터 가져오기
  const { userId } = useParams();

  return (
    <h1>
      User with it {userId} is named: {users[Number(userId) - 1].name}
    </h1>
  );
}

export default User;
```

## Outlet

> 모든 Outlet 컴포넌트는 Route의 자식들을 render 함
> 
> - **기본적으로 Root를 render 하는데, 자식이 있으면 Outlet을 Root의 자식으로 대체시킴**
> - **해당 스크린(Route)에 자식이 있다면 `Outlet`이 해당 스크린의 자식을 render 할 수 있음!**
> - 상대 경로로 표현할 때는 `/` 를 제외시켜야 함
> - 위치를 체크하거나 router도 새로 생성하지 않아도 자식 컴포넌트 불러오기 가능해짐
> - *state를 사용하면 URL에 state를 넣을 수 없음*

```javascript
import { Link, useParams, Outlet } from 'react-router-dom';
import { users } from '../../db';

function User() {
  const { userId } = useParams();

  return (
    <div>
      <h1>
        User with it {userId} is named: {users[Number(userId) - 1].name}
      </h1>
      <hr />
      {/* 상대경로이므로 / 제외한 뒤 경로 설정 */}
      <Link to='followers'>See Followers</Link>
      <Outlet />
    </div>
  );
}

export default User;
```

```javascript
import { createBrowserRouter, Routes, Route } from 'react-router-dom';
...
import Followers from './screens/users/Followers';

const router = createBrowserRouter([
  {
    // 부모 설정 후 자식을 설정
    path: '/',
    element: <Root />,
    // 자식 컴포넌트
    children: [
      ...
      {
        // 동적 변수 삽입 가능
        path: 'users/:userId',
        element: <User />,
        // 해당 유저 내 자식으로 팔로워 컴포넌트 추가
        children: [
          {
            path: 'followers',
            element: <Followers />,
          },
        ],
      },
    ],
    // 잘못된 url 경로인 경우 404 에러 표기
    errorElement: <NotFound />,
  },
]);

export default router;
```

## useOutletContext

> 자식 컴포넌트에 데이터를 넘기고 싶을 때 사용
> 
> - 부모와 자식 화면에서 공유하고 싶은 데이터가 있음을 의미
> - `useParams`를 사용하는 것은 URL 정보를 가져오는 것
> - **이외에 자식 `route`들과 소통해 보는 방법도 있음!**
>   - **`context` 속성을 통해 원하는 것 무엇이든지 보낼 수 있음**
> - 예를 들어, 다크모드와 같이 Root에서부터 context로 모든 자식 컴포넌트에게 상태를 전달할 수 있음

```javascript
import { Link, useParams, Outlet } from 'react-router-dom';
import { users } from '../../db';

function User() {
  const { userId } = useParams();

  return (
    <div>
      ...
      <hr />
      <Link to='followers'>See Followers</Link>
      {/* 해당 스크린의 자식 컴포넌트를 렌더링 시킴 */}
      <Outlet
        context={{
          nameOfMyUser: users[Number(userId) - 1].name,
        }}
      />
    </div>
  );
}

export default User;
```

```javascript
import { useOutletContext } from 'react-router-dom';

// 타입스크립트에게 자료형을 설명해야 함
interface IFollowersContext {
  nameOfMyUser: string;
}

function Followers() {
  // Outlet 으로부터 받은 데이터를 가져오기
  const { nameOfMyUser } = useOutletContext<IFollowersContext>();

  return <h1>Here are {nameOfMyUser}'s followers</h1>;
}

export default Followers;
```

## useSearchParams

> **search 파라미터를 수정하게 도와주는 hook 또는 URL에서 search 파라미터를 읽어내는 걸 도와줌**
> 
> - 검색하거나, filter 하거나, pagination 하고 싶을 때 해당 정보들을 URL에 삽입
> - **`useSearchParams()`는 array 형태로 반환**
>   - 첫 번째 아이템은 search 파라미터를 읽기 위한 것
>   - 다음 아이템은 search 파라미터를 set(수정)하기 위한 함수
> - `URLSearchParams`의 method를 사용
>   - `get`, `has` 등 필요한 메서드 사용 가능

```javascript
import { Link, useSearchParams } from 'react-router-dom';
import { users } from '../db';

function Home() {
  const [readSearchParams, setSearchParams] = useSearchParams();
  // URL 파라미터 수정도 가능! : 3초 뒤에 변경
  setTimeout(() => {
    setSearchParams({
      day: 'today',
      tomorrow: '123',
    });
  }, 3000);

  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <Link to={`/users/${user.id}`}>{user.name}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default Home;
```
