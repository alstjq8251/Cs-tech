### Batch의 개념
`배치`
  - 데이터를 실시간으로 처리 하지 않고 **일괄적으로** 모아서 처리하는 작업을 일컫는다.
    - 사용자의 요청에 대해 실시간으로 대응하지 않아도 되는 서비스에 적용한다.
  - 배치의 핵심은 **대량의 데이터를 , 특정 시간에, 일괄적으로 처리한다**라고 정의할 수 있다.

#### Batch와 반대되는 개념
`OLTP(Online Transaction Processing)`
  - 네트워크 상의 여러 이용자가 실시간으로 데이터 베이스의 데이터를 갱신하거나 조회하는 등의 단위 작업을 처리하는 방식을 말한다.
  - 많은 사용자가 이용할 수 있도록 송,수신 데이터를 트랜잭션 단위로 압축하고 이런 데이터 역시 각각의 트래잭션 단위로 처리하게 된다.

#### Batch와 Oltp를 실무적으로 이용할때는?
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236451589-0ed1ddd5-e2f2-4d27-937b-f8892e8263b5.png">

- 상단의 사진에 보이는 것 처럼 송금은 실시간으로 사용자의 요청에 응답해야 하기 때문에 Batch로 처리하기 적절하지 않아 oltp방식으로 각각의 트랜잭션

  범위를 나눠 요청과 응답을 처리한다.(Ex: 송금 시 목적지로 바로 송금이 되는 기능을 의미 - 각각의 트랜잭션 단위)
  
- 정산이나 대용량 데이터를 처리하는 것은 실시간으로 처리하기에 적합하지 않다. `Batch`
  - 정산이나 대용량 데이터를 처리하는 것은 범위에 따라 다르지만 작지 않아 많은 리소스(시간 , 비용)를 요하는데 이때 해당 기능을 처리하기 위해

  - 실시간으로 대응하는 송금등의 기능을 방해해선 안되기 때문에 일정 주기를 기반으로 대용량 데이터를 처리해 서버가 기능을 안정적으로 대응하게 하는 것 이다.

#### Spring Batch 탄생 배경
`자바 기반 표준 배치 처리 기술 부재`
  - 배치 처리에서 요구하는 재사용 가능한 자바 기반 배치 아키텍쳐 표준 기술의 필요성의 대두 (자바 표준 기술은 JSR이라고 하며 I,O등이 포함)
- SpringBatch는 SpringSource(현재는 pivol)와 Accentual(경영 컨설팅 기업)의 합작품이다.
  - Accentual - 배치 아키텍쳐를 구현하면서 쌓은 기술적인 경험과 노하우
  - SpringSource - 깊이있는 기술적 기반과 스프링 프로그래밍 모델
  
Accentual은 이전에 소유했던 배치 처리 아키텍쳐 프레임 워크를 Spring Batch프로젝트에 기증함

#### Batch의 핵심 패턴
- `Read` - 데이터베이스 , 파일 , 큐에서 데이터를 조회한다.
- `Process` - 특정 방법으로 데이터를 가공한다.
- `Write` - 데이터를 수정된 양식으로 다시 저장한다.

```
위와같은 배치의 핵심 패턴은 db의 ETL과도 비슷하다.
Extract - 추출하다.
Transform - 변형하다.
Load - 적재한다.
```

#### Batch 시나리오
1. 배치 프로세스를 주기적으로 커밋
2. 동시 다발적인 Job의 배치 처리 , 대용량 병렬 처리(멀티 스레드로 job을 처리하는 것을 의미)
3. 실패 후 수동 또는 스케쥴러에 의한 재시작
4. 의존관계가 있는 Step 여러개를 순차적으로 처리
5. 조건적 flow 구성을 통한 체계적이고 유연한 배치 모델 구성 ( 조건에 따른 step 실행을 flow라 명명)
6. 반복 , 재시도 , Skip 처리

### Batch Architecture
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236454840-5e444d3c-5ba8-4f2c-8578-2815181c6dba.png">

#### Spring Batch 초기화 설정 클래스
`@EnableBatchProcessiong`어노테이션이 선언되면 하위의 4개의 설정클래스가 초기화된다.
1. `BatchAutoConfiguration`
  - 스프링 배치가 초기화 될 때 자동으로 실행되는 설정 클래스
  - Job을 수행하는 JobLauncherApplicationRunner 빈을 생성
2. `SimpleBatchConfiguration`
  - `JobBuilderFactory`와 `StepBuilderFactory`생성
  - 스프링 배치의 주요 구성 요소 생성 - 프록시 객체로 생성됨
3. `BatchConfigurerConfiguration`
  - `BasicBatchConfigurer`
    - `SimpleBatchConfiguration`에서 생성한 프록시 객체의 실제 대상 객체를 생성하는 설정 클래스
    - 빈으로 의존성을 주입받아서 주요 객체들을 참조해서 사용할 수 있다.  
  - `JpaBatchConfigurer`
    - Jpa관련 객체를 생성하는 설정 클래스
  - 사용자 정의 `BatchConfigurer` 인터페이스를 구현하여 사용할 수 있다.

> 초기화 설정 클래스가 로드되는 순서는 하단의 사진과 같다.
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236474246-fe8c01f2-7c5c-4c95-b7f4-18830d7512d3.png">

#### Spring Batch Db Schema
**SpringBatch는 기본적으로 배치의 실행 및 관리를 위한 목적으로 db에 데이터를 저장한다.**
1. `스프링 배치 메타 데이터`
  - 스프링 배치의 실행 및 관리를 위한 목적으로 여러 도메인들(Job, Step, JobParameters)의 정보들을 저장, 업데이트, 조회할 수 있는 스키마 제공
  - 과거, 현재의 실행에 대한 세세한 정보, 실행에 대한 성공과, 실패 여부 등을 일목요연하게 관리함으로서 배치운용에 있어 리스크 발생 시 빠른 대처 가능
  - DB와 연동할경우 필수적으로 메타데이터를 생성해야함
2. `DB 스키마 제공`
  - 파일 위치 :/org/springframework/batch/core/schema/*.sql
  - DB 유형별로 제공
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236498922-4d593e5f-769c-4516-9737-5af657dbb32b.png">

> 상단의 스크립트로 생성되는 db스키마는 하단의 사진과 같다.
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236500631-e7a9f885-be5f-4e4a-995e-91382f7d6e6e.png">

> 생성되는 각각의 테이블을 하단에서 설명한다. (테이블 앞에 붙는 Batch는 기본 prefix로서 바꿀 수 있다.)
`Job 관련 테이블`
  1. `Batch_Job_Instance`
    - `Job`이 실행될 때 `Instance`정보가 저장되며 job_name과 job_key를 키로 하여 하나의 데이터가 저장
    - 동일한 job_name과 job_key로 중복 저장될 수 없다.
  2. `Batch_Job_Execution`
    - job의 실행정보가 저장되며 job 생성, 시작 , 종료 시간, 실행상태, 메세지 등을 관리
  3. `Batch_Job_Execution_Parameters`
    - job과 함께 실행되는 jobparameter정보를 저장
  4. `Batch_Job_Execution_Context`
    - Job의 실행동안 여러가지 상태정보, 공유정보를 직렬화(Json형태) 하여 저장
    - Step간 서로 공유 가능함
`Step 관련 테이블`
  1. `Batch_Step_Execution`
    - Step의 실행정보가 저장되며 생성,시작, 종료시간, 실행상태, 메세지 등을 관리
  2. `Batch_Step_Execution_Context`
    - Step의 실행동안 여러가지 상태 정보, 공유 데이터를 직렬화(Json형태)하여 저장
    - Step별로 저장되며 Step간 공유할 수 없음

3. 스키마 생성 설정
  1. `수동 생성`
    - 생성되어있는 쿼리를 복사 후 직접 실행하여 생성
  2. `자동 생성`
  
  | 명령어 | 실행내용 |
  | :--: | :--: |
  | `Always` | 스크립트 항상 실행 <br> RDBMS설정이 되어있을 경우 내장 DB보다 우선하여 실행|
  | `Embeded` | 내장 DB일때만 실행되며 스크립트 자동 생성 , 기본값임|
  | `Never` | 스크립트 항상 실행 안함<br> 내장 DB일 경우 스크립트가 생성이 되지 않기 때문에 오류 발생<br>운영에서 수동으로 스크립트 생성 후 실행하는 것을 권장|
  
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/236500358-3cb1b1a0-12c9-4539-94a1-7256b0fe919b.png">

> 생성 전략은 상단과 같이 yaml, properties에 선언한다.

#### SpringBatch Domain의 이해
`Job`

`JobInstance`
1. 개념
- Job이 실행될때 생성되는 Job의 논리적 실행 단위 객체로서 고유한 식별 가능한 작업 실행을 의미
- Job의 실행과 구성은 동일하지만 Job이 실행되는 시점에 처리하는 내용은 다르기 때문에 Job의 실행을 구별해야함
  - ex: 하루 한번씩 배치 Job이 실행된다면 매일 실행되는 각각의 Job을 JobInstance라고 표현ㅓ
- JobInstance 생성 및 실행
  - 처음 시작하는 Job + Jobparameter 일 경우 새로운 JobInstance 생성
  - 이전과 동일한 Job + Jobparameter 으로 실행할 경우 이미 존재하는 JobInstance 리턴
    - 내부적으로 jobname + jobkey(jobparameter의 해시값)를 가지고 JobInstance를 얻음
- Job과는 1:N 관계
2. BATCH_JOB_INSTANCE테이블과 매핑
  - Jobname(Job) + Jobkey(Jobparameter 해시값)가 동일한 데이터는 중복해서 저장할 수 없음

`JobParameter`
1. 개념
- Job을 실행할때 함께 포함되어 사용되는 파라미터를 가진 도메인 객체
- 하나의 Job에 존재할 수 있는 여러개의 JobInstance를 구분하기 위한 용도
- JobParameter와 JobInstance는 1:1관계
2. 생성 및 바인딩
- 어플리케이션 생성 시 주입
  - java -jar LogBatch.jar requestDate=2021
- 코드로 생성
  -  JobParameterBuilder, DefaultJobParaemterConverter
- SpEL
  - @Value("#{jobparameter[requestDate]}"), @JobScope, @JobScope 선언 필수
3. BATCH_JOB_EXECUTION_PARAM 테이블과 매핑
- JOB_EXECUTION 1:N 관계  

`JobExecution`
1. 개념
- JobInstance에 대한 한번의 시도를 의미하는 객체로서 Job 실행 중에 발생한 정보들을 저장하고 있는 객체
  - 시작시간 , 종료시간 , 상태(시작됨,완료,실패), 종료상태의 속성을 가짐
- JobInstance와의 관계
  - JobExecution은 `FAILED`또는 `COMPLETED` 등의 Job의 실행 결과 상태를 가지고 있음
  - JobExecution의 실행 상태 결과가 `COMPLETED`면 JobInstance의 실행이 완료된 것으로 간주해 재실행이 불가함 
  - JobExecution의 실행 상태 결과가 `FAILED`면 JobInstance의 실행이 완료되지 않은것으로 간주해서 재실행이 가능함
    - JobParameter가 동일한 값으로 Job을 실행할 지라도 계속해서 JobInstance를 실행할 수 있음
  - JobExecution의 실행 상태 결과가 `COMPLETED`가 될때 까지 하나의 JobInstance내에서 여러 번의 시도가 생길 수 있음

2. BATCH_JOB_EXECUTION 테이블과 매핑
- JobInstance와 JobExecution은 1:N관계로서 JobInstance에 대한 성공/실패 내역을 가지고 있음

`Step`
1. 개념
- Batch job을 구성하는 독립적인 하나의 단계로서 실제 배치 처리를 정의하고 컨트롤 하는데 필요한 모든 정보를 갖고 있는 도메인 객체
- 단순한 단일 테스크 뿐만 아니라 입력과 처리 그리고 출력과 관련된 복잡한 비지니스 로직을 포함하는 모든 설정을 담고 있다.
- 배치작업을 어떻게 구성하고 실행할 것인지 Job의 세부 작업을 task 기반으로 설정하고 명세해 놓은 객체
- 모든 Job은 하나 이상의 Step으로 구성됨

2. 기본 구현체
- `TaskletStep`
  - 가장 기본이 되는 구현체로서 tasklet타입의 구현체들을 제어한다.
- `PartitionStep`
  - 멀티 스레드 방식으로 Step을 여러개로 분리해서 실행한다.
- `JobStep`
  - Step내에서 Job을 실행하도록 한다.
- `FlowStep`
  - Step내에서 Flow를 실행하도록 한다.

`StepExecution`
1.개념
- Step에 대한 한번의 시도를 의미하는 객체로서 Step의 실행중에 발생한 정보들을 저장하고 있는 객체
  - 시작시간, 종료시간 ,상태(시작됨,완료,실패), commit count, rollback count 등의 속성을 가짐
- Step이 매번 시도될 때마다 생성되며 각 Step 별로 생성된다.
- Job이 재시작하더라도 이미 성공적으로 완료된 Step은 재 실행되지 않고 실패한 Step만 실행한다.
- 이전 단계 Step이 실패해서 현재 Step을 실행하지 않았다면 StepExecution을 생성하지 않는다. Step이 실제로 실행됐을 때만 StepExecution을 생성한다.
- JobExecution과의 관계
  - Step의 StepExecution이 모두 정상적으로 완료되어야 JobExecution이 정상적으로 완료된다.
  - Step의 StepExecution 중 하나라도 실패한다면 JobExecution은 실패한다.

2. BATCH_STEP_EXECUTION테이블과 매핑
- JobExecution과 StepExecution은 1:N 관계
- 하나의 Job의 여러 개의 Step으로 구성했을 경우 StepExecution은 JobExecution을 부모로 가진다.

`StepContribution`
1. 개념
- 청크 프로세스의 변경 내용을 버퍼링 한 후 StepExecution상태를 업데이트 하는 도메인 객체
- 청크 커밋 직전에 StepExecution의 apply메서드를 호출하여 상태를 업데이트하게 함
- ExitStatus의 기본 종료코드 외 사용자 정의 종료코드를 생성해서 적용할 수 있음
2. 구조 

|구조|속성|의미|
|:--:|:--:|:--:|
| | readcount| 성공적으로 read한 아이템 수|
| | writecount| 성공적으로 write한 아이템 수|
| | filtercount| ItemProcessor에 의해 필터링 된 아이템 수 |
| | parentSkipCount | 부모인 StepExecution의 총 skip 횟수|
| | readskipcount | read에 실패해서 skip된 횟수|
| | writeskipcount | write에 실패해서 skip된 횟수|
| | processSkipcount | process에 실패해서 skip된 횟수|
| | ExitStatus | 실행결과를 나타내는 클래스로서 종료코드를 포함(UNKNOWN, EXECUTING, COMPLETED, NOOP, FAILED, STOPED)|
| | StepExecution | StepExecution 객체를 저장|

`ExecutionContext`
1. 개념
- 프레임워크에서 유지 및 관리하는 키/값으로 된 컬렉션으로 StepExecution 또는 JobExecution객체의 상태(State)를 공유하는 공유 객체
- DB에 직렬화 한 값으로 저장됨{"key"-"value"}
- 공유 범위 
  - Step 범위 각 Step에 StepExecution에 저장되며 Step간 공유 안됨
  - Job 범위 각 Job에 JobExecution에 저장되며 Job간 서로 공유 안되며 해당 Job의 Step간 서로 공유됨
- Job 재시작 시 이미 처리한 Row데이터는 건너뛰고 이후로 수행하도록 할 때 상태정보를 활용한다.

`JobRepository`
1. 개념
- 배치 작업 중의 정보를 저장하는 저장소 역할
- Job이 언제 수행되었고, 언제 끝났으며, 몇번이 수행되었고 실행에 대한 결과 등의 배치 작업의 수행과 관련된 모든 meta deta를 저장함
  - JobLauncher, Job, Step 구현체 내부에서 모든 CRUD 기능을 처리함
2. JobRepository 설정
- @EnableBatchProcessing 어노테이션만 선언하면 JobRepository가 자동으로 빈으로 생성됨
- BatchConfigurer 인터페이스를 구현하거나 BasicBatchConfigurer를 상속해서 JobRepository설정을 커스터마이징 할 수 있다.
  - **JDBC방식으로 설정 - JobRepositoryFactoryBean**
    - 내부적으로 AOP기술을 통해 트랜잭션 처리를 해주고 있음
    - 트랜잭션 isolation의 기본값은 SERIALIZEBLE로 최고 수준, 다른 레벨(READ_COMMITED, REPEATABLE_READ)로 지정 가능
    - 메타테이블의 테이블 prefix를 변경할 수 있음 기본값은 "BATCH_"임
  - **In Memory 방식으로 설정 - MapJobRepositoryFactoryBean**
    - 성능 등의 이유로 도메인 오브젝트를 굳이 데이터베이스에 저장하고 싶지 않을 경우
    - 보통 Test나 프로토타입의 빠른 개발이 필요할 때 사용 
    -    


#### Reference
<https://velog.io/@power0080/%EA%B0%9C%EB%85%90%EC%9B%90%EB%A6%ACspring-batch-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC-1%ED%8E%B8><br>
<https://www.inflearn.com/course/lecture?courseSlug=%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B0%B0%EC%B9%98&unitId=91280><br>

