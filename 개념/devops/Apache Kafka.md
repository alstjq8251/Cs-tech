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

### Reference
<https://www.youtube.com/watch?v=catN_YhV6To><br>
<https://www.iotsi.org/end-to-end-technology-landscape><br>
<https://www.javatpoint.com/apache-kafka-architecture><br>
