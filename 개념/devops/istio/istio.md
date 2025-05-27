### istio란?
- 애플리케이션 네트워크 기능을 유연하고 쉽게 자동화할 수 있는 투명한 언어 독립적 방법을 제공하는 현대화된 서비스 네트워킹 레이어인 서비스 메시다.
- MicroService Architecture의 분산 네트워크 환경(Kubernetes)에서 각 app들의 네트워크 연결을 쉽게 설정할 수 있도록 지원하는 기술이다.

### 동작방식
- istio는 동작하는 파드에 data plain으로 작동하는 Envoy Proxy 컨테이너를 삽입하는 방식으로 보안, 관리, 모니터링 기능을 추가한다.

### 오브젝트

### install

1. istioctl
2. operator
3. helm

### Sidecar 

#### cni
- istio를 sidecar모드로 배포하게 되면 각 파드의 envoy proxy를 주입하기전 Init container로 iptables규칙을 수정하여 envoy 파드로 오게끔 하는 절차가 추가되게 된다.

### Ambient
- sidecar 모드와 다르게 pod에 proxy 컨테이너를 주입하지 않고 외부의 proxy 컨테이너를 배포하고 그 컨테이너를 통해 통신하는 방식

