## CRI-O
`정의`
  - 레드헷에서 주도적으로 개발한 오픈소스 런타임
  - Docker보다 가볍고 최적화 된 Runtime(특히 Kubernetes)
  - Kubernetes 1.20 이상 버전 부터는 CRI를 지원하는 런타임으로 CRI-O사용
  - CRI-O는 기존 도커 이미지와 호환성이 뛰어나고 사용 경험도 그대로 적용가능
  - CRI-O는 Kubernetes환경에서만 사용이 가능함 (Kubernetes없는 환경에서는 사용 불가)

### CRI-O 데몬 상태 확인 및 이슈 로깅 명령어
`CRI-O 데몬 상태 명령어`
  - systemctl status crio
  - journactl -r -u crio

`CRI-O 정보 확인 명령어` (kubernetes 환경에서만 사용 가능)
  - crictl crio

`특정 CRI-O 컨테이너 상세 정보 명령어` (kubernetes 환경에서만 사용 가능)
  - crictl inspect <컨테이너 ID>

`특정 CRI-O 컨테이너 이슈 로깅 명령어` (kubernetes 환경에서만 사용 가능)
  - crictl logs <실행중인 컨테이너 ID>

#### CRI-O 설치법


### Reference
<https://fastcampus.co.kr/courses/208963/clips/>
