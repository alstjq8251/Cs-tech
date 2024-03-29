### Entity
`정의`
  - `엔터티(Entity)`란 업무에서 관리해야 하는 데이터 집합을 의미하며 저장되고 관리되어야 하는 데이터이다.
  - 엔터티는 개념, 사건 , 장소 등의 명사이다.
`특징`

|엔터티 특징| 설명|
|:--:|:--:|
|식별자| - 엔터티는 유일한 식별자가 있어야 한다.<br> - 회원 ID, 계좌번호|
|인스턴스 집합| - **2개 이상의 인스턴스**가 있어야한다.<br> - 고객정보는 2명 이상 있어야 한다.|
|속성| - 엔터니는 반드시 **속성**을 가지고 있다.<br> - 고객 엔터티에 회원ID, 패스워드, 이름, 주소|
|관계| - 엔터티는 **다른 엔터티와 최소한 한 개 이상 관계**가 있어야 한다.<br> - 고객은 관계를 가진다.|
|업무| - 엔터티는 업무에서 관리되어야 하는 **집합**이다. <br> - 고객, 계좌|

`릴레이션과 테이블 인스턴스`
  - 릴레이션과 테이블은 같은 의미로 생각해도 된다.
  - 릴레이션에 기본키 및 제약조건을 설정하면 테이블이 된다.
  - 단, Relationship은 관계를 의미한다.
  - 인스턴스는 릴레이션이 가질 수 있는 값을 의미한다.
  - 간단하게는 행의 수를 의미한다.

`종류`
 - 엔터티의 종류는 유형과 무형에 따른 종류, 엔터티가 발생하는 시점에 따른 종류로 나뉘어진다.
 - 엔터티를 유형과 무형으로 분류하는 기준은 물리적 형태의 존재 여부이다.

1. 유형과 무형에 따른 엔터티 종류

|종류| 설명|
|:--:|:--:|
|유형 엔터티| 업무에서 도출되며 **지속적으로 사용되는 엔터티**이다. <br> - 고객, 강사, 사원 등 |
|개념 엔터티| - **유형 엔터티는 형태가 있지만 개념 엔터티는 물리적 형태가 없다.** <br> - 개념적으로 사용되는 엔터티이다. <br> - 거래소 종목, 코스탁 종목 등|
|사건 엔터티| - **비지니스 프로세스를 실행하면서 생성되는 엔터티**이다. <br> - 주문 , 체결, 주문 취소 등

2. 발생시점에 따른 엔터티 종류

|종류| 설명|
|:--:|:--:|
|기본 엔터티(Base Entity)| - **키 엔터티**라고도 한다. <br> - 다른 엔터티로부터 영향을 받지 않고 **독립적으로 생성되는 엔터티**이다. <br> - 고객 , 상품, 부서|
|중심 엔터티(Main Entity)| - 기본 엔터티와 행위 엔터티간의 중간의 있는 것이다. <br> - 즉, 기본 엔터티로부터 생성되고 행위 엔터티를 생성하는 것이다. <br> - 계좌, 주문, 등
|행위 엔터티(Active Entity)| - 2개 이상의 엔터티로부터 생성된다. <br> - 주문 이력, 체결 이력 등

`엔터티 도출`
- 엔터티는 고객의 비지니스를 프로세스에서 관리되어야 하는 정보를 추출해야 한다.

#### Entity의 속성(Attribute)
`정의`
  - 속성이란 업무에서 필요한 정보인 엔터티가 가지는 항목이다.
  - 속성은 더이상 분리되지 않는 단위로, 업무에서 필요한 데이터를 저장할 수 있다.
  - 인스턴스의 구성 요소이고 의미적으로 더 이상 분해되지 않는다.

`특징`
  - 속성은 업무에서 관리되는 종류이다.
  - 속성은 하나의 값만 가진다.
  - 주식별자에게 함수적으로 종속된다. 
    - 기본키가 변경되면 속성도 변한다는 것을 의미한다.

`종류`
- 분해 여부에 따른 속성의 종류는 하단의 표와 같다.

|종류| 설명|
|:--:|:--:|
|단일 속성| - 하나의 의미로 구성된 것으로 회원 ID, 이름 등이다.|
|복합 속성| - 여러개의 의미가 있는 것으로 대표적으로 주소가 있다. <br> - 주소는 시 , 군 등으로 분리할 수 있다.|
|다중값 속성| - 속성에 여러 개의 값을 가질 수 있는 것으로 예를 들어 상품 리스트가 있다. <br> - 다중값 속성은 엔터티로 분해된다.|

- 특성에 따른 종류는 하단의 표와 같다.

|종류| 설명|
|:--:|:--:|
|기본 속성| - 비지니스 프로세스에서 도출되는 **본래의 속성**이다. <br> - 회원 ID, 이름, 계좌번호 등|
|설계 속성| - **데이터 모델링 과정에서 발생되는 속성**이다. <br> - **유일한 값을 부여**한다. <br> - 상품 코드, 지점 등|
|파생 속성| - **다른 속성에 의해서 만들어지는 속성**이다. <br> - 합계, 평균 등|

`도메인`
  - `도메인은 속성이 가질 수 있는 값의 범위` 이다.
  - 성별이라는 속성의 도메인은 남자와 여자이다.

#### Entity의 관계(ReliationShip)
`정의`
  - 관계는 엔터티 간의 관련성을 의미하며 존재 관계와 행위 관계로 분류된다.
  - 존재 관계는 두 엔터티가 존재 여부의 관계가 있는 것이고, 행위 관계는 두 개의 엔터티가 어떤 행위에 대한 관련성이 있는 것이다.

`종류`
  1. `존재 관계`
     - `존재 관계는 엔터티 간의 상태`를 의미 한다.
    
     - 고객이 은행에 회원가입을 하면 관리점이 할당되고 그 관리점에서 회원가입을 관리하는것을 의미하는 것이 그 예시이다.
      - 고객은 관리점에 소속된다 -> 존재 관계   
  2. `행위 관계`
     - `행위 관계는 엔터티 간의 어떤 행위가 있는 것`으로 계좌를 사용해서 주문을 발주하는 관계가 만들어지는 것등이 그 예시이다.
    
     - 주문을 발주하는 행위로 계좌에서 주문 이력이 생기는데 이때 주문은 가능의 여부이므로 이때 행위 관계라고 한다.

`관계 차수(Cardinality)`
  - `관계 차수는 두 개의 엔터티 간의 관계의 참여하는 수를 의미`한다.
    - 한명의 고객은 여러개의 계좌를 만들 수 있다. -> 1:N

<img width="100%" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/b381c0d9-2457-48d8-8752-dab4f7396bf0">

- 관계 차수의 종류
  1. 1:1 관계
    - 1:1관계는 완전 1:1관계와 선택적 1:1관계가 있다.
      - 고객에게 등급이 부여될 수도 안될 수도 있다. -> 선택적 1:1관계 하나의 엔터티와 관계되는 엔터티가 하나일 수도 없을 수도 있다
      - 하나의 엔터티와 관계되는 엔터티가 하나인 경우로 반드시 존재한다.
  2. 1:N 관계
    - 1:N관계는 엔터티에 행이 하나 있을 때 다른 엔터티의 값이 여러 개 있는 관계이다.
    - 고객은 여러개의 계좌를 가질 수 있는 관계를 말한다.
  3. M:N 관계
    - M:N관계는 두개의 엔터티가 서로 여러 개의 관계를 가지고 있는 것이다.
      - 한명의 학생은 여러개의 강의를 들을 수 있고 한개의 강의또한 여러 명의 학생이 들을 수 있으며 이때 M:N 관계가 된다.  
    - 관계형 데이터베이스에서 M:N관계의 조인(Join)은 카디널리티곱이 발생한다.
      - M:N 관계를 1:N or N:1로 해소해야만 한다.
  4. 필수적 관계와 선택적 관계
    - 필수적 관계는 반드시 하나는 존재해야 하는 관계이고 선택적 관계는 없을 수도 있는 관계이다.
    - 필수 관계는 |로 표현되고 선택적 관계는 0로 표현된다. 

`식별 관계와 비식별 관계`
1. 식별 관계(Identification Relationship)
  - 고객과 계좌 엔터티에서 고객은 독립적으로 존재할 수 있는 `강한 개체(String Entity)`이다.
  - 강한 개체는 어떤 다른 엔터티에게 의존하지 않고 독립적으로 존재한다.
  - 강한 개체는 다른 엔터티와 관계를 가질 때 `다른 엔터티에게 기본키를 공유`한다.
  - 강한 개체는 식별 관계로 표현한다.
    - `식별 관계란 고객 엔터티의 기본키인 회원ID를 계좌 엔터티의 기본키의 하나로 공유하는 것이다.`
    - 강한 개체의 기본키값이 변경되면 식별관계(기본키를 공유 받은)에 있는 엔터티의 값도 변경된다.
    - 여기서 계좌 엔터티는 `약한 개체(Weak Entity)`가 된다.
2. 비식별 관계(Non-Identification Relationship) 
  - `비식별 관계는 강한 개체의 기본키를 다른 엔티티의 기본키가 아닌 일반 칼럼으로 관계를 가지는 것`이다.
  - 예를 들어 관리점 엔터티의 기본키는 지점 코드이고, 고객 엔터티와 비식별 관계를 가지고 있다. 
  - 지점 코드는 고객 엔터티의 기본 키가 아닌 일반 칼럼으로 참조되는 관계를 말한다.
  - 비식별 관계는 점선으로 표현한다.

`강한 개체와 약한 개체란`?
1. 강한 개체는 누구에게도 지배되지 않는 독립적인 개체이다.
2. 약한 개체는 개체의 존재가 다른 개체의 존재에 달려있는 개체이다.

#### Entity Identifier
`정의`
  - **식별자라는 것은 엔터티를 대표할 수 있는 유일성을 만족하는 속성**이다.
    - 일반적으로 회원ID, 계좌번호, 주민등록번호, 여권번호 등이 있다.

1. 주 식별자(기본 키 , Primary Key)
   1.`최소성` : 주식별자는 최소성을 만족하는 키이다.
   2.`대표성` : 주식별자는 엔터티를 대표할 수 있어야 한다.
   3.`유일성` : 주식별자는 인스턴스를 유일하게 식별한다.
   4.`불변성` : 주식별자는 자주 변경되지 않아야 한다.

|데이터베이스 키| 설명|
|:--:|:--:|
|기본 키(Primary Key)| 후보키중에서 엔터티를 대표할 수 있는 키이다|
|후보 키(Candidate Key)| 후보키는 유일성과 최소성을 만족하는 키이다.|
|슈퍼 키(Super Key)| 슈퍼키는 유일성은 만족하지만 최소성을 만족하지 못하는 키이다.|
|대체 키(Alternate Key)| 대체 키는 여러개의 후보키중에서 기본키를 선정하고 남은 키이다.|
|외래 키(Foreign key)| - 하나 혹은 다수의 다른 테이블의 기본 키 필드를 가리키는 것으로 `참조 무결성(Referential Integrity)`을 확인하기 위해서 사용되는 키이다. <br> - 즉 허용된 데이터의 값만 데이터베이스에 저장하기 위해서 사용된다.| 

2. 식별자의 종류
- 식별자는 대표성, 생성 여부, 속성의 수, 대체 여부로 분류된다.
   1. 식별자의 대표성
      - 주 식별자는 엔터티를 대표할 수 있는 식별자이다.
      - 주식별자는 다른 엔터티와 참조 관계로 연결될 수 있다.
      - 보조 식별자는 유일성과 최소성은 만족하지만 불변성은 만족하지 못하는 식별자이다.
   2. 식별자의 생성 여부
      - 생성 여부에 따른 식별자는 내부 식별자와 외부 식별자로 나뉜다. 
      - 내부 식별자는 엔터티 내부에서 스스로 생성되는 식별자를 의미한다. Ex) 부서 코드 , 종목 코드
      - 외부 식별자는 다른 엔터티와의 관계로 만들어지는 식별자를 의미한다. Ex) 계좌 엔터티에 회원 Id
   3. 식별자의 속성의 수
      - 속성의 수에 따른 식별자의 종류에는 단일 식별자와 복합 식별자가 있다.
      - 단일 식별자는 하나의 속성으로 구성된다.
      - 복합 식별자는 두 개 이상의 속성으로 구성된다.
   4. 식별자의 대체 여부
      - 대체 여부에 따른 식별자의 종류에는 본질 식별자와 인조 식별자로 나뉜다.
      - 본질 식별자는 비지니스 프로세스에서 만들어지는 식별자를 말한다.
      - 인조 식별자는 인위적으로 만들어지는 식별자를 말한다.
      
      
### Reference
<https://www.inflearn.com/course/lecture?courseSlug=sqld-%EC%9E%90%EA%B2%A9%EC%A6%9D-%EC%B4%88%EA%B8%89-1&unitId=103278&tab=curriculum>


