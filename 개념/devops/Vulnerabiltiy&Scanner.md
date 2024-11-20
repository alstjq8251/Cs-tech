## Vulnerability
- 취약점을 일컫는 용어로 DevSecOps등에서 sw 시스템, 애플리케이션, 네트워크 등 다양한 환경에서 발생할 수 있는 보안 허점을 의미한다.

### 컨테이너 이미지 스캔이란?
- 도커같은 컨테이너 런타임 환경에서 실행시킬 수 있는 컨테이너 이미지들을 분석하여 보안 취약점들을 찾아내는 활동을 말한다.

#### 컨테이너 이미지 스캔을 하는 이유?
- 코드 레벨에서의 품질 뿐만 아니라 취약점을 스캔하는 이유는 하단의 총 5가지 이유이다.

1. 취약점 탐지 (Vulnerability Detection)

### scanning tool 
- `anchore`
- `docker scout`
- `trivy`
- `clair`
- `gripe`

### scanning tool간 비용 및 제약사항 비교

| 항목                        | **Docker Scout**                                                          | **Trivy**                                                 | **Clair**                                         | **Anchore**                                                     | **Grype**                                            | **Syft**                                           | **Snyk**                         |
| ------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------- | -------------------------------------------------- | -------------------------------- |
| **라이선스 및 비용**             | - 일부 기능은 Pro, Team, 또는 Business 유료 구독 필요  <br>- Docker Desktop 4.17 이상    | - 무료 오픈소스, 제한 없이 사용 가능                                    | - 무료 오픈소스                                         | - 무료 오픈소스                                                       | - 무료 오픈소스                                            | - 무료 오픈소스                                          | - 일부 기능은 유료                      |
| **Docker Hub 퍼블릭 레지스트리**  | 제한 없음                                                                     | 제한 없음                                                     | 제한 없음                                             | 제한 없음                                                           | 제한 없음                                                | 제한 없음                                              | 제한 없음                            |
| **Docker Hub 프라이빗 레지스트리** | 최대 3개 비공개 레지스트리 지원, 각 저장소 500MB 제한  <br>일일 pull/push 제한 있음                | 제한 없음                                                     | 제한 없음                                             | 제한 없음                                                           | 제한 없음                                                | 제한 없음                                              | 제한 없음                            |
| **Docker Hub 외부 레지스트리**   | 지원 불가 (유료 플랜 구독 후에만 사용 가능)                                                | 제한 없음 (다양한 외부 레지스트리 지원)                                   | 제한 없음                                             | 제한 없음                                                           | 제한 없음                                                | 제한 없음                                              | 제한 없음                            |
| **Docker Desktop 통합**     | Docker Desktop 4.17 이상 필요  <br>Docker Hub에서 Docker Scout 활성화              | 별도 설치 필요,                                                 | 별도 설치 필요, Docker Desktop 통합 없음                    | 별도 설치 필요, Docker Desktop 통합 없음                                  | 별도 설치 필요, Docker Desktop 통합 없음                       | 별도 설치 필요, Docker Desktop 통합 없음                     | 별도 설치 필요, Docker Desktop 통합 없음   |
| **지원 환경**                 | Docker Hub 이미지 스캔 (주로 컨테이너 이미지)                                           | Docker, Kubernetes, 파일 시스템 스캔 등 다양한 환경에서 사용 가능            | Docker 이미지 스캔 (주로 Red Hat Quay와 통합 사용)            | Docker, Kubernetes, 파일 시스템 스캔 등 다양한 환경에서 사용 가능                  | Docker 이미지 스캔                                        | Docker 이미지 스캔 및 SBOM 생성                            | Docker 이미지 스캔 및 다양한 환경에서 사용 가능   |
| **출처 및 문서**               | [Docker Scout 요금제 및 제한사항](https://docs.docker.com/billing/scout-billing/) | [Trivy GitHub 페이지](https://github.com/aquasecurity/trivy) | [Clair GitHub 페이지](https://github.com/quay/clair) | [Anchore GitHub 페이지](https://github.com/anchore/anchore-engine) | [Grype GitHub 페이지](https://github.com/anchore/grype) | [Syft GitHub 페이지](https://github.com/anchore/syft) | [Snyk 공식 웹사이트](https://snyk.io/) |

### Scan 범위

### 컨테이너 이미지 스캔 툴 종류와 특징

1. Anchore
<img width="830" alt="image" src="https://user-images.githubusercontent.com/98382954/217847641-2dddb21e-be07-4485-b629-e580817bd0a5.png">

2. trivy

3. clair

4. docker scout

5. gripe

### Reference
- docker scout vs trivy - [DevSecops Tools in CICD Pipeline - DEV Community](https://dev.to/akhil_mittal/devsecops-tools-in-cicd-pipeline-375p)
- synk vs trivy vs gripe vs GoGuard-CLI - [The different flavours of Docker image security](https://www.coguard.io/post/docker-security-snyk-grype-trivy-coguard)
- gripe vs trivy vs synk vs ms - [Cloud Container Scanning Showdown: Which Tool is Best? - CloudFuel](https://cloudfuel.eu/blog/cloud-container-scanning-showdown-which-tool-is-best/)
- trivy vs scout vs gripe vs synk - [4 Free, Easy-To-Use Tools For Docker Vulnerability Scanning | by Tin Plavec | ITNEXT](https://itnext.io/4-free-easy-to-use-tools-for-docker-vulnerability-scanning-bb73342c0faa)
- trivy vs clair vs anchore vs quay - [Comparison with Other Scanners - Trivy](https://aquasecurity.github.io/trivy/v0.17.2/comparison/)
- trivy vs anchore vs  scout vs synk vs sysdig - [Container Security: A Complete Overview of GitHub Actions Integrated Image Scanning Tools | by Anshumaan Singh | Medium](https://medium.com/@anshumaansingh10jan/container-security-a-complete-overview-of-github-actions-integrated-image-scanning-tools-832e6406ec23)
- scanning tool , security tool [Docker image security - a handy comparison of the top tools](https://10clouds.com/blog/devops/docker-image-security-a-handy-comparison-of-the-top-tools/)
- trivy 공식 홈페이지 - [Overview - Trivy](https://aquasecurity.github.io/trivy/v0.57/docs/)