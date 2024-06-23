## ServiceEntry란?
- Istio를 사용할 때 서비스 매시 내의 서비스가 외부 서비스와 상호 작용하는 방식을 관리할 수 있는 필수 구성 객체이다.
- istio가 유지 관리하는 서비스 레지스트리에 추가 항목을 추가하고 이를 통해 외부 서비스에 대해 알고 메시의 일부인 것 처럼 처리할 수 있다.
- 단일 서비스에서 포드와 VM을 모두 선택하는 기능을 사용하면 서비스와 연결된 기존 DNS 이름을 변경하지 않고도 VM에서 Kubernetes로 서비스를 마이그레이션할 수 있다.

### Reference
<https://istio.io/latest/docs/reference/config/networking/service-entry/>