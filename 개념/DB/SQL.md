### SQL(Structure Query Language)
`정의`
  - **SQL은 관계형 데이터베이스에 대해서 데이터의 구조를 정의, 데이터 조작, 데이터 제어 등을 할 수 있는 절차형 언이이다.
  - SQL은 ANSI/ISO 표준을 준수하기 때문에 데이터베이스 관리 시스템이 변경되어도 그대로 사용할 수 있다.

`종류`

|종류 | 설명|
|:--:|:--:|
|DDL(Data Definition Langugage) | - 관계형 데이터베이스의 구조를 정의하는 언어이다. <br> - CREATE, ALTER, DROP, RENAME등이 있다.|
|DML(Data Manipulation Language) | - 테이블에서 데이터를 입력, 수정, 삭제, 조회한다. <br> - INSERT, UPDATE, DELECT, SELECT문이 있다.|
|DCL(Data Control Language) | - 데이터베이스 사용자에게 권한을 부여하거나 회수한다. <br> - GRANT, REVOKE, TRUNCATE등이 있다.|
|TCL(Transaction Control Language) | - 트랜잭션을 제어하는 명령어이다. <br> - COMMIT, ROLBACK, SAVEPOINT등이 있다.|

- **DDL문은 데이터베이스 테이블을 생성하거나 변경, 삭제하는 것으로 데이터를 저장할 구조를 정의하는 언어**이다.
- **DML문은 데이터 구조가 DDL로 정의되면 해당 데이터 구조에 데이터를 입력, 수정, 삭제, 조회할 수 있는 언어**이다.
- **DCL문은 DDL로 정의된 구조에 어떤 사용자가 접근할 수 있는지 권한을 부여하는 언어**이다.

> DDL로 데이터 구조를 만들고 DML로 데이터를 입력, 수정, 변경, 조회하고 DCL로 접근권한(Access Control)을 관리한다.

`실행 순서`
  - 개발자가 작성한 SQL(DDL,DML,DCL)은 3단계를 걸쳐서 수행된다.
  - SQL문의 문법을 검사하고 구문분석을 한뒤 SQL실행을 한다.
  - SQL이 실행되면 데이터를 추출하게 된다.

|SQL 실행 순서|설명|
|:--:|:--:|
|파싱(Parsing)| - SQL문의 문법을 확인하고 구문분석을 한다. <br> - 구문분석한 SQL문은 Library Cache에 저장한다.|
|실행(Execution)| - 옵티마이저(Optimizer)가 수립한 실행 계획에 따라 SQL을 실행한다.|
|인출(Fetch)| - 데이터를 읽어서 전송한다.|

#### DDL
1. `테이블 생성`
  - **테이블 생성은 CREATE TABLE문**을 사용하고 **테이블 변경은 ALTER TABLE문**을 사용하고 **테이블을 삭제할때는 DROP TABLE문**을 사용한다.

|SQL문|설명|
|:--:|:--:|
|CREATE TABLE | - 새로운 테이블을 생성한다. <br> - 테이블을 생성할때 기본 키, 외래 키, 제약조건 등을 설정할 수 있다.|
|ALTER TABLE| - 생성된 테이블을 변경한다. <br> - 칼럼을 추가하거나 변경, 삭제할 수 있다. <br> - 기본키를 설정하거나 외래키를 설정할 수 있다.|
|DROP TABLE| - 해당 테이블을 삭제한다. <br> - 테이블의 데이터 구조 뿐만 아니라 저장된 데이터도 모두 삭제한다.|

2. `기본적인 테이블 생성`

<img width="100%" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/b36f10d8-9ac0-4a61-b9d2-2d20ab7aeb5b">

  - 테이블의 구조를 확인하고 싶다면 `DESC(DESCRIBE)`문을 사용한다.
    - create table로 생성된 테이블의 구조를 확인할 때 사용한다.
3. `제약 조건 사용`
  - 기본키 , 외래키, 기본값, not null등은 테이블을 생성할 때 지정할 수 있다.
    - constraint {pk를 부여할 이름} primary key(필드 이름)
      - Primary Key라고 생성할 시 상단의 것과 똑같이 기본키 제약조건이 모두 설정된다.(Not Null, Unique)
    - constraint {외래키 이름} foreign key(참조할 pk 필드 이름) reference {테이블} (필드);
4. `Cascade`
  - 테이블을 생성할 때 Cascade 옵션을 사용할 수 있다.
  - Cascade 옵션은 참조 관계(기본키,외래키 관계)가 있을 경우 참조되는 데이터를 자동으로 반영할 수 있는 것
    - on delete cascade 
      - 자신이 참조하고 있는 테이블이 삭제되면 자신도 삭제하는 옵션으로 참조 무결성을 준수하게 해준다.
      - 참조 무결성이란 마스터 테이블에는 번호가 없는데 슬레이브 테이블에 마스터 테이블의 pk가 있는 경우 참조 무결성 원칙 위배라고 할 수 있다.

5. `테이블 변경`
  - 테이블명 변경은 Alter Table ~ Rename To 구문으로 변경 가능 하다.
  - 칼럼 추가는 Alter Table ADD(제약 조건); 으로 가능 하다.
  - 칼럼 변경은 Alter Table ~ Modify문으로 변경 가능 하다.
    - 칼럼 변경을 통해 데이터 타입을 변경하거나 데이터의 길이를 변경할 수 있다. 

#### SQL 각 명령어들
`DESC`
  - DESC(DESCRIBE) **테이블 이름**  명령어는 간단하게 테이블 구조를 확인하는 명령어이다.
`Default`
  - Default 제약조건 insert시 비우고 삽입하게 되면 Default 후로 오는 값이 기본값으로 들어가게 된다.


