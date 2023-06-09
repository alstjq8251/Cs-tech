## SQL(Structure Query Language)
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

### DDL
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
    - ALTER TABLE EMP ADD(AGE NUMBER(2) DEFAULT 1); -> COLUMN은 절대 넣어선 안됨 
  - 칼럼 변경은 Alter Table ~ Modify문으로 변경 가능 하다.
    - ALTER TABLE EMP MODIFY(ENAME VARCHAR2(50) NOT NULL); -> 이미 추가된 제약사항으로 추가하면 변경되지 않는다.
    - 칼럼 변경을 통해 데이터 타입을 변경하거나 데이터의 길이를 변경할 수 있다. 
  - 칼럼 삭제는 Alter Table ~ DROP COLUMN {칼럼명}문으로 삭제 가능하다.
    - ALTER TABLE EMP DELETE COLUMN AGE 
  - 칼럼명 변경은 ALTER TABLE RENAME COLUMN ~ TO {변경 칼럼명} 문으로 변경 가능하다.
    - ALTER TABLE RENAME COLUMN ENAME TO NEW_ENAME 

6. `테이블 삭제`
  - 테이블 삭제는 DROP TABLE 문을 사용해서 삭제 가능하다.
    - 테이블 구조와 데이터를 모두 삭제하기 때문에 주의가 필요하다
    - DROP TABLE에서 **CASCADE CONSTRAINT**옵션을 사용해서 해당 테이블의 데이터를 외래키로 참조한

      슬레이브 테이블과 관련된 제약사항도 삭제할 때 사용된다.
        - DROP TABLE EMP CASCADE CONSTRAINT
#### 뷰
`정의`
  -  **뷰란 테이블로부터 유도된 가상의 테이블이다.**
  -  **실제 데이터를 가지고 있지 않고 테이블을 참조해서 원하는 칼럼만을 조회할 수 있게 한다.**
  -  뷰는 데이터 딕셔너리(Data Dictionary)에 SQL문 형태로 저장하되 실행시에 참조된다.
`특징`
  - 참조한 테이블이 변경되면 뷰도 변경된다.
  - 뷰의 검색은 참조한 테이블과 동일하게 할 수 있지만, **뷰에 대한 입력, 수정, 삭제에는 제약이 있다.**
  - **특정 칼럼만 조회시켜서 보안성을 향상**시킨다.
  - **한번 생성된 뷰는 변경할 수 없고 변경을 원하면 삭제 후 재생성해야 한다.**
  - **ALTER문을 사용해서 뷰를 변경할 수 없다.**
    - **뷰를 생성할 때 CREATE VIEW문을 사용하며 이 때 참조할 테이블은 SELECT문으로 지정**한다.
      - CREATE VIEW {view 이름} AS SELECT * FROM 참조 테이블
  - 뷰의 조회는 일반 테이블처럼 SELECT * from 뷰 네임;
  - **뷰의 삭제는 DROP VIEW를 사용하고** 뷰를 삭제했다고 해서 참조했던 테이블이 삭제되지 않는다.
    - DROP VIEW 뷰 네임;
`장점`
  - 특정 칼럼만 조회하기 때문에 보안기능이 있다.
  - 데이터 관리가 가능하다.
  - select문이 간단해진다.
  - **하나의 테이블에 여러 개의 뷰를 생성할 수 있다.**
`단점`
  - 뷰는 독자적인 인덱스를 만들 수 없다.
  - 삽입,수정,삭제 연산이 제약된다.
  - 데이터 구조를 변경할 수는 없다.

### DML
1. INSERT문
  - INSERT문은 테이블에 데이터를 입력하는 DML문이다.
  - 데이터를 입력할때 문자열을 입력하는 경우에는 쌍따음표('')를 사용해야 한다.
  - 만약 특정 테이블의 모든 칼럼에 대한 데이터를 삽입하는 경우에는 칼럼명을 생략할 수 있다.
    - 이땐 모든 데이터를 삽입해야 한다. 
    - **INSERT문을 입력했다고 해서 무조껀 데이터가 저장되는 것은 아니고 최종적으로 데이터를 저장하려면 TCL문인 COMMIT을 입력해야** 한다. 
    - AUTO COMMIT을 입력한 경우엔 COMMIT을 실행하지 않아도 바로 저장이 된다.
2. SELECT문
  - SELECT문을 사용하여 데이터를 조회해서 해당 테이블에 바로 삽입할 수 있다.
  - 단, 입력되는 테이블은 사전에 생성되어 있어야 한다.
    - INSERT INTO DEPT_TABLE SELECT * FROM DESK;
3. Nologging문
  - 데이터베이스에 데이터를 입력하면 로그파일(Log file)에 그 정보를 저장한다.
  - **Nologging 옵션은 로그파일의 기록을 최소화 시켜서 입력 시 성능을 향상시키는 방법 이다.**
    - ALTER TABLE EMP NOLOGGING;
  - nologging 옵션은 BUFFER CACHE라는 메모리 영역을 생략하고 바로 기록을 한다.
4. UPDATE문
  - 입력된 데이터의 값을 수정하려면 UPDATE문을 사용한다.
  - UPDATE문을 사용하여 원하는 조건으로 데이터를 검색해서 해당 데이터를 수정할 수 있다.
  - 만약, **UPDATE문에 조건문을 입력하지 않으면 모든 데이터가 수정**되므로 유의해야 한다.
  - UPDATE문의 주의 사항은 테이터를 수정할 때 조건절에서 검색되는 행 수만큼 수정 된다는 것이다.
5. DELETE문
  - DELETE문은 원하는 조건을 검색해서 해당되는 행을 삭제한다.
  - **DELETE문에 조건절을 입력하지 않으면 모든 데이터가 삭제**된다.
  - DELETE문으로 데이터를 삭제한다고 해서 테이블의 용량이 초기화되지는 않는다.
6. ORDER BY문
  - SELECT를 사용할때 Order By를 사용할 수 있다.
    - **단 Order By구문은 마지막에 실행**된다.
  - **Order By는 데이터를 오름차순(Ascending)이나 내림차순(Descending)으로 출력**한다.
    - 특별한 지정이 없다면 오름차순으로 정렬하게 된다.
  - Order By는 모든 쿼리 실행 후 데이터를 출력 하기 전에 실행되며 정렬하기 때문에 db 메모리를 많이 점유하게 된다. -> 성능 저하로 이어질 수 있음
    - SELECT * from EMP ORDER BY EMPNO , SAL DESC -> Order By 인자의 개수는 정해져 있지 않으며 순차적으로 정렬을 진행한다.
      - EMPNO가 같을때 SAL 내림차순 정렬방식으로 데이터 출력
7. DISTINCT와 ALIAS
  - **DISTINCT문은 칼럼명 앞에 지정하여 중복된 데이터를 한번만 조회**하게 한다.
    - SELECT DISTINCT EMPNO FROM EMP; 
  - **ALIAS(별칭)은 테이블명이나 칼럼명이 너무 길어서 간략하게 할 때 사용한다.**  
    - SELECT ENAME AS "이름" FROM EMP e WHERE e.EMPNO = 1000;
8. LIKE문
  - **LIKE문은 와일드 카드를 사용해서 데이터를 조회**할 수 있다. 
  - <details>
    <summary>와일드 카드</summary>
    <div>
      
      |와일드 카드 | 설명|
      | :--: | :--:|
      |  % |  - 어떤 문자를 포함한 모든 것을 조회한다. <br> - %조로 조회하면 조로로 시작하는 모든 문자를 조회한다.|
      | _(underscore) | - 한개의 단일 문자를 조회한다. | 

      - SELECT * FROM EMP WHERE EMPNAME LIKE %1 => 1로 끝나는 모든 문자
      - SELECT * FROM EMP WHERE EMPNAME LIKE _길 => 길로 끝나는 2글자의 모든 문자
      
    </div>
    </details>
9. BETWEEN문
  - **BETEWEN문은 지정된 범위에 있는 값을 조회**한다.
10. IN문
  - **IN문은 "OR"의 의미를 가지고 있어서 하나의 조건만 만족해도 조회**한다.
11. NULL값 조회
  - <details>
     <summary>특징</summary>
     <div>
       
       - NULL은 모르는 값을 의미한다.
       - NULL은 값의 부재를 의미한다.
       - NULL과 숫자 혹은 날짜를 더하면 NULL이 된다.
       - NULL과 어떤 값을 비교할 때 '알 수 없음'이 반환된다.
       
     </div>
     </details>
     
  - `조회`
     - **NULL을 조회할 땐 IS NULL을 사용하고 NULL이 아닌 값을 조회할 땐 IS NOT NULL을 사용한다.**
  - <details>
     <summary>NULL관련 </summary>
     <div>
       
      |NULL 함수| 설명|
      | :--: |:--:|
      | NVL 함수 | - NULL 이면 다른 값으로 바꾸는 함수이다.<br> - 'NVL(MGR,0)' -> MGR칼럼이 NULL이면 0을 반환한다.|
      | NVL2 함수 | - NVL 함수와 DECODE 함수를 하나로 만든 것이다. <br> - 'NVL2(MGR,1,0)' -> MGR칼럼이 NULL이 아니면 1을 NULL이면 0을 반환한다. |
      | NULLIF 함수 | - 두 개의 값이 같으면 NULL을, 같지 않으면 첫번째 값을 반환한다. <br> - 'NULLIF(ex1,ex2)' -> ex1과 ex2가 같으면 NULL 다르면 ex1을 반환한다. |
      | COALESCE | - NULL이 아닌 최초의 인자값을 반환한다. <br> - 'COALESCE(ex1,ex2,ex3)' ex1이 NULL이 아니면 ex1을 NULL이면 그 뒷 값을 비교해 반환한다. |
       
     </div>
     </details>

`UPDATE DIFFERENCE MASTER`

|DELETE문 | TRUNCATE TABLE |
|:--:|:--:
| - 테이블의 모든 데이터를 삭제한다. <br> - 데이터가 삭제되도 테이블의 용량은 감소하지 않는다.| - 테이블의 모든 데이터를 삭제한다. <br> - 데이터가 삭제되어도 테이블의 용량은 초기화 된다.|
| - DML구문이다 <br> - ROLLBACK 가능하다.| - DDL구문이다.<br> - ROLLBACK 불가능하다.

#### where문 사용 연산자.
1. <details>
    <summary>비교 연산자.</summary>
    <div>
      
    |비교 연산자| 설명|
    | :--: |:--:|
    | = | 같은 것을 조회한다.|
    | < | 작은 것을 조회한다. |
    | <= | 작거나 같은 것을 조회한다. |
    | > | 큰 것을 조회한다. |
    | >= | 크거나 같은 것을 조회한다. |

    </div>
    </details>

2. <details>
    <summary>논리 연산자.</summary>
    <div>

    | 논리 연산자 | 설명|
    | :--: |:--:|
    | AND | 조건을 만족해야 참(true)이 된다.|
    | OR | 조건 중 하나라도 만족하면 참(true)이 된다.|
    | NOT | 참이면 거짓(false)으로 바꾸고 거짓이면 참(true)로 바꾼다.|

    </div>
    </details>

3. <details>
    <summary>SQL연산자</summary>
    <div>

    | SQL 연산자 | 설명|
    | :--: |:--:|
    | LIKE % 비교문자열 % | 비교 문자열을 조회한다. '%'는 모든 값을 의미한다.|
    | BETWEEN A AND B | A와 B사이의 값을 조회한다.|
    | IN(list) | OR를 의미하며 list 값 중에 하나라도 일치하면 조회된다.|
    | IS NULL | NULL 값을 조회한다.|

    </div>
    </details>

4. <details>
    <summary>부정 연산자.</summary>
    <div>

    | 부정 연산자 | 설명|
    | :--: |:--:|
    | NOT BETWEEN A AND B | A와 B사이에 해당하지 않는 값을 조회한다.|
    | NOT IN(list) | list와 불일치한 값을 조회한다.|
    | IS NOT NULL | NULL이 아닌 값을 조회한다.|

    </div>
    </details>
    
#### GROUP 연산
1.GROUP BY 문
  - **GROUP BY는 테이블에서 소규모 행을 그룹화하여 합계,평균,최댓값,최솟값 등을 계산** 할 수 있다.
  - **HAVING 구문을 사용하여 조건문을 사용**한다.
  - **ORDER BY를 사용해서 정렬**할 수 있다.
2. HAVING 문
  - **GROUP BY에 조건절을 사용하려면 HAVING을 사용**해야 한다.
  - 만약 where에 조건절을 사용하게 되면 조건을 충족하지 못하는 대상들은 GROUP BY 대상에서 제외된다.
    - **where절에는 집계(SUM,AVG,MAX,MIN 등) 함수를 사용하면 안된다.** 
3. <details>
   <summary>집계 함수</summary>
   <div>

     | 집계 함수 | 설명|
     | :--: |:--:|
     | COUNT() | 총 수를 조회한다.|
     | SUM() | 합계를 계산한다.|
     | AVG() | 평균을 계산한다.|
     | MAX(), MIN() | 최댓값과 최솟값을 계산한다.|
     | STDDEV() | 표준편차를 계산한다.|
     | VARIAN() | 분산을 계산한다.|

   </div>
   </details>
4. COUNT
  - COUNT()함수는 행 수를 계산하는 함수이며, count(*)은 null을 포함한 모든 행 수를 계산한다.
    - 하지만 count(칼럼명)은 NULL을 제외한 행 수를 계산한다.

#### SELECT문 실행 순서
  - SQL의 실행 순서는 결과로 조회된 데이터를 이해하는데 아주 중요한 요소이다.
  - **SELECT문의 실행 순서는 FROM,WHERE,GROUP by, HAVING, SELECT, ORDER BY 순으로** 진행된다.

#### 명시적(Explict) 형변환과 암시적(Implict) 형변환
`정의`
  - **형 변환이라는 것은 두 개의 데이터 타입(형)이 일치하도록 변환**하는 것이다.
    - 숫자와 문자열의 비교, 문자열과 날짜열의 비교 같은 데이터 타입이 불일치할때 발생 
  - **형 변환은 명시적(Explict) 형 변환과 암시적(Implict) 형변환이 있다.**
  - 명시적 형 변환은 형 변환 함수를 사용하여 데이터 타입을 일치시키는 것으로 개발자가 SQL을 사용할때 형변환 함수를 사용해야 한다.      
    - <details>
       <summary>형 변환 함수</summary>
       <div>

       | 형 변환 함수 | 설명|
               | :--: |:--:|
       | TO_NUMBER(문자열) | 문자열을 숫자열로 변경한다.|
       | TO_CHAR(숫자, 혹은 날짜, [FORMAT]) | 숫자 혹은 날짜를 지정된 FORMAT으로 변경한다. <br> AM, PM을 끝에 붙일 시 오전, 오후 출력|
       | TO_DATE(문자열, FORMAT) | 문자열을 지정된 FORMAT의 날짜형으로 변환한다.|

       </div>
       </details>
  - 암시적 형변환은 개발자가 변환을 하지 않은 경우 데이터베이스 관리 시스템이 자동으로 형 변환 하는 것을 의미한다.

#### 내장형 함수
`정의`
  - 모든 데이터베이스는 SQL에서 사용할 수 있는 내장 함수를 가지고 있다.
  - 내장형 함수는 DBMS별로 차이가 있지만 거의 비슷한 방법으로 사용이 가능하다.
  - **내장형 함수로는 형 변환 함수, 문자열 및 숫자형 함수, 날짜형 함수**가 있다.

```
DUAL 테이블
DUAL 테이블은 oracle 데이터베이스에 의해서 자동으로 생성되는 테이블이다. -> mysql에는 없다.
oracle db 사용자가 임시로 사용할 수 있는 테이블로 내장형 함수를 실행할 때도 사용이 가능하다.
oracle db의 모든 사용자가 이용 가능하다.
```

<details>
<summary>문자열 함수</summary>
<div>

  | 문자열 함수 | 설명|
  | :--: |:--:|
  | ASCII(문자) | 문자 혹은 숫자를 ASCII코드값으로 변환한다.|
  | CHR(ASCII 코드 값) | ASCII 코드 값을 문자로 변환한다.|
  | SUBSTR(문자열,m,n) | 문자열을 m번째 위치부터 n개를 자른다.|
  | CONCAT(문자열1,문자열2) | 문자열 1번과 문자열 2번을 결합한다.<br> Oracle은 || mysql은 +를 사용할 수 있다.|
  | LOWER(문자열) | 영문자를 소문자로 변환한다.|
  | UPPER(문자열) | 영문자를 대문자로 변환한다.|
  | LENGTH 혹은 LEN(문자열) | 공백을 포함해서 문자열의 길이를 알려준다.|
  | LENGTHB(문자열) | 해당 문자열의 바이트 크기를 알려준다. (한글 3, 영어 1)|
  | LTRIM(문자열, 지정문자) | 왼쪽에서 지정된 문자를 제거한다. <br> 지정된 문자를 생략하면 공백을 삭제한다.|
  | RTRIM(문자열, 지정문자) | 오른쪽에서 지정된 문자를 제거한다. <br> 지정된 문자를 생략하면 공백을 삭제한다. |
  | TRIM(문자열, 지정문자) | 왼쪽 및 오른쪽에서 지정된 문자를 제거한다. <br> 지정된 문자를 생략하면 공백을 삭제한다. |
  
  - ORACLE db가 기준이기에 다른 dbms에선 문법의 차이가 있을 수 있다.

</div>
</details>

<details>
<summary>날짜형 함수</summary>
<div>
  
  - 날짜형 함수 중에서 오늘 날짜를 구하기 위해서는 **SYSDATE**를 사용한다.
  - 만약 해당 연도만 알고 싶다면 **EXTRACT**를 사용한다.
  - **TOCHAR함수는 형 변환 함수 중에서 가장 많이 사용하는 것으로 숫자나 날짜를 원하는 형태의 문자열로 변환**한다.


  | 날짜형 함수 | 설명|
  | :--: |:--:|
  | SYSDATE | 오늘의 날짜를 날짜 타입으로 알려준다. <br> mysql에선 SYSDATE() 함수형태로 제공|
  | EXTRACT('YEAR'|'MONTH'|'DAY') from dual | 날짜에서 년,월,일을 조회한다.|
  
  - ORACLE db가 기준이기에 다른 dbms에선 문법의 차이가 있을 수 있다.

</div>
</details>


<details>
<summary>숫자형 함수</summary>
<div>

  | 날짜형 함수 | 설명|
  | :--: |:--:|
  | ABS(숫자) | 절댓값을 반환한다.|
  | SIGN(숫자) | 양수, 음수, 0을 구분한다. <br> 양수일땐 +1 , 음수일땐 -1 , 0일땐 0|
  | MOD(숫자1, 숫자2) | 숫자1을 숫자2로 나누어 나머지를 계산한다. <br> %를 사용해도 된다.|
  | CEIL/ CEILING(숫자) | 숫자보다 **크거나 같은 정수**를 반환한다.|
  | FLOOR(숫자) | 숫자보다 **작거나 같은 정수**를 반환한다.|
  | ROUND(숫자,m) | **소수점 m 번째 자리에서 반올림**한다. <br> m의 기본값(DEFAULT 0)은 0이다.|
  | TRUNC(숫자,m) | **소수점 m자리에서 절삭한다.** <br> m의 기본값(DEFAULT 0)은 0이다.|

  - ORACLE db가 기준이기에 다른 dbms에선 문법의 차이가 있을 수 있다.

</div>
</details>

### DCL
1. GRANT
  - **GRANT문은 사용자에게 권한을 부여**한다.
  - 데이터베이스 사용을 위해서는 권한이 필요하며 연결,입력,수정,삭제,조회를 할 수 있다.
  - `GRANT privilege ON object TO USER;`
    - privilege는 권한을 object는 테이블을 의미한다. user는 db의 사용자를 지정한다.
    - <details>
      <summary>Privilege권한의 종류</summary>
      <div>

        | 권한 | 설명|
        | :--: | :--:|
        | SELECT | 지정된 테이블에 대해서 SELECT권한을 부여한다.|
        | INSERT | 지정된 테이블에 대해서 INSERT권한을 부여한다.|
        | UPDATE | 지정된 테이블에 대해서 UPDATE권한을 부여한다.|
        | DELETE | 지정된 테이블에 대해서 DELETE권한을 부여한다.|
        | PREFERENCE | 지정된 테이블을 참조하는 제약조건을 생성하는 권한을 부여한다.|
        | ALTER | 지정된 테이블에 대해서 수정할 수 있는 권한을 부여한다.|
        | INDEX | 지정된 테이블에 대해서 인덱스를 생성할 수 있는 권한을 부여한다.|
        | ALL | 테이블에 대한 모든 권한을 부여한다.|

      </div>
      </details>
  - <details>
    <summary>WITH GRANT OPTION</summary>
    <div>

      | GRANT 옵션 | 설명|
      | :--: | :--:|
      | WITH GRANT OPTION | 특정 사용자에게 권한을 부여할 수 있는 권한을 부여한다. <br> 권한을 A가 B에 부여하고 B가 C등에 부여한 후 A가 회수하면 전체 회수된다.(Revoke) |
      | WITH ADMIN OPTION | 테이블에 대한 모든 권한을 부여한다. <br> 권한을 A가 B에 부여한 후 B가 C에 부여한 뒤 A가 회수하면 B만 회수됨 |
  
    </div>
    </details>

`Revoke`
  - **Revoke문은 데이터베이스 사용자에게 부여된 권한을 회수한다.**
  - REVOKE privilege ON object FROM USER;

### TCL
`COMMIT`
  - **COMMIT은 INSERT,UPDATE,DELETE문으로 변경한 데이터를 물리적 데이터베이스에 반영한다.**
  - commit이 완료되면 데이터베이스 변경으로 인한 LOCK이 해제(UNLOCK)된다.
  - commit을 실행하면 하나의 트랜잭션 과정을 종료한다.
`ROLLBACK`
  - **ROLLBACK을 실행하면 데이터에 대한 변경 사용을 모두 취소하고 트랜잭션을 종료한다.**
  - INSERT,UPDATE,DELETE문의 작업을 모두 취소한다. 단, 이전에 COMMIT한 곳까지만 복구한다.
  - ROLLBACK을 실행하면 LOCK이 해제되고 다른 사용자도 데이터베이스행을 조작할 수 있다.
`SAVEPOINT`
  - **SAVEPOINT는 트랜잭션을 작게 분할하여 관리하는 것으로 SAVEPOINT를 사용하면 지정된 위치 이후의 트랜잭션만 ROLLBACK할 수 있다.**
  - SAVEPOINT의 지정은 SAVEPOINT <SAVEPOINT명>을 실행한다.
  - **지정된 SAVEPOINT까지만 데이터 변경을 취소하고 싶은 경우는 "ROLLBACK TO <SAVEPOINT명>"**을 실행한다.
  - ROLLBACK을 실행하면 SAVEPOINT와 관계 없이 모든 변경 사항을 저장하지 않는다.

### SQL 각 명령어들
`DESC`
  - DESC(DESCRIBE) **테이블 이름**  명령어는 간단하게 테이블 구조를 확인하는 명령어이다.
`Default`
  - Default 제약조건 insert시 비우고 삽입하게 되면 Default 후로 오는 값이 기본값으로 들어가게 된다.
`DECODE`
  - **DECODE문으로 IF문을 구현할 수 있다. 즉 특정 조건이 참이면 A, 거짓이면 B로 응답한다.**
    - SELECT DECODE(ex,1000,true,false) 1000과 같으면 true 아니면 false
`CASE`
  - **CASE문은 IF~THEN , ~ELSE~AND의 프로그래밍처럼 조건문을 사용할 수 있다.
  - 조건을 when구문에 사용하고 then구문은 해당 구문이 참이면 실행되고 거짓이면 ELSE구문이 실행된다.
  - CASE(ex) WHEN EMPNO = 1000 THEN result1 WHEN EMPNO = 1001 THEN result2 ELSE 12 END
`ROWNUM`
  - **ROWNUM은 ORACLE 데이터베이스의 SELECT문 결과에 대해서 논리적인 일련번호를 부여**한다.
  - **ROWNUM은 조회되는 행 수를 제한할때 많이 사용**한다.
    - SELECT * FROM EMP WHERE ROWNUM < 2 -> 조회 행 1개로 제한
  - **ROWNUM은 화면에 데이터를 출력할 때 부여되는 논리적 순번**이다.
    - 만약 rownum을 이용해서 페이지 단위 출력을 하기 위해서는 인라인 뷰(Inline View)를 사용해야 한다.
  - **인라인 뷰(Inline View)는 SELECT문에서 FROM절에 사용되는 서브 쿼리(SUB QUERY)를 의미**한다.
  - **ROWNUM의 값을 1,2,3,4,5 등 순차적으로 증가하는 ROWNUM 데이터를 얻고 싶을 때 인라인뷰를 사용한다.** 
    - SELECT * FROM (SELECT ROWNUM list, ENAME FROM EMP ) WHERE LIST <=5; 
  - oracle db에선 rownum sql Server에선 top mysql에선 limit으로 동일한 기능을 제공한다.
`ROWID`
  - **ROWID는 ORACLE 데이터베이스 내에서 데이터를 구분할 수 있는 유일한 값**이다.
  - SELECT ROWID,EMPNO FROM EMP; 와 같은 select문으로 확인할 수 있다.
  - ROWID를 통해서 데이터가 어떤 데이터 파일, 어느 블록에 저장되어 있는지 확인할 수 있다.
  - <details>
    <summary>ROWID 구조</summary>
    <div>

      | 구조 | 길이| 설명|
      | :--: |:--:| :--:|
      | 오브젝트 번호 | 1~6 | 오브젝트(Object)별로 유일한 값을 가지고 있으며, 해당 오브젝트가 속해 있는 값이다.|
      | 상대 파일 번호 | 7~9 | 테이블스페이스(Tablespace)에 속해 있는 데이터 파일에 대한 상대 파일번호이다.|
      | 블록 번호 | 10~15 | 데이터 파일 내부에서 어느 블록에 데이터가 있는지 알려준다.|
      | 데이터 번호 | 16~18 | 데이터 블록에 데이터가 저장되어 있는 순서를 알려준다.|

      - ORACLE db가 기준이기에 다른 dbms에선 문법의 차이가 있을 수 있다.

    </div>
    </details>
`WITH구문`
  - **WITH구문은 서브 쿼리(SUB QUERY)를 사용해서 임시 테이블이나 뷰처럼 사용할 수 있는 구문**이다.
  - 서브쿼리 블록에 별칭을 정할 수 있다.
  - 옵티마이저는 SQL을 인라인 뷰나 임시테이블로 판단한다.





  - **WITH구문은 서브 쿼리(SUB QUERY)를 사용해서 임시 테이블이나 뷰처럼 사용할 수 있는 구문**이다.
  - 

## REFERENCE
<https://www.inflearn.com/course/lecture?courseSlug=sqld-%EC%9E%90%EA%B2%A9%EC%A6%9D-%EC%B4%88%EA%B8%89-2&unitId=103382&tab=curriculum>


