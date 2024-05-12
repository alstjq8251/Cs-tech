## kustomize란
- kubernetes의 배포 도구로써 상속을 사용하여 배포 파일을 관리하는 도구다.

### 사용하는 이유
- 쿠버네티스 환경을 운영하면 같은 설정에서 세부 설정만 다르게 설정해 다른 클러스터에 배포해야 하는 시나리오가 존재한다.
- deployment파일을 stage, dev, prod환경에 배포하는데 세부 설정(ex: replicas)만 다를 때 각 환경별로 파일을 만들고

  해당 파일들을 관리하는 것 자체가 어렵고 비효율 적이기 때문에 이런 파일들을 관리하는 도구들이 k8s 배포 도구다.

    - kustomize, helm , Ksonnet같은 도구들이 대표적이다.

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/d28efd51-f58c-49ce-8b1d-831da078879e)

### 배포 도구의 방식

1. 템플릿 방식
  - Manifest파일을 작성할 때 환경에 따라 변경되는 부분을 변수로 처리한 템플릿을 만든 후에, 환경별로 변수 값을 채워 넣는 방식이다.
    - helm

2. 상속 방식
  - 객체지향형 프로그래밍(OOP)에서 사용하는 상속(Inherit)의 개념을 사용하는 방식이 있다.
  - 상속은 base Yaml파일을 정의한 후에 다른 변경할 부분만 상속받은 YAML에서 구현하는 방식이다.
    - kustomize

### Kustomize의 동작 방식
<img width="453" alt="스크린샷 2024-05-07 오후 11 31 30" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/76d25ab3-6f16-4ecd-9899-3feef592a22c">

- 사진과 같이 한 폴더내에 같은 파일들이 존재하고 resource 부분으로 참조하게 되면 하나의 배포단위로 묶이고 해당 리소스들을 생성해 kubernetes 오브젝트를 생성하게 된다.
- patch등 변동사항이 있을 경우에 맞춰서 kustomization을 수정하거나 따로 패치 파일을 두어 수정이 가능하다.

### Kustomize의 장점
- kustomize가 생성하는 파이를과 k8s 오브젝트들은 배포하기 위한 명세로 yaml을 사용하고 IAC로 관리가 가능하다.
- git과 같은 소스 관리, 코드 버전 제어 및 관리 툴에서 사용 가능하여 gitops방식으로 Manifest들을 관리하여 각기 다른 환경에서 배포 구성, 관리 들도 용이하게 하는게 장점이다.

- kustomize는 CD툴에서 배포하는 파일을 관리하고 환경별로 다르게 구성할때 잘 하기 위한 방법으로 많이 활용된다.

#### Kustomize의 버전별 차이점
- kubernetes 1.14버전까지는 kustomize를 사용하려면 플러그인으로 추가해야 했으나 그 이후 부터는 내장 명령어로 실행이 가능하다.
  - ex: kubecctl kustomize <command>

### Kustomize 활용 방법
> 버전이 올라가며 pache로 통일됐으나 방식은 참고가 가능하다.

`patchesStrategicMerge`
- 패치하려는 대상을 kustomization에 병합해 패치를 수행하는 방식(대상은 파편화, 조각화된 패치)
- 패치하려는 Patch Manifest 내부의 Object 네임은 반드시 대상으로 지정한 리소스 네임과 기본 템플릿과 동일해야 함
- Manifest당 Object를 교체/병합/삭제하는 패치가 권장

`patchesJson6902`
- kubernetes Object의 변경을 JSONPatch 규약을 따른다.
  - (https://datatracker.ietf.org/doc/html/rfc6902) 해당 사이트에 규약이 나와 있다.
  - 공식적으로 json에 대해 정의된 규약이 rfc 6902다.
- 패치하려는 대상의 정보는 Patch Manifest 내부의 Object path에 맞춰 value값을 입력한다.
- Json 패치의 정확한 리소스를 찾기 위해, 리소스 내부의 api group, version, kind, name을 kustomization.yaml에 명시한다.

| 방식 | 메서드 | 설명 | 예시 | 
| :--: | :--: | :--: | :--: | 
|   patchsStrategicMerge   | replace | replace를 포함하는 요소가 병합되는 대신 대체 | containers: <br> - name: nginx <br>  image: nginx-1.0 <br>  - $patch: replace |
|   patchsStrategicMerge   |  merge  | merge를 포함하는 요소가 대체되는 대신 병합 | containers: <br> - name: nginx <br> image: nginx-1.0 <br> - name: log-tailer <br> image: fluentd:latest |
|   patchsStrategicMerge   | delete | delete를 포함하는 요소가 삭제 |  containers: <br>- name: nginx name: nginx | 

### Kustomized 명령어
- kubectl kustomize <kustomize_directory>
  - 해당 디렉토리에 있는 리소스들이 어떻게 생성되는지 확인하려면 해당 명령어로 확인할 수 있다.
