## 옵티마이저(Optimizer)와 실행 계획
1. 옵티마이저(Optimizer)
   - SQL 개발자가 SQL을 작성하여 실행할 때, 옵티마이저는 SQL을 어떻게 실행할 것인지를 계획하게 된다.
   - 즉 , SQL 실행계획(Execution Plan)을 수립하고 SQL을 실행한다.
   - **옵티마이저는 SQL의 실행 계획을 수립하고 SQL을 실행하는 데이터베이스 관리 시스템의 소프트웨어이다.**
   - 동일한 결과가 나오는 SQL도 어떻게 실행하느냐에 따라서 성능이 달라진다.
     - 따라서 옵티마이저의 실행 계획은 SQL 성능에 아주 중요한 역할을 한다.
2. 옵티마이저의 특징
   - 옵티마이저는 데이터 딕셔너리(Data Dictionary)에 있는 오브젝트 통계, 시스템 통계 등의 정보를 사용해서 예상되는 비용을 산정한다. 
   - **옵티마이저는 여러 개의 실행 계획 중에서 최저비용을 가지고 있는 계획을 선택해서 SQL을 실행한다.**
3. 옵티마이저의 필요성
   - SQL개발자가 작성한 SQL문을 어떻게 실행 하느냐에 따라서 성능이 달라진다.
   - 옵티마이저가 이러한 실행 계획을 수립하는데 만약 옵티마이저가 비효율적으로 실행 계획을 수립하면, SQL개발자는 SQL을 개선해야 한다.
     - 이때 옵티마이저에게 실행 계획을 변경하도록 요청할 수가 있는데, 이때 힌트(Hint)를 사용한다. 
4. 옵티마이저의 실행 계획 확인
   - **옵티마이저는 실행 계획을 PLAN_TABLE에 저장한다.**
     - 개발자는 PLAN_TABLE을 조회해서 실행 계획을 확인할 수가 있다.
     
## 옵티마이저 종류
1. 옵티마이저의 실행 방법
  - 개발자가 SQL을 실행하면 파싱(Parsing)을 실행해서 SQL의 문법 검사 및 구문분석을 수행한다.
  - **구문분석이 완료되면 옵티마이저가 규칙 기반 혹은 비용 기반으로 실행 계획을 수립한다.**
  - **옵티마이저는 기본적으로 비용 기반 옵티마이저를 사용해서 실행 계획을 수립한다.**
  - **비용 기반 옵티마이저는 통계 정보를 활용해서 최적의 실행 계획을 수립하는 것이다.**
  - 실행 계획이 수립이 완료되면 최종적으로 SQL을 실행하고 실행이 완료되면 데이터를 인출(FETCH)한다.
  - <details>
    <summary>옵티마이저 엔진</summary>
    <div>

      | 옵티마이저 | 설명|
      | :--: | :--: |
      | Query Transformer | SQL문을 효율적으로 수행하기 위해서 옵티마이저가 변환한다. <br> SQL이 변환되어도 그 결과는 동일하다. |
      | Estimator | 통계정보를 사용해서 SQL 실행비용을 계산한다. <br> 총 비용은 최적의 실행 계획을 수립하기 위해서이다. |
      | Plan Generator | SQL을 실행할 실행 계획을 수립한다. |

    </div>
    </details>
2. 옵티마이저 엔진
  - 규칙 기반 옵티마이저(Rule base Optimizer)는 실행 계획을 수립할 때 15개의 우선순위를 기준으로 실행 계획을 수립한다.
  - **최신 Oracle 버전은 규칙 기반 옵티마이저 보다 비용 기반 옵티마이저를 기본적으로 사용한다.**
  - <details>
    <summary>우선 순위</summary>
    <div>

      | 우선 순위 | 설명|
      | :--: | :--: |
      | 1 | ROWID를 사용한 단일 행인 경우|
      | 2 | 클러스터 조인에 의한 단일 행인 경우 | 
      | 3 | 유일하거나 기본키(Primary Key)를 가진 해시 클러스터 키에 의한 단일 행인 경우 | 
      | 4 | 유일하거나 기본키(Primary Key)에 의한 단일 행인 경우 |
      | 5 | 클러스터 조인인 경우 |
      | 6 | 해시 클러스터 조인인 경우 |
      | 7 | 인덱스 클러스터 키인 경우 |
      | 8 | 복합 칼럼 인덱스인 경우 |
      | 9 | 단일 칼럼 인덱스인 경우 | 
      | 10 | 인덱스가 구성된 칼럼에서 제한된 범위를 검색하는 경우 |
      | 11 | 인덱스가 구성된 칼럼에서 무제한 범위를 검색하는 경우 |
      | 12 | 정렬-병합(Sort-Merge) 조인인 경우 |
      | 13 | 인덱스가 구성된 칼럼에서 Max 혹은 MIN을 구하는 경우 |
      | 14 | 인덱스가 구성된 칼럼에서 Order By를 실행하는 경우 |
      | 15 | 전체 테이블을 스캔(Full Table Scan)하는 경우 |
      
    </div>
    </details>
3. 비용 기반 옵티마이저(Cost base Optimizer)
   - 비용 기반 옵티마이저(Cost base Optimizer)는 오브젝트 통계 및 시스템 통계를 사용해서 총 비용을 계산한다.
   - **총 비용이라는 것은 SQL을 실행하기 위해서 예상되는 소요시간 혹은 자원의 사용량을 의미한다.**
   - 총 비용이 적은 쪽으로 실행 계획을 수립한다.
   - 단, 비용 기반 옵티마이저에서 통계 정보가 부적절한 경우 성능 저하가 발생할 수 있다.

## 인덱스
`정의`
   - **인덱스는 데이터를 빠르게 검색할 수 있는 방법을 제공한다.**
   - 인덱스는 인덱스 키로 정렬(SORT)되어 있기 때문에 원하는 데이터를 빠르게 조회한다. 
   - 인덱스는 오름차순(ASCENDING) 및 내림차순(DESCENDING) 탐색이 가능하다.
   - 하나의 테이블에 여러 개의 인덱스를 생성할 수 있고 하나의 인덱스는 여러 개의 칼럼으로 구성될 수 있다.
   - 테이블을 생성할 때 기본키(Primary Key)는 자동으로 인덱스가 만들어지고 인덱스의 이름은 SYSXXXXX이다.
   - 인덱스의 구조는 Root Block, Branch Block, Leaf Block으로 구성되고 Root Block은 가장 상위에 있는 노드를 의미하며

     Branch Block은 다음 단계의 주소를 가지고 있는 포인터(Pointer)로 되어 있다.
   - Leaf Block은 인덱스키와 ROWID로 구성되고 인덱스키는 정렬되어 저장되어 있다.
     - Leaf Block은 Double Linked List로 되어 있어서 양방향 탐색이 가능하다.
     - Leaf Block에서 인덱스 키를 읽으면 ROWID를 사용해서 EMP 테이블의 행을 직접 읽을 수 있다.

`생성`
  - 인덱스 생성은 "Create Index" 문을 사용해서 생성이 가능하다.
    - CREATE [UNIQUE} INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2, ...)  
  - 인덱스를 생성할 때는 한 개 이상의 칼럼을 사용해서 생성할 수 있다.
  - 인덱스키는 기본적으로 오름차순으로 정렬하고 "DESC"구를 포함하면 내림차순으로 정렬한다.

`스캔`
   1. 인덱스 유일 스캔(Index Unique Scan)
     - **INDEX UNIQUE SCAN은 인덱스의 키 값이 중복되지 않는 경우, 해당 인덱스를 사용할때 발생된다.**
   2. 인덱스 범위 스캔(Index Range Scan)
     - **INDEX RANGE SCAN은 SELECT문에서 특정 범위를 조회하는 WHERE문을 사용할 경우 발생한다.**
     - LIKE, BETWEEN이 대표적 예다, 데이터 양이 너무 적은 경우 인덱스 자체를 실행하지 않고, 성능 비교 후 Table Full Scan이 발생될 수 있다.
     - INDEX RANGE SCAN은 Leaf Block의 특정 범위를 스캔한 것이다.
   3. 인덱스 전체 스캔(Index Full Scan)
     - **INDEX FULL SCAN은 인덱스에서 검색되는 인덱스 키가 많은 경우에 Leaf Block의 처음부터 끝까지 전체를 읽어들인다.**
 
## 실행 계획
  - DB 버전에 따라 옵티마이저가 실행 계획을 수립하는 것이 다르기에 정답은 없고 옵티아미저가 항상 선택하는 것이다.

## 옵티마이저 조인
1. Nested Loop 조인
   - **Nested Loop조인은 하나의 테이블에서 데이터를 먼저 찾고 그 다음 테이블을 조인하는 방식으로 실행된다.**
   - Nested Loop조인에서 먼저 조회되는 테이블을 외부 테이블(Outer Table)이라고 하고 다음 조회되는 테이블을 내부 테이블(Inner Table)이라 한다.
   - Nested Loop조인에서는 외부 테이블(선행 테이블)의 크기가 작은 것을 먼저 찾는 것이 중요하다. 
     - 데이터가 스캔 되는 범위를 줄일 수 있기 때문 
   - **Nested Loop조인은 Random Access가 발생하는데 RANDOM ACCESS가 많이 발생하면 성능 저하가 발생하기 때문에 RANDOM ACCESS양을 줄여야 성능이 향상된다.**
2. Sort Merge 조인
   - Sort Merge 조인은 두 개의 테이블을 SORT_AREA라는 메모리 공간에 모두 로딩(Loading)하고 SORT를 수행한다.
   - 두개의 테이블에 대해서 SORT가 완료되면 두 개의 테이블을 병합(Merge)한다.
   - SORT MERGE 조인은 정렬(SORT)이 발생하기 때문에 데이터 양이 많아지면 성능이 떨어지게 된다.
   - 정렬 데이터양이 많으면 정렬은 임시 영역에서 수행된다. 임시 영역은 디스크에 있기 때문에 성능이 급격히 떨어지게 된다.
3. HASH 조인
   - Hash 조인은 두 개의 테이블 중에서 작은 테이블을 HASH메모리에 로딩하고 두 개의 테이블의 조인키를 사용해서 해시 테이블을 생성한다.
   - Hash 조인은 해시 함수를 사용해서 주소를 계산하고, 해당 주소를 사용해서 테이블을 조인하기 때문에 CPU 연산을 많이 한다.
   - 특히, Hash조인 시에는 선행 테이블이 충분히 메모리에 로딩되는 크기여야 한다.

#### 용어 학습
`High WaterMark`
   - Full Table Scan 테이블을 모두 읽은 것을 의미한다.
   - 테이블을 읽을 때 High WaterMark전까지만 Full Table Scan을 한다.
   - 테이블에서 데이터가 저장되어 있는 블록에서 최상위 위치를 의미하고, 데이터가 삭제,추가 되면 High WaterMark의 값도 같이 변경된다.

### Reference
<https://www.inflearn.com/course/lecture?courseSlug=sqld-%EC%9E%90%EA%B2%A9%EC%A6%9D-%EC%B4%88%EA%B8%89-3&unitId=112493&tab=curriculum> 
