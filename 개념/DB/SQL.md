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
