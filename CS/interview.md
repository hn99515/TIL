# GET 방식과 POST 방식의 차이?

`GET`방식과 `POST`방식의 가장 큰 차이점은 프론트엔드에서 백엔드로 데이터를 전송할 때 데이터를 헤더에 담아 전달하느냐 바디에 담아 전달하느냐의 차이입니다.
`POST`방식의 경우 데이터를 바디에 넣어 보내기 때문에 기본적으로 암호화해서 전달하지만 `GET`방식은 암호화하지 않습니다.
또한 `POST`방식은 `GET`방식보다 상대적으로 큰 데이터를 전송할 수 있어서 큰 데이터를 전송할 때에는 `POST`방식을 사용합니다.

# HTTP 와 HTTPS 의 차이?

`http`와 `https`의 차이는 서버와 클라이언트 사이에 전송되는 데이터를 암호화 하느냐 그렇지 않느냐의 차이입니다. 
`https`는 서버와 클라이언트 사이의 모든 데이터를 암호화 하여 전송하기 때문에 보안에 강합니다.
하지만 암호화하는 시간이 더 걸리기 때문에 상대적으로 속도가 느리다는 단점이 있습니다.

# SPA가 무엇인가?

`SPA`는 Single page application으로 기존 웹페이지를 개발할 때에는 각각의 페이지마다 뷰(View)파일을 가졌다면 `SPA`의 구조는 하나의 뷰(View)파일에 컴포넌트를 배치하는 방식으로 페이지를 구성하는 개념 입니다.

# ORM 에 대해 설명하기

`ORM`은 Object Relational Mapping의 약자로 기존에는 SQL문을 사용하여 데이터베이스를 제어 하였다면 ORM은 객체를 데이터베이스와 직접 맵핑하여 제어하는 개념입니다.