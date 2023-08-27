## Prometheus란?
- SoundCloud에서 시작한 프로젝트로 기존 모니터링 도구의 단점을 보완해 다차원 데이터 모델, 단순성, 확장 가능 데이터 수집 및 강력한

- 쿼리 언어를 가진 Prometheus를 2012년부터 개발했다.
- 처음부터 오픈 소스 였으며 Borgmon에 영감을 받았고 2016년 CNCF에 K8s에 이어 두 번째 프로젝트로 Prometheus를 승인해 1.0v 2.0v등 2018년까지 개발을 진행했다.
- 메트릭 수집, 시각화, 알림, 서비스 디스커버리 기능을 모두 가지고 있는 CNCF에 속한 오픈소스 모니터링 툴이다.
- 시계열 데이터 수집하고 저장하는 데 사용된다. 이 값은 레이블이라는 Key-Value 쌍으로 타임스탬프와 함께 저장되어 시계열 데이터가 형상화된다.

### 특징
- 메트릭 이름과 Key-Value 쌍으로 식별되는 시계열 데이터가 있는 다차원 데이터 모델
- PromQL, 이차 원성을 활용하는 유연한 쿼리 언어
- 시계열 수집은 HTTP를 통한 풀 모델을 통해 발생
  - 기본적으로 데이터를 받아오지만 BatchJob과 같은 데이터는 주기적으로 데이터를 받아오는 것이 불가능하다.
  - 하여 대안으로 Pushgateway로 메트릭을 Push하고 프로메테우스는 이 Pushgateway에 접근하여 쌓여있는 메트릭을 Pull하는 방식으로 데이터를 얻을 수 있다. 
- 다양한 클라이언트 라이브러리들
- AlertManager를 통한 쉽고 정확한 알림

### Architecture
![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/e9e301f1-e285-4b68-a8dd-4e2742331056)

- 프로메테우스의 구조는 상단과 같이 구성하는걸 목표로 두고 있으며 서비스 , 모니터링 목적에 따라 적절하게 연동하여 구성한다.

`Jobs/exporters`
- `데이터`를 밖으로 내보내는 컴포넌트이며 Prometheus가 데이터를 pull할 수 있도록 매트릭을 노출하는 agent이다.
- 프로메테우스는 exporter가 제공하는 endpoint로 GET요청을 날려 형식화된 매트릭 데이터를 수집하고 저장한다.
  - Spring에선 Actuator가 exporter의 역할을 한다.
 
`Service Discovery`
- Netflix Eureka나 K8s에서 제공하는 기능 같은 Service Discovery와 통합하여 매트릭을 수집할 대상을 동적으로 설정할 수 있다.
  - MSA환경에선 서비스가 자주 내려갔다 구동될 수 있기 때문에 인스턴스의 endpoint를 갖고 있는 service Discovery를 연동하는 것이
 
    모니터링 구축을 유연하게 만들 수 있다.

`TSDB`
- Time-series Database의 약자이며 수집한 시계열 데이터를 저장하는 역할을 한다.
- 프로메테우스서버의 메모리, Disk에 저장되며 초기엔 Scale Out을 고려하지 않아 확장에 제한이 있었지만 최근엔 `Thanos`같은 클러스터링을

  지원하는 오픈 소스등과 연계하여 운영간 확장의 어려움을 보완할 수 있다.

`AlertManager`
- Prometheus에서 경고가 트리거 되기 위한 조건을 지정할 수 있으며 지정되면 Alertmanager로 전송되고 이후 이메일, slack, PagerDuty와 같은 알림 서비스로 전달할 수 있다.
- Alartmanager Webhook Receiver을 이용해 다른 외부 메시징 시스템과 통합하여 사용이 가능하다.
  - 매트릭을 수집하며 임계치를 넘는 것에 대한 규칙을 설정할 시 연동한 slack으로 알림이 가는 이치라고 이해하면 된다.
 
`PromQL & Visualization`
- 사용자가 데이터를 선택하고 집계할 수 있는 자체 쿼리 언어 `PromQL(Prometheus Query Language)`를 제공한다.
- `TSDB`와의 규칙에 따라 작동하도록 특별히 조정되어 시간 관련 쿼리 기능을 제공한다.
  - rate()함수, 각 시계열에 대한 많은 샘플을 제공할 수 있는 순간 벡터 및 범위 벡터등
- PromQL엔 쿼리 명령으로 실행하는 규칙 중 중점적으로 수집하는 4가지 측정 유형이 존재한다.
  - Gauge
  - Counter
  - Histogram
  - Summary  
- 프로메테우스의 웹 콘솔이나 외부 API와 연동하여 시각화 할 수 있는데 이때 가장 많이 사용되는 것이 `Grafana`이다.

### Metric이란?
- Prometheus에서 저장되는 시계열 데이터를 모두 `매트릭`이라고 표현하는데 그 매트릭이란 **타임스탬프와 한 두가지 숫자값을 포함하는 이벤트로 볼 수 있다.**

### Promtheus를 모니터링 도구로 채택시 고려할 점
- 순수 숫자 시계열(매트릭)을 기록하는 데 적합해 기계 중심 모니터링, 동적인 서비스 지향 아키텍처의 모니터링 모두에 적합하다.
  - 클라우드 환경 , 클러스터링 환경에서 k8s와 통합해 클러스터의 상태, 인스턴스들의 상태를 측정하는데 용이하며 연동하는 것이 쉽다.
  - MSA에서 다차원 데이터 수집 및 쿼리에 대한 지원이 특히 강점이다.  
- 각 Promtheus 서버는 network storage나 기타 외부 API등에 의존하지 않고 독립적이므로 외부 인프라의 장애에도 정상적으로 모니터링이 가능하다.
  - 이로 인해 Prometheus는 신뢰성 있게 시스템에 관한 오류 통계를 확인할 수 있으며 광범위한 인프라를 설정할 필요가 없다.
  
### 사용간 주의할 점
- 프로메테우스는 신뢰성을 중요시하여 서버의 장애 상황에도 시스템에 관한 오류 통계를 항상 확인할 수 있지만 이 데이터가 자세하고 완전하다는 보장은 불가능하다.
  - 요청 당 청구와 같이 100 % 정확성이 필요한 경우 Prometheus는 좋은 선택이 아니므로 다른 모니터링 도구를 사용하는 것이 좋다.
  
### Reference
<https://prometheus.io/docs/introduction/overview/><br>
<https://joon2974.tistory.com/29><br>
<https://velog.io/@ckstn0777/prometheus%ED%94%84%EB%A1%9C%EB%A9%94%ED%85%8C%EC%9A%B0%EC%8A%A4%EB%9E%80><br>
