### Kubernetes란?
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
        - Omega에서 k8s로 발전되며 해당 특징은 이어받은채 개발되었기 때문에 자세히 알아둘 필요가 있는데 k8s에선 `resourceVersion`이라 불리며
     
          etcd에 저장된 리소스의 상태를 변경하는 등 작업을 할때 조회하는 순간 버전이 v1등으로 부여되며 변경된 이후 자신이 조회한 버전과 현재 etcd내의

          리소스 버전이 맞지 않게되면 그 요청을 거부하는 방식을 말한다. 

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

`클러스터 구성요소`
- 마스터 노드 Control Plane
	- 클러스터 상태를 저장하고 관리 - 그런 구성요소들이 들어있음
	- etcd(key-value data store)
		클러스터에 배포된 애플리케이션 실행 정보를 저장
	- API Server
	- Scheduler - 노드를 선택하기 위한 노드 스케줄링
	-  Controller Managers - 사용자가 선택한 컨테이너 개수가 맞는지 아닌지 검사하고 API Server에게 요청
- Worker 노드
	- 컨테이너 실행을 담당
	- kubelet, Container Runtime(Docker, CRIO)
	- Kube-proxy

#### Kubernetes Cluster 형태

`단일 클러스터`
- 단일 클러스터는 마스터 노드1 + 워커 노드 N 개의 형태를 유지한 클러스터라고 할 수 있다.
  
`HA 클러스터`
- HA 클러스터는 고가용성 클러스터로써 마스터 노드N + 워커 노드N 개로 구성된 클러스터의 형태라고 할 수 있다.
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
`정의`
- 쿠버네티스에서 구동되는 어플리케이션의 집합을 의미한다.
- 노드에서 컨테이너를 실행하기 위한 가장 **기본적인 배포 단위**
- 여러 노드에 **1개 이상의 Pod을 분산 배포/실행 가능(Pod Replicas)**

`특징`
- 쿠버네티스는 **Pod을 생성할 때** 노드에서 **유일한 IP를 할당**(서버 분리 효과)
- **Pod 내부 컨테이너에서 localhost로 통신** 가능, **포트 충돌 주의**
- Pod안에서 네트워크와 볼륨 등 자원 공유

`주의할 점`
- **Pod IP는 클러스터 내에서만 접근할 수 있다.**
- 클러스터 외부 트래픽을 받기 위해선 Service 혹은 Ingress 오브젝트 필요

`Pod 및 컨테이너 설정 시 고려할 점`
1. 컨테이너들의 라이프사이클이 같은가?
   - Pod 라이프사이클 = 컨테이너 라이프 사이클 , 즉 강결합, 약결합인지를 판단해 동일 파드내의 다른 역할의 컨테이너를 묶어서 배포
   - A는 앱 B는 로그 수집기라면 A가 다운됐을때 B의 실행의미가 사라짐
2. 스케일링 요구사항이 같은가?
   - 웹 서버 vs DB 서버, 트래픽이 많은가 , 그렇지 않은가
   - 트래픽을 많이받는 서버의 경우 스케일아웃이 자유롭지만 db는 그렇지 않아 같은 파드내로 묶기 애매한경우
3. 인프라 활용도가 더 높아지는 방향으로
   - 쿠버네티스가 노드 리소스 등 여러 상태를 고려하여 Pod을 스케줄링
   - k8s는 각 노드의 리소스를 예측해 파드를 배포하는데 파드의 크기가 크면 클수록 적절하게 배포는것이 어려워지므로 파드의 크기를 적절하게 만들어야함
   - 여러개의 파드로 분리하는것이 쿠버네티스가 리소스를 활용하고 자원을 활용하는데 도움을줌

`노드에 배포되는 과정`
1. 사용자로부터 Pod 배포 요청을 수락한다.
2. 요청 받은 수만큼 Pod Replica를 생성한다.(Pod desired state = current state)
3. Pod을 배포할 적절한 노드를 선택한다.(Node Selector)
4. 5에게 이미지 다운로드를 명령하고 Pod 실행을 준비한다. Pod 상태를 업데이트한다.
5. 이미지를 다운로드하고 컨테이너를 실행한다.

`한계점`
- Pod가 나도 모르게 종료된경우
  - 자가 치유 능력(Self-Healing)이 없음, Pod이나 노드 이상으로 종료되면 끝
  - **"사용자가 선언한 만큼 POD을 유지"** 해주는 ReplicaSet 오브젝트 도입
 
- Pod IP는 외부에서 접근할 수 없다, 그리고 생성될 때마다 IP가 변경된다.
- 클러스터 **"외부에서 접근"할 수 있는 "고정적인 단일 엔드포인트"** 필요
- **Pod집합**을 **클러스터 외부로 노출**하기 위한 Service 오브젝트 도입 

```
POD IP는 클러스터 외부에서 접근이 불가하고 클러스터 내부에서만 접근 가능한 IP를 부여
이 IP는 컨테이너와 공유하기 때문에 파드에 정의된 컨테이너가 여러개라면 포트 충돌을 주의해야함
하나의 POD에 속한다면 localhost로 통신가능하고 다른 파드간의 통신은 IP를 이용
```

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
    - volumes : 파드레벨에서의 볼륨을 설정
    - volumeMounts : 컨테이너 레벨에서의 볼륨을 설정
      
- `spec`
  - 사용자가 원하는 오브젝트 상태
  - 선언할 수 있는 속성은 오브젝트 마다 다름

```yaml
apiVersion: apps/v1   # 쿠버네티스 API 버전
kind: Deployment      # 오브젝트 타입
metadata:             # 오브젝트를 유일하게 식별하기 위한 정보
   name: nginx-deployment  # 오브젝트 이름
   labels:  nginx              # 오브젝트 집합을 구할 때 사용하는 이름표 
spec:                 # 사용자가 원하는 오브젝트의 바람직한 상태
   nodeSelector:       # Pod을 배포할 노드
   selector:
      matchLables:
         app: nginx
      replicas: 2
      template:
         metadata:
            lables:
               app: nginx
         spec:
            containers:    # Pod안에서 실행할 컨테이너 목록 
               - name: nginx
                 image: nginx:1.14.2
                 ports:
                    - containerPort: 80
                 volumes:    # 컨테이너가 사용할 수 있는 볼륨 목록
```

`쿠버네티스의 상태를 알려주는 Status 필드`

<img width="1145" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/80c52b0f-56e8-4204-81f9-9406c93a0365">

**Spec이란 쿠버네티스가 달성해야할 목표이며 Status는 오브젝트의 현재 상태를 의미한다.**
   - 쿠버네티스의 API Server가 Spec을 읽어 오브젝트를 생성하고 Status와 맞는지 지속적으로 비교한다.
   - 이 때 쿠버네티스의 Controller Manager가 Spec과 Status를 비교하면서 계속 조정하고 상태를 업데이트 한다.

#### ReplicaSet
`정의`
- 레플리카 파드 집합의 실행을 항상 안정적으로 유지하는 것이다.

`작동방식`

`필요성`
- 레플리카 오브젝트는 **내결함성(fault tolerance)**를 위해 존재한다.
   - Pod에 문제가 생겼을 시 복구되는동안 서비스에 장애가 생길 수 있는데 이 때 Replicaset오브젝트로 파드를 관리해

     필요한 Pod의 개수를 선언하고 해당 수만큼 실행을 보장하여 사람의 개입없이 정상적인 기능을 수행할 수 있게 하기 위해서 사용한다.

`오브젝트 생성 예시`
``` yaml
apiVersion: apps/v1 #k8s api 버전

kind: Replicaset #오브젝트 타입

metadata: #오브젝트를 유일하게 식별하기 위한 정보
	name: blue-app-rs #오브젝트 이름
	lables: # 오브젝트 집합을 구할 때 사용하는 이름표
		app:blue-app 

spec: # 사용자가 원하는 Pod의 바람직한 상태
	selector: #Replicaset이 관리하기 위한 Pod을 선택하기 위한 label query
	replicas: # 실행하고자 하는 Pod 복제본 개수 선언
	template: # Pod 실행 정보 - Pod template과 동일
```

#### Deployment
`정의`
- Pod배포를 자동화한 오브젝트 (ReplicaSet + 배포전략)
    - 새로운 Pod를 롤아웃/롤백할때 Replicaset 생성을 자동화해준다(Pod 복제)
    - 다양한 배포 전략을 자동화하고 이전 파드에서 새로운 파드로의 전환 속도를 제어할 수 있다
    - 이제는 Pod를 배포할 때 ReplicaSet이 아니라 Deployment를 이용한다.

`배포전략`
1. recreate
- 이전에 배포되어있는 Pod들을 순차적으로 하나씩 지우면서 롤아웃을 시작하고 모든 Pod가 다 지워졌으면 새로운 ReplicaSet과 Pod집합을 생성시키며 롤아웃을 마무리한다.
- Pod가 존재하지 않는 구간이 존재해 Service DownTime이 존재한다.
    - 개발 단계에선 유용하다 ( RollingUpdate 배포 전략은 이전 버전, 새 버전이 같이 배포되어 있어 트래픽이 이전 버전의 파드로도 흘러갈 수 있기 때문) 
2. RollingUpdate
- 새로운 Pod 생성과 이전 Pod 종료가 동시에 일어나는 방식  
- 이전 Pod가 모두 지워질때까지 점진적으로 Pod가 존재하지 않는 구간이 없어 Service DownTime이 존재하지 않지만 이전 버전과

  새로운 버전이 모두 존재하여 이전 버전의 요청이 들어온다면 이전 Pod가 해당 트래픽을 받게되고 새로운 Pod가 다른 트래픽을 받는 등 혼동될 수 있다.
- 점진적으로 배포하는 속도를 제어할 수 있는데 해당 옵션은 하단의 2가지와 같다.
    1. `maxUnavailable` - 최대 몇개까지 즉시 종료할것인가를 선언하는 옵션
    2. `maxSurge` - 최대 몇개까지 즉시 배포할 것인가를 선언하는 옵션

#### Service
`Pod의 한계로 인한 Service의 탄생`
1. 파드 스케줄링에 따른 잦은 Pod IP 변경 -> 변경된 파드 IP 목록에 영향받는 파드 클라이언트
2. Pod IP는 클러스터 내부에서만 접근 가능 -> 클러스터 외부에서 접근할 단일 EndPoint의 필요
   - kubectl port-forward 프로세스로 파드에 트래픽을 라우팅 할 수 있지만 이것은 개발에서만 사용하는 것이고
     Cluster외부에서 Pod로 접근할 방법이 존재하지 않음

`정의 및 역할`
- 파드 추상화 및 로드밸런싱
  - 파드들의 단일 엔드포인트를 제공한다.
  - Service는 Selector에 의해 선택된 파드 집합 중 임의의 파드로 트래픽을 전달한다(로드밸런싱).
- `Service Discovery`

#### StatefulSet
`역할`
- 파드의 ID와 파드의 볼륨을 유지한다.
- 컨테이너 애플리케이션의 상태를 관리하는 데 사용하는 컨트롤러


#### Ingress
`탄생 배경`

`역할`
- L7 단에서 작동하며 헤더, 호스트, url 등으로 구분하여 라우팅해주는 역할을 담당하며 Ingress는 해당 라우팅하는 규칙들의 모임일뿐이며 단일로는 라우팅을 수행하지 못한다.

#### LivenessProbe & ReadinessProbe
`LivenessProbe`
- 생사, 수사를 합친 의미로서 파드 내 컨테이너의 생사를 확인하기 위한 방법이다.
- warkerNode에서 돌고있는 kubelet이 주기적으로 LivenessProbe에서 제공한 Endpoint에서 적절한 응답을 받지 못했을 때 자체적으로 컨테이너를 재시작하는 매커니즘
- k8s가 제공하는 self-healing과 재시작 매커니즘
- 쿠버네티스가 컨테이너 상태를 확인할 수 있도록 Pod livenessProbe를 선언하는 방법이다.
   - Pod livenessProbe를 선언하게 되면 kubelet은 제공된 이 EndPoint로 상태를 체크할 수 있게 된다.


#### ConfigMap
`역할`
- 설정 정보, 환경 설정에 들어가는 등의 데이터를 저장하고 있는 오브젝트다.

`사용 이유`


#### Secret

#### Metrics Api

### K8s 배포 파일들

#### k8s 설치시 권장 사양 및 권장 설정
`centOs ver 7`
- setenforce 0
- sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
    - k8s에서 SELINUX 비활성화 하는 것으로 SELINUX가 권한을 제어하여 k8s가 호스트 파일 접근 제어를 할 수 있기 때문에 비활성화(권장)
- `swapoff -a`
- `echo 0 > /proc/sys/vm/swappiness`
- `sed -e '/swap/ s/^#*/#/' -i /etc/fstab/`
- `vi /etc/fstab`  # SWAP이 정의된 줄을 '#'으로 주석처리해준다.
    - Kubernetes에서는 kubelet이 제대로 동작하게 하려면, 반드시 swap을 사용하지 않게 하라고 권장함
- CNI(Container Network Inteface)설정

- CRI-O 설치시 버전
    - k8s와 cri-o는 버전을 동일하게 맞춰서 설치해야 적용이 가능하다.   

#### CNI
`CNI(Container Network Interface)`란? 
- 컨테이너가 생성되거나 소멸될때 컨테이너 네트워킹을 쉽게 구성할 수 있도록 설계된 표준이다.
- 쿠버네티스에서는 Pod 간의 통신을 위해서 CNI 를 사용한다.
- 쿠버네티스는 기본적으로 'kubenet' 이라는 자체적인 CNI 플러그인을 제공하지만 네트워크 기능이 매우 제한적인 단점이 있다.
    - 그 단점을 보완하기 위해, 3rd-party 플러그인을 사용하는데 그 종류에는 Flannel, Calico, Weavenet, NSX 등 다양한 종류의 3rd-party CNI 플러그인들이 존재한다.

##### k8s 설치시 master 노드와 worker노드의 역할을 겸하게 할땐 하단의 명령어를 입력한다.
- `kubectl taint node {master-node name} node.role.kubernetes.io/control-plane:NoSchedule-`


 
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

