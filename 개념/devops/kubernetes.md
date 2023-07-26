## Kubernetes란?
`정의`
- 쿠버네티스란 **여러 개의** 컨테이너화된 애플리케이션을 **여러 서버(클러스터)에** 자동으로 배포, 스케일링 및 관리해주는 오픈소스 시스템이다.

## Kubernetes가 나오게 된 배경
- `도커`와 같은 컨테이너 런타임 환경으로 컨테이너들을 배포하는 환경에서 서버 및 서비스가 1~2개에서 N개로 늘어나게 됨(MSA)
- 이 때 컨테이너들의 생명주기 및 컨테이너들이 내렸다 올라가는 과정에서 바뀌는 endpoint를 관리할 `Service Discovery`가 필요
- 어플리케이션을 배포하는 것에 있어 자원을 최적화 시키는 필요성(Pod) 및 인프라 구축에 신경쓰는 것을 덜하기 위해서
    - MSA에서 구동되는 수많은 어플리케이션의 조합이 힘들뿐더러 충분한 자원의 배치 -> 자원 낭비 , 비용 발생으로 이어지기 때문
- 서비스의 장애로 재구동해야할때 서비스를 찾고 서비스를 접속하여 재구동 -> 복구시간이 오래걸림

### Kubernetes가 제공하는 것
1. `자동화된 빈 패킹(bin packing)`
    - 컨테이너화된 작업을 실행하기 위한 쿠버네티스 클러스터 노드를 제공한다.
    - 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)을 쿠버네티스에게 지시한다.
    - 쿠버네티스는 컨테이너를 노드에 맞춰서 리소스를 가장 잘 사용하도록 해준다.

2. `자동화된 복구(self-healing)`
    - 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, `사용자 정의 상태 검사`에 응답하지 않는 컨테이너를 죽이고

      서비스 준비가 끝날 때까지 그러한 과정을 클라이언트에 보여주지 않는다.

3. `자동화된 롤아웃과 롤백`
    - 쿠버네티스를 사용하여 배포한 컨테이너의 원하는 상태를 서술할 수 있으며, 현재 상태를 원하는 상태로 설정한 속도에 따라 배포할 수 있다.
        - 쿠버네티스를 자동화하여 새 컨테이너 만들고, 기존 컨테이너 제거한 뒤 기존 리소스를 새 컨테이너에 모두 할당할 수 있다.

### Kubernetes까지의 컨테이너 관리 시스템 발전

1. <details>
    <summary><strong>Borg.7</strong></summary>
    <div>

    - Google에서 개발한 최초의 컨테이너 관리 시스템
    - 자원 요구사항 예측하여 리소스 활용도를 높이고 비용을 줄이는 방법을 제공
    - 그 외 Configuration 파일을 실행 중인 서비스에 동적으로 반영,
    - 서비스 디스커버리, 로드밸런싱, 자동 크기 조정 등
    
    </div>
    </details>

2. <details>
    <summary><strong>Omega</strong></summary>
    <div>

    - 클러스터 상태의 일관성을 부여하기 위해 `클러스터 상태를 저장 기능을 추가` - 영구 저장소
    - 낙관적 동시성 제어(optimistic concurrency control) 방법을 이용해서 `리소스 충돌을 해결`

    </div>
    </details>

3. <details>
    <summary><strong>Kubernetes</strong></summary>
    <div>

    - Borg, Omega와 달리 오픈소스
    - 구글 퍼블릭 클라우드 인프라 사업을 성장시키기 위해 설계하고 개발함
    - Omega처럼 리소스 변경 사항을 저장하기 위한 공유 영구 저장소가 있지만, Omega는 신뢰할 수 있는 구성요소들이 

      직접 접근하는 것에 반해 k8s는 **더 높은 수준의 추상화를위해 Rest Api를 통해서만 접근할 수 있도록** 바뀌었다.
    - k8s는 쿠버네티스가 클러스터에서 실행되는 어플리케이션을 개발하는 개발자의 경험에 초점을 맞추어 개발되었다.
    - 주요 설계 목표는 **컨테이너로 향상된 리소스 활용의 이점을 누리면서도 복잡한 분산 시스템을 쉽게 배포하고 관리할 수 있게 만드는** 것이다.

  </div>
  </details>

### Kubernetes Cluster
`정의`
- 클러스터란 여러 개의 서버를 하나로 묶은 집합, 하나의 서버처럼 동작한다.
- 쿠버네티스 클러스터란 애플리케이션 컨테이너를 배포하기 위한 서버 집합을 의미한다.

### Kubernetes Cluster 구성

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/6fa9e838-b3a6-453f-85c6-d0262e5dced6)

`Master-Node의 Control Plane`
- 클러스터의 상태를 저장하고 관리
  1. etcd(key-value data store)
    - 클러스터에 배포된 애플리케이션의 상태를 저장
  2. API server
    - 클러스터 상태 조회, 저장을 위한 API 제공
  3. Scheduler
  4. Controller Manager

`Warker Node`
- 컨테이너 실행을 담당
- kubelet, Container Runtime(Docker, Containerd..)
- Kube-proxy

#### Kubernetes에 어플리케이션 컨테이너를 배포하는 것이란?
`의미`
- 쿠버네티스 오프젝트 Manifest 파일을 작성해서 마스터 노드에 있는 API Server에 요청을 보내는 행위
-`Manifest`란 쿠버네티스 오브젝트를 생성하기 위한 필수 정보 즉 작업을 시키기 위한 지시서의 역할

<img width="1156" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/93a9fe83-642f-4428-b7b6-84beb6194f18">

### Kubernetes Pods

### Kubernetes Object
`정의`
- 쿠버네티스 클러스터를 이용해서 애플리케이션을 배포하고 운영하기 위해 필요한 모든 쿠버네티스 리소스

`쿠버네티스 오브젝트가 될 수 있는 것(클러스터의 상태를 표현하는 개체들)`
- 어떤 애플리케이션을 - POD
- 얼마나 - ReplicaSet
- 어디에 - Node, Namespace
- 어떤 방식으로 배포할 것인가 - Deployment
- 트래픽을 어떻게 로드밸런싱할 것인가 - Service, Endpoint

`쿠버네티스 Manifest파일`
- 오브젝트 필수 정보
- `apiVersion`
  - 오브젝트를 생성할 때 사용하는 API 버전
- `kind`
  - 생성하고자 하는 오브젝트 종류
- `metadata`
  - 오브젝트를 구분지을 수 있는 정보
    - `name`, `resourceVersion`, `labels`, `namespace`
- `spec`
  - 사용자가 원하는 오브젝트 상태
  - 선언할 수 있는 속성은 오브젝트 마다 다름

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
   name: nginx-deployment
spec:
   selector:
      matchLables:
         app: nginx
      replicas: 2
      template:
         metadata:
            lables:
               app: nginx
         spec:
            containers:
               - name: nginx
                 image: nginx:1.14.2
                 ports:
                    - containerPort: 80
```

`쿠버네티스의 상태를 알려주는 Status 필드`

<img width="1145" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/80c52b0f-56e8-4204-81f9-9406c93a0365">

**Spec이란 쿠버네티스가 달성해야할 목표이며 Status는 오브젝트의 현재 상태를 의미한다.**
   - 쿠버네티스의 API Server가 Spec을 읽어 오브젝트를 생성하고 Status와 맞는지 지속적으로 비교한다.
   - 이 때 쿠버네티스의 Controller Manager가 Spec과 Status를 비교하면서 계속 조정하고 상태를 업데이트 한다.

#### Kubernetes 명령어(Kubectl)

`kubectl api-resources`
   - 쿠버네티스 클러스터에서 사용할 수 있는 오브젝트 목록 조회

`kubectl explain <type>`
   - 쿠버네티스 오브젝트의 설명과 1레벨 속성들의 설명
   - apiVerson, kind, metadata, spec, status

`kubectl explain <type>,<fieldName>[.<fieldName>]`
   - kubectl explain pods.spec.containers
   - 쿠버네티스 오브젝트 속성들의 구체적인 설명(Json 경로)

`kubectl get Nodes`
   - 쿠버네티스 클러스터에서 속한 노드 목록 조회

`kubectl apply -f <object-file-name>`
   - kubectl apply -f deployment.yaml
   - 쿠버네티스 오브젝트 생성/변경

`kubectl get pods`
   - 실행 중인 Pod(컨테이너) 목록 조회

> 배포한 컨테이너들을 운영할 때 사용할 수 있는 명령어

`kubectl scale -f <object-file-name> --replicas=#`
   - kubectl scale -f deployment.yaml --replicas=3
   - 애플리케이션 배포 개수를 조정(replicas:복제본)

`kubectl diff -f <object-file-name>`
   - kubectl diff -f depolyment.yaml
   - 현재 실행중인 오브젝트 설정과 입력한 파일의 차이점 분석

`kubectl edit <type>/<name>`
   - kubectl edit depolyment/nginx-depolyment:replicas를 4로 변경
   - 쿠버네티스 오브젝트의 Spec을 editor로 편집

`kubectl port-foward <type>/<name> <localport>:<container-port>`
   - kubectl port-forward pod/nginx-depolyment-874-fc 8080:80
   - 로컬 포트는 실행중인 컨테이너 포트로 포워딩

`kubectl attach <type>/<name> -c <container-name>`
   - kubectl attack depolyment/nginx-depolyment -c nginx
   - 현재 실행중인 컨테이너 프로세스에 접속하여 로그 확인

`kubectl logs <type>/<name> -c <container-name> -f`
   - kubectl logs depolyment/nginx-depolyment -c nginx -f
   - 현재 실행중인 컨테이너 프로세스의 모든 로그 출력 (-f:watch 모드)

