### Spring Eureka란?
`정의`
  - **Eureka**는 클라우드 환경의 다수의 서비스(예: API 서버)들의 로드 밸런싱 및 장애 조치 목적을 가진 미들웨어서버이다.
    - 로드 밸런싱은 클러스터링 환경에서 트래픽을 분산시켜주는 활동을 말한다.
    - 미들웨어 서버는 데이터를 주고 받는 양쪽의 서비스(웹의 예로 클라이언트와 API 서버)의 중간에 위치해 매개 역할을 하는 소프트웨어다.
  - Eureka는 이러한 미들웨어 기능을 하기 위해 각 연결된 서비스의 IP / PORT /InstanceId를 가지고 있고 REST기반으로 작동한다.
  - Eureka는 Client-Sever 방식으로 Eureka Server에 등록된 서비스는 Eureka Client로 불린다.

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/7b55789d-eebe-4345-8428-679d3d48d237)

`쓰는 이유`
  - MSA 환경에서 내부의 다른 서비스와 소통할 때 다른 서버의 P 또는 FQDN(FQDN-Fully Qualified Domain Name)과 포트를 알아야 한다.
  - k8s환경에선 pod가 증감되는것은 다반사며 이때 IP나 네임스페이스가 바뀌는 경우도 비일비재 한데 각 마이크로서비스가 모든 마이크로서비스의

    IP/FQDN과 PORT정보를 갖고 있는것은 매우 비효율적이기 때문에 각 마이크로서비스의 IP/FQDN과 PORT정보를 저장하고 제공하는 Service Discovery가 필요하다.

      - 외부의 서비스들이 마이크로서비스를 검색하기 위해 사용하는 일종의 전화번호부와 같은 역할로 각각의 마이크로서비스가 어느 위치에 있는지를 등록해 놓은 곳을 의미한다.
      - Spring Cloud 기술 중 Spring Cloud Nefilx Eureka가 그 역할을 한다고 할 수 있다.

> 일반적인 인프라 환경에선 로드밸런서가 트래픽을 받고 Eureka와 같은 Service Discovery가 받아 필요한 서비스의 위치를 Spring Cloud GateWay에게<br>
> 요청을 위임하면 API Gateway는 이에 따라 해당 서비스를 호출하고 결과를 받는 식으로 구성된다.

#### K8s와 Spring Cloud Eureka?
  - k8s환경으로 모두 서비스가 배포되는 환경에선 Kubernetes가 이미 Eureka의 역할을 갖고 있어 각 파드와 매니페스트, 메타데이터 정보를 갖고 있기 때문에

    그 땐 쓰지 않지만 도커런타임과 같은 k8s와 다른 런타임 환경이나 다른 서비스와 통신을 할 땐 범용적으로 쓰일 수 있는 Spring Discovery 즉

    Spring Cloud Eureka가 쓰이게 되고 필요한 이유다.

#### Spring Cloud Eureka Architecture 및 사용법

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/f815c03d-e288-4fa0-b825-c040a241f887)

- 각 마이크로 서비스는 구동과 동시에 Eureka Server에 Regist하는 절차를 제일 먼저 하게 되고 자신의 IP/FQDN과 PORT를 등록한다.
- 이 후 Eureka Server는 주기적으로 각 마이크로서비스의 실행여부를 체크하며 정지된 경우, registry에서 삭제합니다.
- 추 후 서비스끼리 호출할 땐 Eureka 이를 중개해 라우팅하기 때문에 서로의 엔드포인트를 알지 못해도 통신이 가능하다.

`Eureka Registry 동기화`
  - 유레카 클라이언트는 리본을 사용해 클라이언트측 로드 밸런싱을 수행한다.
    - 이를 위해 서비스 레지스트리를 로컬에 캐싱하고 주기적으로 유레카 서버와 통신하면서 이를 업데이트한다.
  - 서비스 레지스트리를 로컬에 캐싱하기 때문에 유레카 서버가 멈추더라도 서비스끼리 통신할 수 있다는 점이 장점이다.


### Reference
<https://happycloud-lee.tistory.com/210><br>
<https://cjw-awdsd.tistory.com/52><br>
<https://javachoi.tistory.com/396><br>

