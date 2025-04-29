## keda
- Kubernetes Event-Drived Autoscaling의 줄임말로
  
  KEDA는 다양한 소스로 부터 이벤트를 받아 애플리케이션 파드들을 오토 스케일링 하기 위한 목적으로 개발된 경량의 쿠버네티스 컴포넌트이다.

### Keda Architecture

![Image](https://github.com/user-attachments/assets/3da9d0a7-82a3-4290-80dd-159b1111b5ea)

KEDA는 다양한 소스로 부터 이벤트를 받아 애플리케이션 파드들을 오토 스케일링 하기 위한 목적으로 개발된 경량의 쿠버네티스 컴포넌트이다.

`3가지 역할`

1. agent

2. metrics

3. Admission Webhooks

### 동작 방식

- Keda는 궁극적으로 HPA를 이용해서 Deployment, Replicaset, Job 등을 autoscaling 하게 된다.

### Keda의 CRD

- KEDA는 자동 크기 조정 기능을 수행하기 위해 ScaledObject, ScaledJob, TriggerAuthentication, ClusterTriggerAuthentication이라는 네 가지 사용자 정의 리소스를 제공한다.

`ScaledJob`
- Kubernetes Job에 대한 확장 규칙을 지정하는 데 사용된다.

`ScaledObject`
- 이벤트 소스와 객체 간의 매핑을 나타내며 K8s 클러스터의 Deployment, StatefulSet, Jobs 또는 사용자 지정 리소스에 대한 확장 규칙을 지정한다.

#### KEDA의 Trigger

1. kafka

2. aws

3. rabbitMQ