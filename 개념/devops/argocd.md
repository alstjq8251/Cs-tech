##  Argocd
- ArgoCD는 GitOps를 구현하기 위한 도구 중 하나로 Kubernetes 애플리케이션의 자동 배포를 위한 오픈소스 도구이다

### Architecture

`ArgoCD application-controller`

`argocd repo server`
- 역할

`redis server`

`dex server`

`argocd notification server`

### install

`yaml`

`helm`

`operator`

#### Ha구성

### Gitops

`declarative configuration`
1. 모든 시스템 구성은 Git에 선언적으로 정의되어야 한다
- 애플리케이션 리소스, 환경 설정, 역할 정의 등 모든 구성은 YAML 등의 선언적 파일 형태로 Git 저장소에 저장되어야 하며, Git이 변경의 단일 출처(Single Source of Truth)임을 보장한다.