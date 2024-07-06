## Operator 개념 전 알아야될 컴포넌트

`Kubernetes Controller`
- Kubernetes Controller는 Object의 선언적인 Spec 기준 원하는 상태(Desire State)를 읽고 Object의 현재 상태(Current State)와 비교해서 처리한 후에 etcd에서 상태(Status)를 갱신하는 방식으로 동작하는 컴포넌트
- CRD를 사용하여 만든 Custom Resource를 이용하여 사용자가 원하는 상태를 선언하면(etcd에 갱신되면), Custom Controller가 그 상태를 맞추기 위해 동작

```
kubernetes에서 제공하는 기본적인 컨트롤러 외에도 커스텀 컨트롤러가 존재하고 
원하는 방식으로 커스텀 리소소들을 관리하고 배포 실행 이슈 관리, 파드 통신 등 
다양한 기능들에 대한 니즈등에 대한 부분을 kubernetes상에서 자동화로 구현하기 위해서 구현된 형태가 Operator 방식
```

### Operator란?
- kubernetes Operator는 Custom Resource Definition(CRD)를 기반으로 애플리케이션 및 컴포넌트를 관리하는 kubernetes API 확장 개념

### 이점
- Operator 방식으로 특정 kubernetes 상의 워크노드에서 배포되고 실행되고자 하는 부분 그리고 수행 방식의 자동화를 구현할 수 있다.

### 특징
- kubernetes의 워크로드 배포, 실행 등의 자동화 및 수행 방식의 자동화 구현
- CRD를 위해 API를 구현하는 방식같이 구현, 처리하고자 하는 형태들을 패넡화해서 관리하는 방식
- kubernetes Controller 개념을 통해 Go 언어 기반의 kubernetes 구현 소스코드를 수정(빌드/컴파일/배포)하지 않고 k8s 클러스터의 동작을 확장

### 도구
- Operator Framework
- Operator Lifecycle Manager(OLM)
  - Operator의 생태주기를 관리하는 매니저로 Operator를 설치 , 업데이트, 백업, 스케일링 등 운영/관리 가능
  - operatorhub.io에 OLM과 연관지어 Operator방식으로 Custom Resource를 배포할 수 있다.
- Kudo(Kubernetes Universal Declarative Operator)
  - Operator 작성 도구
  - yaml 형태의 선언적 명세 작성만으로 손쉽게 Operator를 만들 수 있는 도구 제공
  - Operator 라이프 사이클 간의 공통화 및 재사용 할 수 있는 기능 제공

### Helm과의 차이점
- Helm은 변경되는 내용을 value.yml이나 set옵션을 줘서 변경하거나 업데이트 후 변경된 내용을 확인해야함 (수동)
- 배포, 실행, 업데이트 부분의 차원을 넘어 실제 kubernetes상에서 변경되는 형태들을 좀 더 자동화된 형태로 관리함