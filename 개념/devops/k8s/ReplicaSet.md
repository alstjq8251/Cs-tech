## ReplicaSet
`replicaset은 pod 복제본을 생성하고 관리한다.`
- n개의 Pod을 생성하기 위해 N개의 명령어를 실행할 필요 x
- Replicaset 오브젝트를 정의하고 원하는 Pod개수를 replicas 속성으로 선언헌다.

> 우리가 replicas에 선언한 개수만큼만 Pod의 개수를 클러스터에서 유지시킨다는것을 의미 - Pod가 부족하면 늘리고 과하다면 삭제함으로써

### ReplicaSet을 사용하는 이유
Pod에 문제가 생겼을때 즉시 클라이언트의 요청을 처리할 수 없기 때문

### ReplicaSet을 사용함으로써 얻을 수 있는 이점
1. 소프트웨어의 내결함성(fault tolerance)
   - 소프트웨어나 하드웨어의 실패가 발생하더라도 사람의 개입없이 정상적인 기능을 수행할 수 있게 하기 위해서 
2. replicaset을 이용해서 Pod 복제 및 복구 기능 자동화
   - 클러스터 관리자는 Replicaset 오브젝트를 이용해서 필요한 Pod의 개수를 쿠버네티스에게 선언하고 쿠버네티스가 Replicaset요청서에 있는 replicas를 읽고 그 수만큼 Pod 실행을 보장

### ReplicaSet 예시 탬플릿
```yaml
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

### ReplicaSet을 사용하여 보장하는 Pod로 로드밸런싱을 하는 법은?
- replicaset은 파드 복제와 복구에만 초점을 맞춰져 있기 때문에 로드밸런싱이 자동으로 이뤄지지 않는다.
- 개발에서 연습으로 rs의 컨테이너 포트로 포트포워딩 후 통신해보면 첫번째 파드로만 포워딩 되고 다른 파드로는 포워딩이 이뤄지지 않는걸로 확인이 가능하다.

`ReplicaSet은 파드 복제와 복구에만 초점을 맞추기 때문에 다른 오브젝트(Service)를 사용해야 하며 오브젝트의 역할을 혼동하지 말아야 한다.`

### ReplicaSet으로 파드 배포 시 파드 버전을 바꿔 프로비저닝 하는 방법
1. Replicaset으로 rollBack rollOut을 시도하려면 replicas를 조정하거나 image를 바꾼뒤 replicas를 0으로 바꾸는 것으로만 가능하다.
   - RepliaSet은 label단위로 파드 그룹을 관리하며 기존에 관리하고 있던 파드 그룹을 삭제하고 replicas를 조정하여 파드를 재배포하는것으로 파드 버전의 업데이트가 가능하다.

**해당 방법은 새 Pod의 문제가 있거나 Pod의 버전을 바꿀때 매번 사용해야 하는 데 수동으로 해야하며 순단이 발생해 운영환경에선 적합하지 않다.**