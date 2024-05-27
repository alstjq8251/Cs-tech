## Ingress
**Ingress란 Service를 추상화하고 의미있는 단일 엔드포인트를 제공하는 오브젝트**
- 트래픽을 Service로 분산하기 위한 라우팅 규칙 모음이다.
- 클라이언트가 호출한 Host헤더나 Path를 통해 Service를 구분하고 트래픽을 포워딩한다.
  - Ingress는 라우팅을 하기 위한 규칙을 정의하는 것이며 Ingress가 트래픽을 포워딩하는 주체는 아니다.
    Ingress Controller : Ingress 규칙에 따라 트래픽 분산을 실행하기 위한 리소스
- 쿠버네티스 클러스터 제공자가 구현한 Ingress Controller마다 기능이 다르다.
- 쿠버네티스 지원 Ingress Controller https://bit.ly/3GkpoZq
  - Ingress리소스를 생성하게 되면 GKE에선 Google Cloud Load Balancer를 Ingress Controller를 생성한다.

### Ingress와 Ingress Controller의 필요성
- k8s cluster엔 수많은 파드와 파드들로 요청을 포워딩 하기 위한 수많은 서비스가 존재한다.
  - 서비스 만큼의 Loadbalancer가 생기게 되고 서비스마다의 IP가 생기고 클라이언트는 해당 개수만큼의 IP를 통신하기 위해 기억해야 한다.
    - 이러한 IP를 기억할 필요 없이 어떻게 하면 단일 엔드포인트로 제공할 수 있을까 하다보니 Ingress나 Mesh 서비스를 이용하게 된다.

> Ingress를 통해 여러 Service의 단일 IP를 생성할 수 있다.