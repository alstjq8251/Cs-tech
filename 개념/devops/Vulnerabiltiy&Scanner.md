## Vulnerability
- 취약점을 일컫는 용어로 DevSecOps등에서 sw 시스템, 애플리케이션, 네트워크 등 다양한 환경에서 발생할 수 있는 보안 허점을 의미한다.

### 컨테이너 이미지 스캔이란?
- 도커같은 컨테이너 런타임 환경에서 실행시킬 수 있는 컨테이너 이미지들을 분석하여 보안 취약점들을 찾아내는 활동을 말한다.

#### 컨테이너 이미지 스캔을 하는 이유?

### scanning tool 
- `anchore`
- `docker scout`
- `trivy`
- `clair`
- `gripe`


### 컨테이너 이미지 스캔 툴 종류와 특징

1. Anchore
<img width="830" alt="image" src="https://user-images.githubusercontent.com/98382954/217847641-2dddb21e-be07-4485-b629-e580817bd0a5.png">

### Reference
- docker scout vs trivy - [DevSecops Tools in CICD Pipeline - DEV Community](https://dev.to/akhil_mittal/devsecops-tools-in-cicd-pipeline-375p)
- synk vs trivy vs gripe vs GoGuard-CLI - [The different flavours of Docker image security](https://www.coguard.io/post/docker-security-snyk-grype-trivy-coguard)
- gripe vs trivy vs synk vs ms - [Cloud Container Scanning Showdown: Which Tool is Best? - CloudFuel](https://cloudfuel.eu/blog/cloud-container-scanning-showdown-which-tool-is-best/)
- trivy vs scout vs gripe vs synk - [4 Free, Easy-To-Use Tools For Docker Vulnerability Scanning | by Tin Plavec | ITNEXT](https://itnext.io/4-free-easy-to-use-tools-for-docker-vulnerability-scanning-bb73342c0faa)
- trivy vs clair vs anchore vs quay - [Comparison with Other Scanners - Trivy](https://aquasecurity.github.io/trivy/v0.17.2/comparison/)
- trivy vs anchore vs  scout vs synk vs sysdig - [Container Security: A Complete Overview of GitHub Actions Integrated Image Scanning Tools | by Anshumaan Singh | Medium](https://medium.com/@anshumaansingh10jan/container-security-a-complete-overview-of-github-actions-integrated-image-scanning-tools-832e6406ec23)
- scanning tool , security tool [Docker image security - a handy comparison of the top tools](https://10clouds.com/blog/devops/docker-image-security-a-handy-comparison-of-the-top-tools/)
- trivy 공식 홈페이지 - [Overview - Trivy](https://aquasecurity.github.io/trivy/v0.57/docs/)