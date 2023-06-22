### Apache Kafka가 나오게 된 배경

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/133a328b-1200-4a1c-939e-23504928e007)

- Kafka이전엔 엔드투엔드(End-To-End)방식의 아키텍쳐를 사용했다.
  - 상단의 사진에서 보이는거처럼 하드웨어, 모니터링 간의 데이터 연동은 대단히 복잡해 보인다.
  - 이로 인해 하드웨어 , 인프라 간 데이터 연동의 복잡성(하드웨어, 운영체제, 장애)이 증가했다.
  - 각기 다른 데이터 파이프라인을 연동하는것도 어려웠으며 이를 확장하는것은 더더욱 어려웠다.
 
> 모든 시스템으로 데이터를 실시간으로 전송하면서 데이터가 갑자기 많아지더라도 확장이 용이한 시스템이 필요했고 그에 따라 kafka가 나오게 됐다. 

### Apache Kafka란?

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/4ce77f94-0eea-4339-b82f-90311b619ebd)

`특징`
- 이전의 복잡한 구조와 달리 카푸카로 메세지 , 이벤트를 전송하고 다시 카푸카가 각 엔드포인트로 전송하는 구조로 바뀌게 되었다.
- 이로인해 Producer와 Consumer를 분리했으며 메세지 데이터를 여러 컨슈머에게 허용함으로써 데이터를 여러번 사용하게 만들었다.
- 높은 처리량을 위해 메세지 최적화를 하였으며 스케일 아웃이 가능하다.
  - 데이터 처리량이 많아질때 was를 scale out하는 것과 같이 카푸카를 만든 뒤엔 무중단으로 스케일 아웃이 가능하다.
- 관련 생태계를 매우 다양하게 제공한다.

#### Kafka Broker 
`특징`
  - 실행된 카푸카 어플리케이션 서버 중 한대
  - 3대 이상의 브로커로 클러스터 구성
  - 주키퍼와 연동(~2.5.0버전)
    - 주키퍼는 메타데이터(브로커id, 컨트롤러id등 저장) 저장하는 역할을 한다.
  - n개 브로커중 한 대는 `컨트롤러(Controller)` 역할을 수행한다.
    - 각 브로커에게 담당파티션 할당 수행 , 브로커 정상 동작 모니터링 관리, 누가 컨트롤러 인지는 주키퍼에 저장
   
#### Record 
- 레코드란 전체 카프카 파이프라인에서 다루는 메시지의 최소단위라고 생각하면 된다. RDB의 row와 비슷하다고 생각하면 된다.
- 객체를 프로듀서에서 컨슈머로 전달하기 위해 kafka내부에 byte형태로 저장할 수 있도록 직렬화/역직렬화하여 사용한다.
  - 기본 제공 직렬화 class: StringSerializer, ShortSerializer
  - 커스텀 직렬화 class를 통해 `Custom Object 직렬화, 역직렬화 가능`
 
#### Topic * Partition
- `Topic`은 메세지 분류 단위이다.
- 1개의 토픽에 최소한 한개 이상의 파티션이 할당되어야 하며 n개의 파티션이 할당 가능하다.
- 각 파티션마다 고유한 `오프셋(Offset)`을 가진다.
- 메세지 처리순서는 파티션 별로 유지 관리됨
  - 파티션이 여러개일 때는 처리순서가 입력 순서로 처리되지 않을 수 있음
 
#### Kafka 동작 순서
1. `프로듀서는` 레코드를 생성하고 브로커로 전송
2. 전송된 레코드는 파티션에 신규 오프셋과 함께 기록된다.
3. `컨슈머는` 브로커로 부터 레코드를 요청하여 가져감(Polling)

#### Kafka log and Segment
`Segment`
  - 실제로 메세지가 저장되는 `파일시스템 단위`
  - 메세지가 저장될때는 세크먼트파일이 열려있다.
  - 세그먼트는 시간 또는 크기 기준으로 닫힌다.
  - 세그먼트가 닫힌 이후 일정 시간(용량)에 따라 삭제(delete) 또는 압축(compact)한다.

##### 운영단계별 카푸카 구조 
1. Producer 1 , partition 3 , consumer 1
  - Producer가 만드는 모든 레코드가 3개의 파티션에 할당되고 모든 파티션의 레코드를 하나의 컨슈머가 처리함
  - 한개의 컨슈머가 처리하는 속도에는 한계가 있기 때문에 처리속도가 저하되고 장애에 대응하기 어려움

2. Producer 1, partition 3, consumer 3
  - Producer가 만드는 모든 레코드가 3개의 파티션별로 할당되고 각각의 컨슈머에 메세지가 할당되어 처리된다.
  - `리밸런스 발생` 컨슈머에 장애가 생겼을 때 해당 파티션의 레코드를 정상적인 컨슈머에 다시 할당하여 처리하는 과정
  - 컨슈머가 놀지 않고 장애에도 대처할 수 있다.

3. Producer 1, partition 3, consumer 4
  - Producer가 만드는 모든 레코드가 3개의 파티션별로 할당되나 3개의 컨슈머가 레코드를 처리하고 한개의 컨슈머는 작업을 하지 않는다.
  - 한개의 컨슈머가 작업을 하지 않기에 효율적인 아키텍쳐 구성이라고 할 수 없다.

4. Producer 1, partition 3, consumer group 2개 이상(각각 한개의 컨슈머를 가짐)
  - 목적에 따른 컨슈머 그룹을 분리할 수 있다.
    - 장애에 대응하기 위해 재입수(또는 재처리) 목적으로 임시 컨슈머 그룹을 생성하여 사용하기도 한다.
    - ex: 애플리케이션 로그 적재용 
       - 엘라스틱 서치(실시간, 시간순 정렬)
       - hadoop(대용량 데이터 적재, 이전 데이터 확인)
    - 컨슈머 그룹 장애에 격리되는 컨슈머 그룹으로 구성하여 컨슈머 그룹간 간섭(coupling)을 줄인다.

#### Kafka Replication

`나오게 된 배경`
  - bin/kafka-topic.sh --ex-server localhost:9092 --create -topic ex_topic --partition3
     - 카푸카에서 토픽을 만드는 명령어이며 상단과 같이 입력할 시 브로커 3개에 각각의 파티션이 할당된다.
     - 이 때 1번 브로커가 장애가 발생했을 시 복구되기 전까지 파티션1이 사용 불가하다.
     - 장애를 대응하기 위해 레플리케이션이 도입되었다.
   
`예시 명령어`
  - bin/kafka-topic.sh --ex-server localhost:9092 --create -topic ex_topic --partition3 --replication-factor 3
  - 3개의 브로커에 있는 것중 1번 브로커의 1번 파티션이 2번 3번으로 복제되게 된다.

`리더 파티션`
  - Kafka 클라이언트와 data를 주고받는 역할

`팔로워 파티션`
  - 리더 파티션으로부터 지속적으로 데이터를 복제(복제하는데 시간이 걸린다.)
  - 리더 파티션의 장애가 발생했을 시 나머지 팔로워 중 한개가 리더로 선출된다.

`ISR(In Sync Replica)`
  - 파티션 3개, 어플리케이션 3개로 이루어진 토픽이 브로커에 할당되어 리더, 팔로워 파티션이 모두 복제되 sync가 맞는 상태
  - ISR이 아닌 상태에서 장애가 발생하면 
    - unclean.leader.election.enable false - 리더 파티션의 데이터가 모두 복제되고 복구될때 까지 기다린 후 수행
    - unclean.leader.election.enable true - 리더 파티션의 데이터가 복제, 복구되지 않아도 수행

#### Kafka rack-awareness
  - 1개의 Rack에 다수의 브로커를 몰아넣는것은 위험하다.
    - rack이 다운되면 모든 서버가 다운되기 때문(가용성 측면)
  - 다수의 rack에 배치하여 브로커 옵션(brocker.rack) 설정 및 배치
    - 파티션 할당 및 레플리케이션 동작시 특정 브로커에 몰리는 현상 방지
   
#### Kafka가 서버 장애에 대응하는 로직이 많은 이유?
- 서비스 운영에 있어 `장애 허용(Fault-tolerant)는` 매우 중요하다.
  - 서버의 중단(이슈발생, 재시작)은 언제든지 발생할 수 있기 때문이다.
  - 카푸카는 대규모 분산 메세지 , 이벤트를 처리하기 때문에 일부 서버가 중단되더라도 데이터가 유실되면 안된다.
    - 안정성이 보장되지 않으면 신뢰도가 하락해 사용하지 않기 때문이다.


#### Kafka 핵심 요소

`Broker` 
  - 카푸카 어플리케이션 서버 단위

`Topic`
  - 데이터 분리 단위, 다수 파티션 보유

`Partition`
  - 레코드를 담고 있음 , 컨슈머 요청시 레코드 전달

`Offser`
  - 각 레코드당 파티션에 할당된 고유 번호

`Consumer`
  - 레코드를 Polling하는 어플리케이션
    - `Consumer group`
       - 다수 컨슈머 묶음
    - `Consumer Offset`
       - 특정 컨슈머가 가진 레코드 번호

`Producer`
  - 레코드를 브로커로 전송하는 어플리케이션

`Replication`
  - 파티션 복제 기능
  - `ISR`
     - 리더+팔로워 파티션의 sync가 된 묶음

`Rack-awarence`
  - Server rack 이슈에 대응

#### kafka Streams
`특징`
  - 데이터를 변환(transform)하기 위한 목적으로 사용되는 API
  - 스트림 프로세싱을 지원하기 위한 다양한 기능을 제공한다.
    - stateful , stateless 와 같이 상태기반 스트림 처리 가능
    - stream api와 DSL(Domain Specific Language)를 동시 지원
    - Exactly-once 처리, 고가용성 특징
    - Kafka Security(acl, sasl 등) 지원
    - 스트림 처리를 위한 별도 클러스터(ex: yarn 등) 불필요

#### Kafka Connect
`특징`
  - Kafka Client로 데이터를 넣는 코드를 작성할 때도 있지만, kafka connect를 통해 data를 import, export할 수 있다.
  - 코드 없이 Configuration으로 데이터를 이동시키는 것이 목적
    - standalone mode, distribution mode 지원
    - Rest api interface를 통해 제어
    - stream 또는 batch 형태로 데이터 전송 가능
    - 커스텀 connector를 통한 다양한 plugin 제공(file, s3, hive, mysql 등)

#### Kafka Mirror Maker
`특징`
  - 특정 카프카 클러스터에서 다른 카프카 클러스터로 Topic 및 Record를 복제하는 Standalone tool
  - 클러스터간 토픽에 관한 모든 것을 복제하는 것이 목적
    - 신규 토픽, 파티션 감지 기능 및 토픽 설정 자동 Sync 기능
    - 양방향 클러스터 토픽 복제
    - 미러링 모니터링을 위한 다양한 metric(latency, count 등) 제공

#### Kafka 를 지원하는 다양한 어플리케이션
1. conflunet/ksqlDB
   - sql 구문을 통한 stream data processing 지원
2. confluent/schema Registry
   - avro 기반의 스키마 저장소
3. confluent/Rest Proxy
   - Rest Api를 통한 Producer/consumer
4. linkedin/kafka burrow
   - consumer lag 수집 및 분석
5. yahoo/CAMK
   - 카푸카 클러스터 매니저
6. uber/uReplicator
   - 카푸카 클러스터간 토픽 복제(전달)
7. Spark stream
   - 다양한 소스(카푸카 포함)로 부터 실시간 데이터 처리

#### Kafka 운영시 도움될 환경변수
`log.retention.hours=72`
  - kafka는 기본적으로 메세지, 이벤트 로그를 7일동안 남기게 되어있는데 클러스터링 환경에 따라 너무 많은 디스크

    용량을 차지할 수 있기에 주말 고려하여 3일이 적합한 옵션
    
`delete.topic.enable=true`
  - kafka는 기본적으로는 topic을 지울 수 없게 되있으며 레퍼런스를 봐도 해당 옵션은 적용되어 있지 않은데 이후 운영*관리 측면에서

    토픽을 삭제할 수 있기에 운영단계에선 적용해야할 옵션

`allow.auto.create.topics=false`
  - kafka는 토픽이 생성되어있지 않을때 기본 설정이 true이기에 토픽을 만들고 메세지를 producer가 발행하게 되는데

    해당 토픽을 사용하지 않음에도 이 옵션때문에 재생성이 된다면 관리가 어려워져서 꼭 필요한 옵션

### Reference
<https://www.youtube.com/watch?v=catN_YhV6To><br>
<https://www.iotsi.org/end-to-end-technology-landscape><br>
<https://www.javatpoint.com/apache-kafka-architecture><br>
