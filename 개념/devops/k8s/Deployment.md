## Deployment
`Pod배포를 자동화한 오브젝트(ReplicaSet + 배포전략)`

`역할`
- 새로운 POD를 롤아웃/롤백할 때 ReplicaSet 오브젝트 생성을 자동화해준다(Pod 복제 & 복구)
- 다양한 배포 전략을 자동화하고 이전 파드에서 새로운 파드로의 전환 속도를 제어할 수 있다.
- 실무에서 POD를 배포할 때 ReplicaSet 오브젝트를 사용하는 것이 아니라 Deployment를 이용해 배포한다.

`Deployment가 생성하는 ReplicaSet 이름 규칙`
- <Deployment-name>-<pod-template-hash>

### Deployment 배포전략
1. `recreate`
    - 이전에 배포되어있는 POD들을 순차적으로 하나씩 지우면서 롤아웃을 시작하고 모든 POD가 다 지워지면 새로운 ReplicaSet과 POD집합을 생성시키며 롤아웃을 마무리한다.
    - POD가 존재하지 않는 구간이 존재해 Service DownTime이 존재한다. - 개발 단계에선 유용
      - 자원을 N배로 사용하지 않으며 버전간 혼용이 없기 때문
2. `RollingUpdate`
    - 새로운 POD생성과 이전 POD 종료가 동시에 일어나는 방식
      - 이전 Pod가 모두 지워질 때 까지 점진적으로 새로운 POD를 생성하며 파드간 롤 아웃/ 롤백을 진행한다.
      - POD가 존재하지 않는 구간이 없어 Service DownTime이 없지만 이번 버전과 새로운 버전이 모두 존재한다.
        - 트래픽이 들어올 때 이전 버전으로 들어가거나 새로운 POD가 다른 트래픽을 받는 등 혼동될 여지가 존재한다.
      - 이전 버전 & 새 버전이 모두 존재하여 자원을 더 많이 점유하며 이 때문에 넉넉한 컴퓨팅 자원이 필요하다.
    - 점진적으로 배포하는 속도를 제어할 수 있는데 해당 옵션은 하단과 같다.
    - 1. `maxUnavailable` - 최대 몇개까지 즉시 종료할것인가를 지정하는 것
        - 롤링 업데이트를 수행하는 동안 유지하고자 하는 최소 POD의 비율(수)를 지정할 수 있다.
        - 최소 POD 유지 비율 - maxUnavailable값
        - ex: replicas 10, maxUnavailable 30%
          - 이전 버전의 POD를 replicas 수의 최대 30%만큼 즉시 Scale Down할 수 있다.
          - replicas를 10으로 할 때, 이전 버전의 POD를 즉시 3개만큼 종료할 수 있다.
          - 새로운 POD생성과 이전 POD종료를 진행하며 replicas 수의 70% 이상의 POD를 항상 Running 상태로 유지해야 한다.
    - 2. `maxSurge`
        - 롤링 업데이트를 수행하는 동안 허용할 수 있는 최대 POD의 비율(수)을 정할 수 있다.
        - 최대 POD허용 비율 = maxSurge값
        - ex: replicas 10, maxSurge 30%
          - 새로운 버전의 POD을 replicas수의 최대 30%까지 즉시 Scale up 할 수 있다.
          - 새로운 버전의 POD을 3개까지 즉시 생성할 수 있다.
          - 새로운 버전의 POD생성과 이전 버전의 POD종료를 진행하며 총 POD수가 replicas수의 130%를 넘지 않도록 유지해야 한다.

| `recreate` | `RollingUpdate` |
| :---: |  :---: |
| 새로운 버전 배포 전 이전 버전 배포 | 새로운 버전을 배포하면서 이전 버전 종료|
| 컨테이너가 정상적으로 시작되기 전까지 서비스하지 못함 | 서비스 다운 타임 최소화  |
| replicas 수만큼 컴퓨팅 리소스 필요 | 동시에 실행되는 Pod 개수가 replicas를 넘게 되므로 컴퓨팅 리소스가 더 필요함 |
| 개발단계에서 유용 | 운영단계에서 유용 |

### 배포간 버전 관리
- Deployment는 롤아웃 히스토리를 Revision#으로 관리한다.
- Deployment revision1 2 등등 오름차순으로 늘어나며 높은게 최신순이고 revision으로 롤백을 진행할 수 있다.
- k8s 시스템에서 배포 기록을 관리할 수 있고 revision으로 관리하며 현재 버전의 Pod-Template을 해싱해서 가지고 있다.
  - pod-template-hash가 같으면 같은 파드 템플릿을 배포했다는 것을 알 수 있다.

`롤백 명렁어`
- kubectl rollout undo deployment <deployment-name> --to-revision=1
  - 어떤 리비전 버전으로 롤백할 것인지 명시하면 해당 버전에서 배포됐던 파드 템플릿을 이용해 교체가 이뤄진다.

`히스토리 관리시 이력을 남기는 cli`
- kubectl annotate deployment/my-app kubernetes.io/change-cause="image reverted to 1.0 for a few bugs"
  - annotate는 특정 오브젝트의 메타 데이터를 추가하며 그 중 주석을 추가하는 리소스이며 kubernetes.io/change-cause는 배포 변경 이력을 관리하는 표준 주석 키이다.
  - 해당 키로 추가하게되면 이후 kubectl rollout history 로 조회시 변경 이력을 확인할 수 있으며 그 땐 상단의 키로 변경 이유가 등록되어있어야 한다.