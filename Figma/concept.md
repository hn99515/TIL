# 디자인 Thinking 개념

* 목표 설정 이후

1️⃣ 공감하기: 사용자에 대한 충분한 이해

2️⃣ 정의하기: 사용자가 어떤 부분에서 어려움을 겪는지

`=> 문제점 도출` = 우선순위를 통해 문제점의 우선 순위를 정하기

3️⃣ 아이데이션: 여러 해결책을 생각

4️⃣ 프로토-타입: 가설을 검증하기 위해 시제작

5️⃣ 테스트

`=> 문제 해결 `

성공한다면 더 좋은 방법을 강구하는 것이며, 실패한다면 또 다른 가설을 수립하여 문제 해결 시도

## ▶️ 프로토 타입

> 사용자에게 테스트해보기 위한 제품의 시뮬레이션, 또는 샘플 버전

* 프로토타입의 종류
  
  시각적 완성도가 낮을수록 수정 및 업데이트가 쉽고 시간이 별로 걸리지 않음
  
  * Lo-Fi (Low Fidelity)
  
  * Mid-Fi (Mid Fidelity)
  
  * Hi-Fi (High-Fidelity)

## ▶️ 로우파이 프로토타입

* 스케치 = 손으로 그리기
  
  * 빠른 실행과 반복, 커뮤니케이션에 좋은 방법
  
  * 서로 간의 이해를 높이기 위한 방법 = 커뮤니케이션의 오해를 줄일 수 있음

## ▶️ 미드파이 프로토타입

* 와이어프레임
  
  * **텍스트, 버튼 등 화면에 대한 구성에 대해 정의하고, 화면 간의 플로우를 표현**
    
    * work-flow 를 표시
  
  * 최종버전인 UI 디자인 대비 빠른 수정 및 커뮤니케이션에 좋음
  
  * 텍스트 사이즈, 색상 등은 추후에 진행하며 최대한 기능에 초점을 맞춤

## ▶️ 하이파이 프로토타입

* UI 디자인
  
  * 사용자가 실제로 사용하게 될 높은 퀄리티의 디자인 산출물, 색, 폰트 및 폰트 사이즈, 아이콘 등 세부 사항이 적용됨

## ▶️ 프로토타이핑 (Prototyping)

* 실제 개발이 된 것은 아니지만, 사용자 테스트 단계에서 사용자의 피드백을 얻거나 내부 커뮤니케이션을 위해 제작

## ▶️ 디자인 핸드오프 (Hand-off)

* UI 디자인이 완료된 후 실제품 개발을 위해 개발자에게 전달하는 디자인 산출물

# 시각 계층

> 시각 정보의 계층

* UI 디자인이란 = 시각정보의 룰을 만들어주는 것
  
  * 더 좋은 정보를 더 빨리 볼 수 있도록 유도해야 함
  
  * 사용자가 다 읽지 않더라도 중요한 내용은 모두 알 수 있도록 도와줌

* 기본 원리
  
  * 중요도(우선 순위)에 따라 정보가 더 잘 보이도록 하는 것 = 크기나 색상을 통해❗️
  
  * 제목, 가격 등이 더 중요하면 눈에 더 띄도록 디자인

# BITMAP vs VECTOR

## ▶️ 비트맵 (Bitmap)

> 픽셀(점)이 모여 이루어진 그림

* 확대하면 깨져서 보임

* 대표 포맷: JPG, PNG

* 대표 소프트웨어: 포토샵

## ▶️ 벡터 (Vector)

> 선분과 면으로 이루어진, 수학적 연산에 의해 이루어진 그림

* 확대해도 깨지지 않음 = 다양한 케이스(크기 조절)에서 오브젝트를 활용할 수 있음

* 다양한 화면 사이즈를 위한 디자인에 최적화됨

* 대표 포맷: AI, SVG

* 대표 소프트웨어: 피그마, 일러스트레이터

# 레이어란?

> 그림을 그릴 수 있는 투명 필름으로 이해하면 쉬움

* 1개의 레이어는 1장의 투명 필름
  
  * 각각의 레이어에 그릴 수 있으며 합쳐서 표현할 수도 있음

* 수정하거나 유지보수에 용이하기에 레이어를 사용

# 피그마의 디자인 작업 환경

* 좌측 패널 = 페이지 및 레이어, 그림 요소, 에셋 관리

* 상단 네비게이션 = 이미지, 텍스트, 도형 등 오브젝트 제작을 위한 시발점 / Figma 메뉴

* 페이지 캔버스 영역 = 제작한 오브젝트들을 보여주는 공간

* 우측 패널 = 각 오브젝트에 대한 속성 관리

## ▶️ 작업 공간의 계층

1. 페이지 캔버스 = 가장 큰 단위

2. 프레임 = 내가 보여줄 디바이스의 화면 해상도

3. 레이어 = 각 컴포넌트

## ▶️ 단축키

> Mac 기준 (? > keyboardshorcut 에 들어가면 나옴)

* Essential
  
  * `command + \` = 좌우측 패널 및 상단 네비게이션 보여주기/숨기기
  
  * `ctrl + C` = 색상 확인
  
  * `command + /` = 메뉴, 커맨드, 플러그인 찾기

* Tools
  
  * `V` = tool 이동하기
  
  * `F` = frame tool
  
  * `P` = pen tool
  
  * `shift + P` = pencil tool
  
  * `T` = Text tool
  
  * `R` = Rectangle tool
  
  * `O` = Ellipse tool
  
  * `L` = Line tool
  
  * `shift + L` = Arrow tool
  
  * `C` = 주석 달기
  
  * `S` = Slice tool

* View
  
  * `alt/option + command + \` = 멀티플레이어 커서
  
  * `shift + R` = Rulers
  
  * `shift + O` = 외부 테두리만 보이기/가리기
  
  * `shift + command + P` = pixel preview
  
  * `shift + G` = layout Grids
  
  * `shift + '` = pixel Grid

# 화면 보기 관련 단축키

* **줌인/줌아웃**
  
  * 마우스 패드 핀치 인/아웃 동작
  
  * 우측 상단 % 변경
  
  * 2배 단위 줌인/줌아웃 = `+` 또느 `-`

* **해상도 100% 만들기**
  
  * MacOS = `Command + 0`
  
  * Windows = `Ctrl + 0`

* **화면 이동**
  
  * 마우스 패드 두손가락 터치후 드래그
  
  * 마우스 스크롤 버튼 클릭 후 드래그
  
  * `Space`를 누른 상태에서 마우스 드래그

* **Figma UI 숨기기/보이기**
  
  * MacOS = `Command + \`
  
  * Windows = `Ctrl + \`
