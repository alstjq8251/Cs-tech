## Service
- 외부 DNS 혹은 내부 label을 가진 파드 그룹으로 트래픽을 라우팅하는 오브젝트
- 파드들의 단일 엔드포인트 + 로드밸런싱 = 파드 추상화
- Selector에 의해 선택된 파드 집합 중 임의의 파드로 트래픽을 전달한다.

> Service가 관리하고 있는 파드의 endpoint들을 Service Object가 관리한다고 볼 수 있다. -> Service discovery 

### Service가 나오게 된 배경
1. 운영환경에서의 POD 한계점
    - 변경이 잦은 POD IP 목록에 영향 받는 클라이언트
      - 파드 클라이언트가 최신 상태의 모든 POD IP를 알고 있어야 한다.
      - 클라이언트가 특정 POD IP로 오프라인 상태의 POD에 접근한다면 요청은 실패한다.
2. POD IP는 클러스터 내부에서만 접근할 수 있다.
    - 클러스터 외부에서 접근할 수 있는 방법이 필요하다.
    - kubectl port-forward 프로세스는 개발 단계에서만 사용해야 한다.

### Service 오브젝트가 관리하는 Endpoint 오브젝트
`Service가 관리하고자 하고 노출하는 POD IP와 Port의 최신 목록을 관리하는 오브젝트`

- Service 오브젝트를 생성하면 자동으로 Endpoints라는 오브젝트를 생성한다.
- Service에 선언한 Selector의 파드 집합이 변경될 때마다 Endpoints 목록도 업데이트 된다.
- Service가 받은 트래픽을 Endpoints 중에 하나로 리다이렉트 한다.
  - Service Registry역할을 하게 되며 Service Registry엔 Service오브젝트가 전달할 파드들에 대한 주소 목록이 적혀 있따.
    - 파드 스케줄링에 따른 IP의 변경에 따라 Endpoints의 addresses목록이 변경된다.
- Service를 통해 POD IP의 변경에 상관없이 클라이언트가 파드로 통신할 수 있게 되고 POD집합에 대한 주소 목록을 Service를 통해 추상화 할 수 있다.

`Endpoint 오브젝트 생성시 갖게되는 상태`
```yaml
EndPoints : name
addresses
	- ip: 10.76.0.168
	- ip: 10.76.0.169
	- ip: 10.76.0.170
ports:
	- port: 8080
	- port: 443
```

### 클러스터 내 파드 간 엔드포인트 Service와 통신하는 방법
1. 컨테이너 환경변수에 설정된 Service IP와 Port를 이용하는 방법
    - 쿠버네티스가 POD를 생성할 때 컨테이너 환경변수에 모든 Service IP와 Port를 추가함
      - 000_SERVICE_HOST, 000_SERVICE_PORT
      - 주의1. Service를 클라이언트 Pod보다 먼저 생성해야 한다. 
        - POD생성시 이미 생성되어 있는 Service목록을 환경변수로 등록하기 때문에 Service가 이후에 생성된다면 환경변수에 없기 때문이다.
      - 주의2. 다른 네임스페이스에 있는 Service의 IP와 Port는 환경변수에 등록되지 않는다.
2. Service이름으로 내부 DNS 서버에(coredns) 서버에 질의하여 Service IP를 얻어내는 방법
    - 쿠버네티스가 DNS서버 IP 주소를 컨테이너의 /etc/resolve.conf 파일에 등록한다.
    - Service이름으로 요청을 실행하면(ex, ex.dns.svc.cluster.local) DNS 서버로부터 Service IP을 조회할 수 있다.

Pod 안에서는 Service 이름과 네임스페이스 이름을 이용해 다른 Pod와 통신할 수 있다.
- 같은 네임스페이스에 잇는 Pod: <Service-name>:<Service-port>
- 다른 네임스페이스 ""       : <Service-name>:<Service-port>.<namespace>


### Service Type

### Headless Service
- 쿠버네티스 서비스 생성 시 .spec.clusterIP 필드 값을 None으로 설정하면 클러스터 IP가 없는 서비스를 만들 수 있고 이걸 Headless Service라 부른다.

`역할`

#### Service Type은 총 3가지로 만들 수 있으며 기능의 포괄도는 1 < 2 < 3 으로 확장된다.

1. `ClusterIP`
    - 기본 Service타입, POD IP처럼 외부에서는 접근할 수 없는 IP를 할당받는다.
    - 이 때 Service IP는 클러스터 내부 통신용으로 사용된다.
      - Service는 Internal IP를 할당받아 클러스터 내부의 다른 POD에선 접근할 수 있지만 External IP가 <none>으로 설정 돼 외부에선 접근이 불가하다.
2. `NodePort`
    - 외부에서 접근할 수 있는 External IP가 아니라 NodePort를 할당받는다.
    - 할당받는 노드 Port를 통해 들어온 트래픽을 파드 집합으로 포워딩한다.
      - ClusterIp와 마찬가지로 External IP는 설정되어있지 않으나 외부의 트래픽을 수신할 수 있는 NodePort를 할당받는다.
      - 외부 트래픽이 수신됐을 때 Service오브젝트가 요청을 수신받아 관리하고 있는 파드 집합으로 트래픽을 포워딩한다.
    - 한계: 노드의 상태에 따라 요청을 수신받을 수 없어 클라이언트가 영향을 받게 된다.
      - LoadBalancer를 두어 건강한 노드를 체크해서 트래픽을 포워딩해야 한다.
3. `LoadBalancer`
    - 클라우드 서비스의 Load Balancer를 프로비저닝하고 External IP를 할당 받는다.
    - 클라이언트는 Load Balancer IP를 통해 특정 서비스로 외부 트래픽을 포워딩할 수 있음
      - LoadBalancer의 External IP를 할당받아 외부에선 해당 IP로 요청하고 내부적으로 NodePort로 Service 오브젝트로 통신하는 방식이다.

> Service 생성 시 type을 LoadBalancer로 생성하게 되면 External IP 할당과 동시에 클라우드 서비스의 로드밸런서가 사용됨

#### Service 타입간 특징
`ClusterIP 특징`
- Service는 파드 집합에 대한 단일 엔드포인트 생성
- Service를 생성하면 ClusterIP가 할당된다.
- ClusterIP는 클러스터 내부에서만 접속 할 수 있다.

`NodePort 특징`
- 클러스터내 모든 노드에 포트 할당은 Service를 NodePort 타입으로 생성 했을 때 일어난다.
- 노드의 External IP와 서비스 NodePort를 이용해서 파드 접근 가능
- 서비스 ClusterIp도 여전히 클러스터 내부에서 사용 가능

`LoadBalancer 특징`
- LoadBalancer를 만들게 되면 클라우드 서비스의 로드밸런서가 실행됨
- 로드밸런서의 IP가 Service의 External IP로 할당된다.
- Service의 External IP이자 로드밸런서 IP로 외부에서 파드에 접근할 수 있다.
- 서비스 ClusterIp, NodePort 기능도 여전히 사용 가능

#### FQDN
쿠버네티스에서 사용하는 도메인 이름 규칙은 FQDN(Fully Quallyfied DOmain Name)을 따른다.
- FQDN: <서비스 이름>.<네임스페이스>.svc.cluster.local
- FQDN을 이용해서 DNS쿼리 실행
- DNS 서버로부터 받은 Service IP를 이용해 요청 완료
  - 1 -> http://pod -> 2(pod:kube-dns)
  - 1 <- 10.80.2.141(Service ClusterIP) 