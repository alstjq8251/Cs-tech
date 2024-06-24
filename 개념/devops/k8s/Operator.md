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