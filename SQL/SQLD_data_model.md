# 데이터 모델링의 이해

## ▶️ 엔터티의 개념

> 변별할 수 있는 사물, DB 내에서 변별 가능한 객체, 정보를 저장할 수 있는 어떤 것

* 엔터티는 사람, 장소, 물건, 사건, 개념 등의 명사에 해당

* 엔터티는 업무상 관리가 필요한 관심사에 해당

* 엔터티는 저장이 되기 위한 어떤 것
  
  * 업무에 필요하고 유용한 정보를 저장하고 관리하기 위한 집합적인 것

* 엔터티는 그 집합에 속하는 개체들의 특성을 설명할 수 있는 속성을 갖는다.

* 엔터티는 인스턴스의 집합 = 인스턴스라는 것은 엔터티의 하나의 값에 해당

* 눈에 보이지 않는 개념에 대해서도 엔터티로서 인식할 수 있다.

## ▶️ 엔터티의 특징

* 반드시 해당 업무에서 필요하고 관리하고자 하는 정보이어야 한다.
  
  * 예) 환자, 토익의 응시횟수

* 유일한 식별자에 의해 식별이 가능해야 함

* 영속적으로 존재하는 인스턴스의 집합이어야 한다 (단, 두 개 이상!)

* 엔터티는 업무 프로세스에 의해 이용되어야 함

* 엔터티는 반드시 속성이 있어야 함

* 엔터티는 다른 엔터티와 최소 한 개 이상의 관계가 있어야 함

## ▶️ 엔터티의 분류

* 유무형에 따른 분류 - 유형, 개념, 사건
  
  * 유형 엔터티: 물리적인 형태가 있고 안정적이며 지속적으로 활용되는 엔터티
    
    * 예) 사원, 물품, 강사 등
  
  * 개념 엔터티: 물리적인 형태로 존재하지 않고 관리해야 할 개념적 정보로 구분되는 엔터티
    
    * 예) 조직, 보험상품 등
  
  * 사건 엔터티: 업무를 수행함에 따라 발생하는 엔터티, 발생량이 많으며 각종 통계자료에 이용
    
    * 예) 주문, 청구, 미납 등

* 발생 시점에 따른 분류 - 기본/키, 중심, 행위
  
  * 기본엔터티(키)
    
    * 업무에 원래 존재하는 정보로서 다른 엔터티와 관계에 의해 생성되지 않고 독립적으로 생성 가능하고 자신은 타 엔터티의 부모의 역할을 함
    
    * 다른 엔터티로부터 주식별자를 상속받지 않고 자신의 고유한 주식별자를 가짐
    
    * 예) 사원, 부서, 고객, 상품, 자재 등
  
  * 중심엔터티
    
    * 기본엔터티로부터 발생하고, 업무에 있어서 중심적인 역할을 함
    
    * 데이터의 양이 많이 발생되고 다른 엔터티와의 관계를 통해 많은 행위엔터티를 생성함
    
    * 예) 계약, 사고, 예금원장, 청구, 주문, 매출 등
  
  * 행위엔터니
    
    * 두 개 이상의 부모 엔터티로부터 발생되고 자주 내용이 바뀌거나 데이터량이 증가함
    
    * 분석 초기 단계에서는 잘 나타나지 않고 상세 설계단계나 프로세스와 상관모델링을 진행하면서 도출될 수 있음
    
    * 예) 주문목록, 사원변경이력 등

### ▶️ 엔터티의 명명

1. 가능하면 현업업무에서 사용하는 용어를 사용

2. 가능하면 약어를 사용하지 않는다.

3. 단수명사를 사용

4. 모든 엔터티에서 유일하게 이름이 부여되어야 함

5. 엔터티 생성의미대로 이름을 부여함

# 속성

## ▶️ 속성의 개념

> 사물이나 개념이 어떤 것인지를 나타내고 그것을 다른 것과 구별하는 성질

* 업무에서 필요로 하는 인스턴스로 관리하고자 하는 의미상 더 이상 분리되지 않는 최소의 데이터 단위! = 업무상 관리하기 위한 최소의 의미 단위이며 엔터티에서 한 분야를 담당

* 엔터티를 설명하고 인스턴스의 구성요소가 된다.

* 예) 생년월일, 

## ▶️ 엔터티, 인스턴스, 속성, 속성값의 관계

* 한 개의 엔터티는 두 개 이상의 인스턴스의 집합이다.

* 한 개의 엔터티는 고유위 성격을 표현하는 속성 정보를 두 개 이상 갖는다.
  
  * 예) 사원 - 이름, 주소, 전화번호, 직책 등

* 한 개의 속성은 한 개의 속성값을 가진다.

* 각각의 인스턴스는 속성의 집합으로 설명

* 하나의 속성은 하나의 인스턴스에만 존재할 수 있음

* 속성은 관계로 기술될 수 없고 자신이 속성을 가질 수도 없음

* 엔터티 내에 있는 하나의 인스턴스는 각각의 속성들의 대해 한 개의 속성값만을 가질 수 있음

## ▶️ 속성의 특징

* 엔터티와 마찬가지로 반드시 해당 업무에서 필요하고 관리하고자 하는 정보이어야 함
  
  * 예) 강사의 교재 이름

* 정규화 이론에 근간하여 정해진 주식별자에 함수적 종속성을 가져야 함

* 하나의 속성에는 한 개의 값만을 가짐. 하나의 속성에 여러 개의 값이 있는 다중값일 경우 별도의 엔터티를 이용하여 분리함

## ▶️ 속성의 분류

* 속성에 따른 분류
  
  1) 기본 속성
     
     * 업무로부터 추출한 모든 속성이 여기에 해당하며 엔터티에 가장 일반적이고 많은 속성을 차지
     
     * 코드성 데이터, 엔터티를 식별하기 위해 부여된 일련번호, 그리고 다른 속성을 계산하거나 영향을 받아 생성된 속성을 제외한 모든 속성은 기본속성이다.
  
  2* 설계속성
  
  * 업무상 필요한 데이터 이외에 데이터 모델링을 위해, 업무를 규칙화하기 위해 속성을 새로 만들거나 변형하여 정의하는 속성이다. 코드성 속성은 원래 속성을 업무상 필요에 의해 변형하여 만든 설계속성이고 일련번호와 같은 속성은 단일한 식별자를 부여하기 위해 모델 상에서 새로 정의하는 설계 속성이다.
  
  3* 파생속성
  
  * 다른 속성에 영향을 받아 발생하는 속성으로서 보통 계산된 값들이 이에 해당
  
  * 다른 속성에 영향을 받기 때문에 프로세스 설계 시 데이터 정합성을 유지하기 위해 유의해야 할 점이 많으며 가급적 파생속성을 적게 정의하는 것이 좋음
  
  * 예) 이자

* 엔터티 구성방식에 따른 분류
  
  1. PK 속성
  
  2. FK 속성
  
  3. 엔터티에 포함되어 있고 PK, FK에 포함되지 않은 일반 속성

## ▶️ 도메인(Domain)

> 각 속성은 가질 수 있는 값의 범위가 있는데 이를 그 속성의 도메인이라 함

* 각 속성은 도메인 이외의 값을 가지지 못함

* 엔터티 내에서 속성에 대한 데이터타입과 크기 그리고 제약사항을 지정하는 것

## ▶️ 속성의 명명

1. 해당업무에서 사용하는 이름을 부여

2. 서술식 속성명은 사용하지 않는다.

3. 약어사용은 가급적 제한

4. 전체 데이터모델에서 유일성을 확보하는 것이 좋다.

# 관계

## ▶️ 관계의 개념

> 엔터티의 인스턴스 사이의 논리적인 연관성으로서 존재의 형태로서나 행위로서 서로에게 연관성이 부여된 상태

* 관계의 페어링
  
  * 유의해야 할 점
    
    * 엔터티 안에 인스턴스가 개별적으로 관계를 가지는 것(페어링)이고 이것의 집합을 관계로 표현한다는 것. 따라서 개별 인스턴스가 각각 다른 종류의 관계를 가지고 있다면 두 엔터티 사이에 두 개 이상의 관계가 형성될 수 있음
  
  * 관계 페어링
    
    * 각각의 엔터티의 인스턴스들은 자신이 관련된 인스턴스들과 관계의 어커런스로 참여하는 형태

## ▶️ 관계의 분류

> 존재에 의한 관계와 행위에 의한 관계로 구분될 수 있는 것은 관계를 연결함에 있어 어떤 목적으로 연결되었느냐에 따라 분류하기 때문

* 존재의 형태에 의한 관계
  
  * 어느 팀에 소속된 어느 직원

* 행위에 의한 관계
  
  * 주문한다는 행위를 통해 주문번호를 생성

* UML 내 관계
  
  * 연관관계 (실선으로 표현)
    
    * 항상 이용하는 관계로 존재적 관계에 해당
    
    * 소스코드에서 멤버변수로 선언하여 사용
  
  * 의존관계 (점선으로 표현)
    
    * 상대방 클래스의 행위에 의해 관계가 형성될 때 구분하여 표현한다는 것
    
    * 행위를 나타내는 코드인 operation에서 파라미터 등으로 이용

## ▶️ 관계의 표기법

1. 관계명 - 관계의 이름
   
   * 엔터티가 관계에 참여하는 형태를 지칭
   
   * 관계가 시작되는 편을 관계시작점이라고 부르고 받는 편을 관계끝점이라고 부름
   
   * 참여자의 관점에 따라 능동적이거나 수동적으로 명명

2. 관계차수(Degree/Cardinality) - 1:1, 1:M, M:N
   
   > 두 개의 엔터티 간 관계에서 참여자의 수를 표현한 것
   
   - 가장 중요하게 고려해야 할 사항은 한 개의 관계가 존재하느냐 아니면 두 개 이상의 멤버십이 존재하는지를 파악하는 것이 중요
   
   - 한 개가 참여하는 경우 실선을 그대로 유지하고 다수의 경우 까마귀 발과 같은 모양으로 표현

3. 관계선택사양 - 필수관계, 선택관계
   
   > 데이터 모델의 관계에서는 필수참여관계나 선택적인 관계가 존재할 수 있다.
* 필수참여는 참여하는 모든 참여자가 반드시 관계를 가지는 타 엔터티의 참여자와 연결이 되어야 하는 관계이다.
  
  * 예) 주문서 - 반드시 주문목록을 가져야 하며 주문목록이 없는 주문서는 의미가 없으므로 필수참여 관계
  
  * 반대로 목록은 주문이 될 수도 있고 주문이 되지 않은 목록이 있을수도 있으므로 선택참여 관계

## ▶️ 관계의 정의 및 읽는 방법

* 관계 체크사항
  
  * 두 개의 엔터티 사이에 관심있는 연관규칙이 존재하는가?
  
  * 두 개의 엔터티 사이에 정보의 조합이 발생되는가?
  
  * 업무기술서, 장표에 관계연결에 대한 규칙이 서술되어 있는가?
  
  * 업무기술서, 장표에 관계연결을 가능하게 하는 동사가 있는가?

* 관계 읽기
  
  > 데이터 모델을 읽는 방법은 먼저 관계에 참여하는 기준 엔터티를 하나 또는 각으로 읽고 대상 엔터티의 개수(하나, 하나 이상)를 읽고 관계선택사양과 관계명을 읽도록 함
  
  * 기준 엔터티를 한 개 또는 각으로 읽는다.
  
  * 대상 엔터티의 관계 참여도 즉 개수(하나, 하나 이상)를 읽는다.
  
  * 관계선택사양과 관계명을 읽는다.

# 식별자

## ▶️ 식별자 개념

> 엔터티는 인스턴스들의 집합이라고 함.
> 
> 여러 개의 집합체를 담고 있는 하나의 통에서 각각을 구분할 수 있는 논리적인 이름를 식별자

* 하나의 엔터티에 구성되어 있는 여러 개의 속성 중에 엔터티를 대표할 수 있는 속성을 의미하며 하나의 엔터티는 반드시 하나의 유일한 식별자가 존재해야 한다.

* 엔터티내에서 인스턴스들을 구분할 수 있는 구분자이다.

* 식별자와 키의 차이
  
  * 식별자: 업무적으로 구분되는 정보로 생각할 수 있으므로 논리데이터 모델링 단계에서 사용
  
  * 키: 데이터베이스 테이블에 접근을 위한 매개체로서 물리데이터 모델링 단계에서 사용

## ▶️ 식별자의 특징

> 주식별자와 외부식별자인지에 따라 특성의 차이가 존재

* 주식별자인 경우
  
  * 주식별자에 의해 엔터티 내에 모든 인스턴스들이 유일하게 구분되어야 함 = 유일성
  
  * 주식별자를 구성하는 속성의 수는 유일성을 만족하는 최소의 수가 되어야 함 = 최소성
  
  * 지정된 주식별자의 값은 자주 변하지 않는 것이어야 함 = 불변성
  
  * 주식별자가 지정이 되면 반드시 값이 들어와야 함 = 존재성

* 외부식별자인 경우
  
  * 주식별자 특징과 일치하지 않으며 참조무결성 제약조건에 따른 특징을 지님

## ▶️ 식별자 분류

* 대표성 여부에 따라 주식별자와 보조식별자로 구분

* 엔터티 내에서 스스로 생성되었는지 여부에 따라 내부식별자와 외부식별자로 구분

* 속성의 수에 따라 단일식별자와 복합식별자로 구분

* 대체 여부에 따라 본질식별자와 인조식별자로 구분

## ▶️ 주식별자 도출 기준

> 데이터 모델링 작업에서 중요한 작업 중의 하나가 주식별자 도출 작업이다.

* 해당 업무에서 자주 이용되는 속성을 주식별자로 지정함
  
  * 예) 직원 엔터티 - 주민번호와 사원번호

* 명칭, 내역 등과 같이 이름으로 기술되는 것들은 가능하면 주식별자로 지정하지 않음
  
  * 예) 회사 내 각 부서 명칭
  
  * 인스턴스를 식별할 수 있는 다른 구분자가 없는 경우 일련번호와 코드를 많이 사용

* 복합으로 주식별자로 구성할 경우 너무 많은 속성이 포함되지 않도록 함
  
  * 주식별자로 선정된 속성들이 복잡하고 많아야 한다면 새로운 인조식별자를 생성하여 데이터 모델을 구성하는 것이 데이터 모델을 단순하게 한다.

## ▶️ 식별자 관계와 비식별자 관계에 따른 식별자

* 식별자관계와 비식별자 관계의 결정
  
  * 외부식별자는 자기 자신의 엔터티에서 필요한 속성이 아니라 다른 엔터티와의 관계를 통해 자식 쪽에 엔터티에 생성되는 속성을 외부식별자라 하며, 데이터베이스 생성 시 Foreign Key 역할
  
  * 자식엔터티에서 부모엔터티로부터 받은 외부식별자를 자신의 주식별자로 이용할 것인지 또는 부모와 연결이 되는 속성으로서만 이용할 것인지를 결정해야 함

* 식별자 관계
  
  > 자식엔터티의 주식별자로 부모의 주식별자가 상속이 되는 경우
  
  * 부모로부터 받은 식별자를 자식엔터티의 주식별자로 이용하는 경우는 NULL 값이 오면 안되므로 반드시 부모엔터티가 생성되어야 자기 자신의 엔터티가 생성되는 경우다.

* 비식별자 관계
  
  > 부모엔터티로부터 속성을 받았지만 자식엔터티의 주식별자로 사용하지 않고 일반적인 속성으로만 사용하는 경우
  
  * 비식별자 관계에 의한 외부속성을 생성하는 4가지 경우
    
    * 자식엔터티에서 받은 속성이 반드시 필수가 아니어도 무방하기 때문에 부모 없는 자식이 생성될 수 있는 경우
    
    * 엔터티별로 데이터의 생명주기를 다르게 관리할 경우
    
    * 어러 개의 엔터티가 하나의 엔터티로 통합되어 표현되었는데 각각의 엔터티가 별도의 관계를 가질 때이며 이에 해당
    
    * 자식엔터티에 주식별자로 사용하여도 되지만 자식엔터티에서 별도의 주식별자를 생성하는 것이 더 유리하다고 판단될 때 비식별자 관계에 의한 외부식별자로 표현

* 식별자 관계로만 설정할 경우의 문제점
  
  * 식별자 관계만으로 연결된 데이터 모델의 특징은 주식별자 속성이 지속적으로 증가할 수 밖에 없는 구조로서 개발자 복잡성과 오류가능성을 유발시킬 수 있는 요인이 될 수 있다는 사실을 기억

* 비식별자 관계로만 설정할 경우의 문제점
  
  * 각 엔터티 간의 관계를 비식별자 관계로 설정하면 이런 유형의 속성이 자식 엔터티로 상속이 되지 않아 자식엔터티에서 데이터를 처리할 때 쓸데없이 부모엔터티까지 찾아가야하는 경우 발생

* 식별자관계와 비식별자관계 모델링
  
  1. 비식별자관계 선택 프로세스(비식별자 관계 설정 고려사항)
     
     * 관계의 분석 > 관계의 강약분석 > 자식테이블 독립 PK 필요성 > SQL 복잡도 증가
  
  2. 식별자와 비식별자관계 비교
     
     * 강한관계인 식별자
       
       * 반드시 부모엔터티에 종속
       
       * 자식 주식별자구성에 부모 주식별자 포함
       
       * 상속받은 주식별자 속성을 타 엔터티에 이전 필요
     
     * 약한 관계인 비식별자관계
       
       * 자식 주식별자구성을 독립적으로 구성
       
       * 자식 주식별자구성에 부모 주식별자 부분 필요
       
       * 상속받은 주식별자속성을 타 엔터티에 차단 필요
       
       * 부모쪽의 관계참여가 선택관계
  
  3. 식별자와 비식별자를 적용한 데이터 모델
     
     * 식별자관계와 비식별자관계를 적절하게 선탬함으로써 데이터 모델의 균형감을 갖춤
