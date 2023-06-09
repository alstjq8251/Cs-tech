## JOIN
1. EQUI(등가)조인 (교집합)
   1. EQUI(등가) 조인
    - **조인은 여러 개의 릴레이션을 사용해서 새로운 릴레이션을 만드는 과정이다.**
    - 조인의 가장 기본은 교집합을 만드는 것이다.
    - 두 개의 테이블 간에 일치하는 것을 조인한다.
      - SELECT * FROM EMP, DEPT WHERE EMP.DEPTNO = DEPT.DEPTNO; 
      - EQUI조인은 '='을 사용하여 두 개의 테이블을 연결한다. , where조건절에 추가 조건을 사용 가능하다.
    - EQUI 조인을 한 후에 실행 계획을 확인해서 내부적으로 두 테이블을 어떻게 연결했는지 확인할 수 있다.
    - 두 개 이상의 테이블 전체를 읽은 뒤에(TABLE ACCESS FULL) 해시 함수를 사용해서 두 개의 테이블을 연결한 것 이다.
    -  <details>
       <summary>해시 함수</summary>
       <div>
         
         - 해시 함수는 테이블을 해시 메모리에 적재한 후에 해시 함수로써 연결하는 방법이다.
            - 해시 함수는 EUQI조인만 사용 가능한 방법이다.
         - `해시 조인`
            - 해시 조인은 먼저 선행 테이블을 결정하고 선행 테이블에서 주어진 조건에(where 절) 해당하는 행을 선택한다.
            - 해당 행이 선택되면 조인 키(JOIN Key)를 기준으로 해시 함수를 사용해서 해시 테이블을 메인 메모리(Main Memory)에
         
              생성하고 후행 테이블에서 주어진 조건에 만족하는 행을 찾는다.
            - 후행 테이블의 조인 키를 사용해서 해시 함수를 적용하여 해당 버킷을 검색한다.
            - 해시코드 : 메모리를 구별할 수 있는 유일한 값을 정수값을 표식

       </div>
       </details>
   2. INNERJOIN
    - EQUI조인과 마찬가지로 ISO 표준 SQL로 INNER JOIN이 있다. 
      - INNER JOIN 은 ON문을 사용해서 테이블을 연결한다.
      - SELECT * FROM EMP INNERJOIN DEPT ON ~;
   3. INTERSECT 연산
    - **INTERSECT연산은 두 개의 테이블에서 교집합을 조회**한다.
    - **즉, 두 개의 테이블에서 공통된 값을 조회한다.**
      - SELECT DEPTNO FROM EMP INTERSECT SELECT DEPTNO FROM DEPT;  
      - 교집합의 조건이 되는 값을 칼럼값으로 지정해야 한다.
2. Non-EQUI(비등가) 조인
  - Non-EQUI는 두 개의 테이블간에 '='을 사용하지 않고 '>' '<' '>=' '<=' 등을 사용한다.
  - **즉 Non-EQUI는 정확하게 일치하지 않는 값을 조인하는 것**이다.
3. OUTER JOIN
  - **OUTER JOIN은 두 개의 테이블간에 교집합(EQUI JOIN)을 조회하고 한 쪽 테이블에만 있는 데이터도 포함시켜서 조회**한다.
    - 이때 **왼쪽 테이블에만 있는 행도 포함하면 LEFT OUTER JOIN이라고 하고 오른쪽에만 있는 행도 포함하면 RIGHT OUTER JOIN**이라고 한다.
    - **FULL OUTER JOIN은 LEFT OUTER JOIN 과 RIGHT OUTER JOIN 모두를 하는 것**이다.
    - oracle db에선 OUTER JOIN 을 할 때 '+' 기호를 사용해서 할 수 있다.
4. CROSS JOIN
  - **CROSS JOIN은 조인 조건구문 없이 2개의 테이블을 하나로 조인한다.**
  - **조인구가 없기 때문에 카테시안곱이 발생한다.**
    - 행이 14개 있는 테이블과 행이 4개있는 테이블을 조인하면 56개의 행이 조회된다.
  - CROSS JOIN은 FROM절에 "CROSS JOIN"구문을 사용하면 된다.
  - MERGE JOIN으로 실행결과가 출력된다.
5. UNION을 사용한 합집합 구현
   1. UNION
     - **UNION연산은 두 개의 테이블을 하나로 만드는 연산**이다.
       - 즉,두 개의 테이블을 하나로 합치는 것이며, 두 개의 테이블의 칼럼 수 , 칼럼의 데이터 형식 모두가 일치해야 한다.
       - 만약 두 개의 테이블에 UNION 연산이 사용될 때 칼럼의 개수 혹은 데이터 형식이 다르면 오류가 발생한다.
     - UNION연산은 중복된 데이터를 제거하면서 테이블을 합친다.
       - 그래서 UNION 연산은 정렬(SORT)과정을 발생시킨다.
     - ex: SELECT U.USERID FROM USERTBL UNION B.USERID FROM BUYTBL  
   2. UNION ALL
     - **UNION ALL은 두 개의 테이블을 하나로 합치는 것이다. UNION처럼 중복을 제거하거나 정렬을 유발하지 않는다.**
     - ex: SELECT U.USERID FROM USERTBL UNION ALL B.USERID FROM BUYTBL  
 6. 차집합을 만드는 MINUS
   - **MINUS 연산은 두 개의 테이블에서 차집합을 조회한다.**
   - **즉, 먼저 쓴 SELECT문에는 있고 뒤에 쓰는 SELECT문에는 없는 집합을 조회하는 것**이다.
   - MS-SQL에서는 MINUS와 동일한 연산이 EXCEPT이다.
   - MYSQL에서는 지원하지 않는다.(NOT IN, NOT EXISTS등 사용)
   - SELECT U.USERID FROM USERTBL U MINUS B.USERID FROM BUYTBL B; 

## 계층형 조회(Connect by)
![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/8aed385c-f401-437c-b0a3-41d0f257ad4e)

`정의`
  - **계층형 조회는 ORACLE 데이터베이스에서 지원하는 것으로 계층형으로 데이터를 조회할 수 있다.**
  - 직급으로 예를들면 차장 부장 팀장 대리 사원 순으로 트리 형태의 구조를 위에서 아래로 탐색하면서 조회하는 것이다.
    - 역방향 조회 또한 가능하다.
- **Connect by는 트리(Tree)형태의 구조로 질의를 수행하는 것으로 START WITH구는 시작 조건을 의미하고 CONNECT BY PRIOR는 조인 조건이다.**
- **Root노드로부터 하위 노드의 질의를 실행한다.**
- **계층형 조회에서 MAX(LEVEL)을 사용하여 최대 계층 수를 구할 수 있다.**
   - 즉, 계층형 구조에서 마지막 Left Node의 계층 값을 구한다.
   - SELECT MAX(LEVEL) FROM C#PERPEAR.EMP START WITH MGR IS NULL CONNECT BY PRIOR EMPNO = MGR; 
- **계층형 조회의 결과를 명확히 보기 위해서 LPAD함수를 사용할 수 있다.**
   - SELECT LEVEL, LPAD(' ',4*(LEVEL-1)) || EMPNO , MGR, CONNECT_BY_ISLEAF FROM EMP START WITH MGR IS NULL

     CONNECT BY PRIOR EMPNO = MGR;
     
     - CONNECT_BY_ISLEAF: 계층형 쿼리에서 해당하는 로우가 자식노드가 있는지 없는지 여부를 체크 (있을 경우 0 , 없을 경우 1)
     - CONNECT_BY_ROOT: 계층형 쿼리에서 최상위 노드를 찾고자 할 때 사용
     - ORACLE LPAD 함수: 좌측에 자릿수 만큼 채워주는 함수 LPAD(변수,길이,변형자)
     - <details>
       <summary>CONNECT BY 키워드</summary>
       <div>
          
          | 키워드 | 설명|
          | :--: | :--: |
          | LEVEL | 검색 항목의 깊이를 의미한다. 즉, 계층 구조에서 가장 상위 레벨이 1이 된다.|
          | CONNECT_BY_ROOT | 계층 구조에서 가장 최상위 값을 표시한다.|
          | CONNECT_BY_ISLEAF | 계층 구조에서 가장 최하위를 표시한다.|
          | SYS_CONNECT_BY_PATH | 계층 구조에서 전체 전개 경로를 표시한다.|
          | NOCYCLE | 순환구조가 발생지점까지만 발생한다.|
          | CONNECT_BY_ISCYCLE | 순환구조 발생 지점을 표시한다.|

       </div>
       </details>

> MYSQL에서는 SELF JOIN 이 계층형 조회같은 역할을 지원한다.

## SUB QUERY
1. MAIN QUERY 와 SUB QUERY 
   - **Subquery는 SELECT문 내에 다시 SELECT문을 사용하는 SQL문**이다.
   - **Subquery의 형태는 FROM구에 SELECT문을 사용하는 인라인 뷰(Inline View)와 SELECT문에 Subquery를 사용하는**

     **스칼라 서브쿼리(Scala subquery)등**이 있다.
   - **WHERE구에 SELECT문을 사용하면 서브쿼리(Sub Query)라고 한다.**
   - FROM구에 SELECT문을 사용하여 가상의 테이블을 만드는 효과를 얻을 수 있다.
2. 단일 행 서브쿼리와 다중 행 서브쿼리
   - 서브쿼리(Sub query)는 반환하는 행 수가 한 개인 것과 여러 개인 것에 따라서 단일 행 서브쿼리와 멀티 행 서브쿼리로 분류된다.
   - **단잃 행 서브쿼리는 단 하나의 행만 반환하는 서브쿼리로 비교 연산자(=,>,<,>=,<=,<>)를 사용한다.**
   - 다중 행 서브쿼리는 여러 개의 행을 반환하는 것으로 IN,ANY,ALL,EXISTS를 사용해야 한다.
  - <details>
    <summary>단일 행 서브쿼리 , 다중 행 서브쿼리</summary>
    <div>

       | 서브쿼리 종류 | 설명|
       | :--: | :--: |
       | 단일 행 서브쿼리 | 서브쿼리를 실행하면 `그 결과는 반드시 한 행만 조회`된다. <br> 비교 연산자(=,>,<,>=,<=,<>)를 사용한다.|
       | 다중 행 서브쿼리 | 서브쿼리를 실행하면 그 결과는 여러 개의 행이 조회 된다. <br> 다중 행 비교 연산자인 `IN,ANY,ALL,EXISTS`를 사용한다.|

    </div>
    </details>
   
    1. ANY
       - select * from emp where empno > ANY (SELECT empno from usertbl where empname ='경남')
       - 서브쿼리 앞에 **ANY의 의미는 OR의 개념과 비슷하다. 즉 비교대상들중 조건만 만족한다면 모두 출력하겠다는 의미**이다.
       - **아울러 ANY와 동일한 역할을 하는 것으로 SOME도 있다.**
    2. ALL
       - **ALL은 서브쿼리의 결과 값 둘 다 만족하는 데이터만 출력**한다. 즉 AND의 개념과 유사
    3. IN()
       - **IN()은 Main query의 비교조건이 Subquery의 결과 중 하나만 동일하면 참이 된다.(OR과 유사)**
       - IN은 반환하는 여러 개의 행 중에서 하나만 참이 되어도 참이 되는 연산이다.
    4. EXISTS
       - **EXISTS는 Subquery로 어떤 데이터 존재 여부를 확인하는 것 이다.**
       - 즉 EXISTS의 결과는 참과 거짓인 BOOLEAN값이 반환된다.
       - 첫 번째 SELECT 문의 결과를 토대로 하여 WHERE EXISTS의 SELECT를 비교해서 맞는 행이 있으면 출력한다. DISTINCT와 비슷한 역할 수행
3. 스칼라(Scala Subquery)
   - **스칼라 Subquery는 반드시 한 행과 한 칼럼만 반환하는 서브쿼리이다. 만약 여러 행이 반환되면 오류가 발생한다.**
   
## 그룹함수
1. ROLLUP
  - **ROLLUP은 GROUP BY 칼럼에 대해서 Subtotal을 만들어 준다.**
  - ROLLUP을 할 때 GROUP BY구에 칼럼이 두 개 이상 오면 순서에 따라서 결과가 달라진다.
2. GROUPING 함수
  - **GROUPING 함수는 ROLLUP, CUBE, GROUPING SETS에서 생성되는 합계 값을 구분하기 위해서 만들어진 함수**이다.
  - 예로 소계, 합계등이 계산되면 GROUPING함수는 1을 반환하고 그렇지 않으면 0 을 반환해서 합계 값을 식별할 수 있다
3. GROUPING SETS 함수
  - **GROUPING SETS 함수는 GROUP BY에 나오는 칼럼의 순서와 관계 없이 다양한 소계를 만들 수 있다.**
  - GROUPING SETS 함수는 GROUP BY에 나오는 칼럼의 순서와 관계 없이 개별적으로 모두 처리한다.
4. CUBE 함수
  - **CUBE는 CUBE 함수에 제시한 칼럼에 대해서 결합 가능한 모든 집계를 계산**한다.
  - 다차원 집계를 제공하여 다양하게 데이터를 분석할 수 있게 한다.
  - 즉, 조합할 수 있는 경우의 수가 모두 조합되는 것이다.
   
## 윈도우 함수
1. 윈도우 함수
   - **윈도우 함수는 행과 행간의 관계를 정의하기 위해서 제공되는 함수**이다.
   - 윈도우 함수를 이용해서 순위, 합계 , 평균 , 행 , 위치 등을 조작할 수 있다.
   - <details>
     <summary>윈도우 함수 구조</summary>
     <div>

        | 구조 | 설명|
        | :--: | :--: |
        | ARGUMENTS(인수) | 윈도우 함수에 따라서 0~N개의 인수를 설정한다. |
        | PARTITION BY | `전체 집합을 기준에 의해 소그룹`으로 나눈다. |
        | ORDER BY | 어떤 항목에 대해서 정렬한다.|
        | WINDOWING | 행 기준의 범위를 정한다. <br> `ROWS는 물리적 결과의 행 수`이고 `RANGE는 논리적인 값에 의한 범위`이다.|

     </div>
     </details>
   
      - <details>
        <summary>WINDOWING</summary>
        <div>

           | 구조 | 설명|
           | :--: | :--: |
           | ROWS | 부분집합인 윈도우 크기를 물리적 단위로 행의 집합을 지정한다. |
           | RANGE | 논리적인 주소에 의한 행 집합을 지정한다. |
           | BETWEEN ~ AND | 윈도우의 시작과 끝의 위치를 지정한다. |
           | UNBOUNDED PRECENDING | 윈도우의 시작 위치가 첫 번째 행임을 의미한다. |
           | UNBOUNDED FOLLOWING | 윈도우의 마지막 위치가 마지막 행임을 의미한다. |
           | CURRENT ROW | 윈도우의 시작 위치가 현재 행임을 의미한다.|
           
           - **CURRENT ROW는 데이터가 인출된 현재 행임을 의미**한다.

        </div>
        </details>
2. 순위함수
   - 윈도우 함수는 특정 항목과 파티션에 대해서 순위를 계산할 수 있는 함수를 제공한다.
   - **순위 함수는 RANK, DENSERANK, ROW_NUMBER 함수**가 있다.
   - <details>
     <summary>순위(RANK)관련 윈도우 함수</summary>
     <div>

        | 순위 함수 | 설명|
        | :--: | :--: |
        | RANK | 특정 항목 및 파티션에 대해서 순위를 계산한다. <br> `동일한 순위는 동일한 값이 부여`된다. |
        | DENSERANK | `동일한 순위를 하나의 건수로 계산`한다.|
        | ROW_NUMBER | 동일한 순위에 대해서 `고유의 순위를 부여`한다.|
        
        - **RANK()함수는 순위를 계산하며, 동일한 순위에는 같은 순위가 부여**된다.
        - **DENSERANK()함수는 동일한 순위를 하나의 건수로 인식해서 조회**한다.
        - **ROW_NUMBER()함수는 동일한 순위에 대해서 고유한 순위를 부여**한다.

     </div>
     </details>
3. 집계함수
   - <details>
     <summary>집계(AGGREGATE)관련 윈도우 함수</summary>
     <div>

        | 집계 함수 | 설명|
        | :--: | :--: |
        | SUM | 파티션 별로 합계를 계산한다. |
        | AVG | 파티션 별로 평균을 계산한다. | 
        | COUNT | 파티션 별로 행 수를 계산한다. |
        | MAX와 MIN | 파티션 별로 최댓값과 최솟값을 계산한다. |

     </div>
     </details>
4. 행 순서 관련 함수
   - 행 순서 관련함수는 상위 행의 값을 하위에 출력하거나 하위 행의 값을 상위 행에 출력할 수 있다.
   - 특정 위치의 행을 출력할 수 있다.
   - <details>
     <summary>행 순서 관련 윈도우 함수</summary>
     <div>

        | 행 순서 | 설명|
        | :--: | :--: |
        | FIRST_VALUE | 파티션에서 `가장 처음에 나오는 값`을 구한다. <br> `MIN함수를` 사용해서 같은 값을 구할 수 있다.|
        | LAST_VALUE | 파티션에서 `가장 나중에 나오는 값`을 구한다. <br> `MAX함수를` 사용해서 같은 값을 구할 수 있다.|
        | LAG | `이전 행`을 가지고 온다.|
        | LEAD | 윈도우에서 `특정 위치의 행을 가지고 온다.` <br> 기본값은 1이다.|
 
     </div>
     </details>
5. 비율 관련 함수
   - 비율 관련 함수는 누적 백분율, 순서별 백분율, 파티션을 N분으로 분할한 결과 등을 조회할 수 있다.
   - <details>
     <summary>비율 관련 관련 윈도우 함수</summary>
     <div>

        | 비율 함수 | 설명|
        | :--: | :--: |
        | CUME_DIST | 파티션 전체 건수에서 현재 행보다 작거나 같은 건수에 대한 누적 백분율을 조회한다. <br> 누적 분포상에 위치를 0~1사이의 값을 가진다.|
        | PERCENT_RANK | 파티션에서 제일 먼저 나온 것을 0으로 제일 늦게 나온 것을 1로 하여 값이 아닌 행의 순서별 백분율을 조회한다.|
        | NTILE | 파티션별로 전체 건수를 ARGUMENT값으로 N등분한 결과를 조회한다.|
        | RATIO_TO_REPORT | 파티션 내에 전체 SUM(칼럼)에 대한 행 별 칼럼값의 백분율을 소수점까지 조회한다.
 
     </div>
     </details>
## 테이블 파티션
1. Partition 기능
   - **파티션은 대용량의 테이블을 여러 개의 테이블 파일에 분리해서 저장한다.**
   - 테이블의 데이터가 물리적으로 분리된 데이터 파일에 저장되면 입력,수정,삭제,조회 성능이 향상된다.
   - 파티션은 각각의 파티션 별로 독립적으로 관리될 수 있다. 즉, 파티션 별로 백업하고 복구가 가능하면 파티션 전용 인덱스도 생성이 가능하다.
   - 파티션은 Oracle 데이터베이스의 논리적 관리 단위인 테이블 스페이스 간에 이동이 가능하다.
      - 테이블 스페이스: 데이터베이스 오브젝트 내 실제 데이터를 저장하는 공간이다.
          - 이것은 데이터베이스의 물리적 부분과 논리적 부분으로 나뉜다.
      - 데이터를 조회할 때 데이터의 범위를 줄여서 성능을 향상시킨다.
2. Range Partition
   - **Range Partition은 테이블의 칼럼 중에서 값의 범위를 기준으로 여러 개의 파티션으로 데이터를 나누어 저장하는 것이다.**
3. List Partition
   - **List Partition은 특정 값을 기준으로 분할하는 방법이다.**
4. Hash Parition 
   - **Hash Partition은 데이터베이스 관리 시스템이 내부적으로 해시 함수를 사용해서 데이터를 분할한다.**
      - 결과적으로 데이터베이스 관리 시스템이 알아서 분할하고 관리하는 것이다.
5. 파티션 인덱스 
   - 파티션 인덱스는 4가지 유형의 인덱스를 제공한다.
   - 즉, 파티션 키를 사용해서 인덱스를 만드는 Prefixed Index와 해당 파티션만 사용하는 Local Index로 나뉜다.
   - Oracle 데이터베이스는 Global Non-Prefixed를 지원하지 않는다.
   - <details>
     <summary>파티션 인덱스</summary>
     <div>

        | 구분 | 주요 내용|
        | :--: | :--: |
        | Global Index | 여러 개의 파티션에서 하나의 인덱스를 사용한다. |
        | Local Index | 해당 파티션 별로 각자의 인덱스를 사용한다. |
        | Prefixed Index | 파티션 키와 인덱스 키가 동일하다. |
        | Non Prefixed Index | 파티션 키와 인덱스 키가 다르다. |
 
     </div>
     </details>
   
   
   
1. Partition 기능
   
   
### REFERENCE
<https://www.inflearn.com/course/lecture?courseSlug=sqld-%EC%9E%90%EA%B2%A9%EC%A6%9D-%EC%B4%88%EA%B8%89-2&unitId=103395&tab=curriculum><br>
<https://blog.naver.com/javaking75/220010288704>



