##  Argocd
- ArgoCD는 GitOps를 구현하기 위한 도구 중 하나로 Kubernetes 애플리케이션의 자동 배포를 위한 오픈소스 도구이다

### Architecture

`ArgoCD application-controller`

`argocd repo server`
- cluster에 생성할 manifests들을 Git에서 참조하여 최종적으로 생성하는 책임이 있는 컴포넌트

`redis server`

`dex server`

`argocd notification server`

### install

`yaml`

`helm`

`operator`

#### Ha구성

### Gitops

1. `declarative configuration`
2. `Version-controlled immutable storage`
   - GitOps 접근 방식에서는 모든 인프라 및 애플리케이션의 변경 사항은 Pull Request(PR)를 통해 수행되어야 한다.
     직접 Git 저장소에 push하지 않고 PR 기반의 변경 절차를 따름으로써, **동료 간 리뷰(Peer Review)**가 가능하며, 변경 이력 추적성과 승인 절차를 확보할 수 있다.
3. `Automated delivery`
4. `Software agents`
5. `Closed loop`