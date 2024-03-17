## Helm 이란?

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/9443f5e7-656b-4527-b3c5-8c2b928e95b0)

- Helm 이란 쿠버네티스 패키지 관리 도구로서 배포하고자 하는 애플리케이션 단위를 패키징하여 관리하는데 중점을 두고 k8s상에서 개발하고, 찾고, 공유하는데 최적화된 툴이라고 할 수 있다.

### Helm을 사용하는 이유?
```
배포하고 실행하고자 하는 오브젝트의 컨테이너 배포 및 실행방식을 kubernetes Manifest로 작성하고 해당 Manifest파일을 기반으로 배포, 실행했던게 기존의 방식이었는데
MSA환경으로 가며 서버가 많아지고 서버와 같이 배포해야 하는 ConfigMap, Service등 수많은 오브젝트와 연관되어 같이 배포되고 실행되어야 정상적으로 동작하는
다수의 봊갑한 형태로 배포되는 Manifest들을 일일이 배포 관리, 버전, 워커노드 파드 스케줄링등을 관리하는 것은 어려워진다는 점에 있다.
```

**이러한 부분을 패키징화하여 하나의 패키지로 묶어 관리하고 버전, 형상관리할 수 있는 Helm 이라는 패키지로 관리하고 쿠버네티스에 활용할 수 있는 방식이 채택되고 표준화됨으로써 보편화 된 것**

### Helm의 주요 특징

| 구분 | 설명 |
| :--: | :--: |
| 관리 복잡성 해소 | - Helm Chart는 복잡한 애플리케이션을 쉽게 설치 및 반복 설치하도록 패키징 <br> - Helm을 통해 여러 오브젝트를 한번에 배포하도록 구성하여 관리 복잡성 해소 |
| 업데이트 용이 | - 배포된 어플리케이션에 대한 설정 변경 후 업데이트나 CLI옵션을 통한 업데이트 용이 <br> - Helm Upgrade 명령어를 통해 업데이트 가능 |
| 쉬운 공유 | - Helm Chart는 개인, 공용의 커스텀 Helm Repository에 패키징해 쉬운 공유 및 제공 <br> - 공식적인 특정 Helm Repository에서 공개 된 Chart로 kubernetes에 바로 배포 |
| 버전 관리 | - Helm을 통해 설치하고 업데이트할 때마다 Revision으로 버전 관리 <br> - Helm Rolllback 명령어를 이용해 쉽게 이전 버전으로 Rollback |

### Helm 주요 개념
1. Chart(패키지)
2. Repository(저장소)
3. Release(인스턴스)

#### Helm CLI 명령어 모음
> Helm Repo
1. Helm Stable Repository
   - http://charts.helm.sh/stable
2. Bitnami kubernetes OpenSource Repo - Vmware 벤더사쪽에서 제공
   - https://charts.bitnami.com/bitnami
3. AWS EKS 관련 Repo
   - https:// aws.github.io/eks-charts

1. 설치확인
    - helm version
2. Helm Repo 추가
    - helm repo add [repo명] [Repo URL]
3. Repo 조회
    - helm repo list
4. Repo 삭제
    - helm repo remove [repo명]
5. Repository 정보 업데이트
    - helm repo update
6. Repository 내 Chart 조회
    - helm search repo [공식 Helm Charts 릴리즈명]
7. Helm Chart 설치
    - helm install [helm repo명]/[공식 Helm Charts 릴리즈명][옵션] (Repo참조)
    - helm install [배포될 Helm Chart 릴리즈명][Helm Chart 파일 경로]

##### 자주 사용되는 옵션
| 옵션명 | 설명 |
| :--: | :--: |
| --version | chart의 버전 지정. <br> Chart.yaml안에 version 정보를 참조 |
| --set | 해당 옵션으로 values.yaml 값을 동적으로 설정 |
| --namespace | chart가 설치될 네임스페이스를 지정 <br> - 특정 네임스페이스에 종속되어 배포됨 |
    
8. 배포된 Helm Chart 릴리즈 목록 확인
    - helm list , helm ls
9. 배포된 특정 Helm Chart 릴리즈 상태 및 제공 도움말 확인
    - helm status [배포된 특정 Helm Chart 릴리즈명]
10. Helm Chart 업데이트
    - helm upgrade [배포된 Helm Chart 릴리즈명] [Helm Repository명]/[공식 Helm Chart 릴리즈명][옵션] - Repo 참조
    - helm upgrade [배포된 Helm Chart 릴리즈명][Helm Chart 파일 경로] - 파일 참조
11. Helm Chart 버전 히스토리 확인
    - helm history [배포된 Helm Chart 릴리즈명]
12. Helm Chart 버전 RollBack
    - helm rollback [ 배포된 Helm Chart 릴리즈명][Rollback할 Revision 번호]
13. Helm Charts Fetch
    - helm fetch --untar [Helm Repo 명]/[공식 Helm Chart 릴리즈명] --version [Helm Chart 버전]
         - Fetch 후 압축 해제된 상태의 Helm Chart 디렉토리 확인
14. Helm Chart 생성 명령어
    - helm create [Helm Chart명]
15. Helm Chart 코드 문법 스캔 명령어
    - helm init [Helm Chart 디렉토리 경로]
16. Helm Chart 랜더링(Dry-run) audfuddj
    - helm template [Helm Chart 디렉토리 경로]
17. Helm Chart 패키징 명령어
    - helm package [Helm Chart 디렉토리 명령어]
18. Helm Chart Test 수행 명령어
    - helm test [Helm Chart 릴리즈명]
19. Helm Chart 내 Chart.yaml에 설정된 정보 확인 명령어
    - helm show chart [Helm Chart 디렉토리 경로]
20. Helm Chart 내 Chart.yaml 및 values.yaml에 설정된 정보 확인 명령어
    - helm show all [helm Chart 디렉토리]


### Helm Chart의 파일 구조

| 파일명 | 설명 |
| :--: | :--: |
| Chart.yaml | Helm Chart에 대한 이름, 버전, 설명이 설정된 파일|
| charts/ | Helm Chart에서 참조해서 사용할 특정 Helm Charts 패키지를 저장하는 디렉토리 |
| values.yaml | Helm Chart의 변수값이 설정된 파일 |
| templates/ | Template Manifest 파일 저장 디렉토리 |
| _helpers.tpl | Template Manifest에서 공용으로 사용하는 변수값 정의 및 구현하는 파일 |
| NOTES.txt  | Helm install, upgrade, status시 출력되는 도움말 혹은 정보를 조합해 출력하는 파일 |
| *.yaml | Helm Charts로 릴리즈 배포할 kubernetes Object의 Template Manifest 파일 |

## Helm 사용 목적
- 다수의 오브젝트, 리소스를 활용 & 오브젝트 및 리소스들을 변수에 따라 동적으로 템플릿화하고 해당 템플릿화된

  Manifest를 생성 및 파일 기반 kubernetes 오브젝트를 생성하기 위함

#### Chart.yaml의 적용 항목

| 항목 | 설명 |
| :--: | :--: |
| apiVersion | Helm 자체의 API Version으로 Helm3부터는 v2지원(Required) |
| name | Helm Chart명(Required) |
| description | Helm Chart의 설명(Optional)|
| type | Helm Chart의 유형, 유형은 application 및 library 중 택1(Required) <br> application은 버전이 저장된 아카이브로 패키징 할 수 있는 템플릿 모음, 배포 O <br> library는 chart 개발시 필요한 유틸리티 혹은 기능을 제공, 템플릿이 없으므로 배포 X |
| version | Helm Chart버전으로 SemVer(Semantic Versioning)규칙을 준수해야함(Required) |
| appVersion | 버전 형태는 숫자로 된 x.y.z 형식이어야함(https://semver.org/lang/ko/) <br> Helm Charts에서 제공되는 App 버전이며, SemVer 형식을 따르지 않아도 됨 (Optional) <br> 보통은 배포될 App Container Image의 버전(Tag)를 명시|

#### values.yaml에 적용 항목
- template에 지정하는 변수값이 선언됨.
- template에 k8s의 여러가지 object즉 Manifest가 명시되어 있고 해당 Object를 배포하기 위해선 Object에 들어가는 변수값이 정의되야

  파라미터에 변수값이 매핑되고 구현되어 생성할 때 그 값들이 조합되어 k8s Manifest가 되어 배포됨

- key-value 값으로 넣고 template에 values.yaml에 들어가는 key값을 변수로 지정하고 키가 되는 파라미터로 매핑될 수 있게

  values.yaml에 명시되어 있다.

#### template/*.taml내 적용 항목
| 항목 | 설명 |
| :--: | :--: |
| .values | values.yaml 파일에서 설정된 변수값 구현|
| .Charts | Chart.yaml 파일에서 설정된 변수값 구현|
| .Release | 배포할때 할당된 정보를 변수로 구현 <br> (ex. Helm Charts 릴리즈명을 nginx로 할 경우 .Release.Name에 Nginx로 구현)|
| include | _helpers.tpl에서 설정된 변수값 구현 |
| if ~ end | if문(statement) 구현 (구문 안 else적용 가능, if not 형태로 적용 가능) |
| with ~ end | 변수에 대한 범위(scope)를 지정, 해당 범위는 . 아래 설정된 변수값을 구현 | 
| nindent | 들여쓰기 적용 공백수(char 단위) | 
| toYaml | 구현된 변수를 yaml형식으로 변경 |
| default | 기본적으로 구현되는 변수 |
| quote | 변수값이 string type으로 구현 |

### Reference
<https://helm.sh/><br>
