### jacoco란?
- javaCodeCoverage의 약자로 코드를 분석해주는 도구이다.
- JaCoCo는 자바 코드 커버리지를 체크하는 데에 사용되는 오픈소스 라이브러리이다.

#### Jacoco의 3가지 특징
1. Line, Branch Coverage를 제공한다.
2. 코드 커버리지 결과를 보기 좋도록 파일 형태로 저장할 수 있다.
3. html, xml, csv 등으로 Report를 생성할 수 있다.

### Jacoco를 연동하기 위한 과정 및 각 설정 별 설명
1. gradle
  - 하단의 사진처럼 의존성을 추가한다.
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227242229-7fdeffc0-aefc-46a1-9dce-97752913ea00.png">

  - 상단의 의존성을 추가하고 gradle refresh 혹은 ./gradlew clean build --refresh dependencies를 진행하면 하단을 확인 가능하다.
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227561314-83b3e960-4de4-49c0-8089-ad74f2599298.png">

**상단의 사진처럼 추가된 2가지는 Jacoco플러그인의 Task이다.**
1. `JacocoTestReport` 바이너리 커버리지 결과를 사람이 읽기 좋은 형태의 리포트로 저장해주는 Task
2. `JacocoTestCoverageVerification` 원하는 커버리지 기준을 만족하는지 확인해주는 Task

#### JacocopluginExtension 속성
- `JacocopluginExtension` 타입의 `Project extension`을 통해 추가적인 설정을 하단의 사진처럼 진행하며 설명은 다음과 같다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227700521-f31c5c11-8076-40cc-9640-c0ad1bc6ef21.png">

1. `reportsDir` Report가 생성될 디렉토리 경로
2. `ToolVersion` 사용할 Jacoco의 JAR 버전
  - 해당 버전은 gradle의 작성된 버전을 받아 사용하며 0.8.7버전을 사용한다.

#### JacocoTestReport Task 
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227700659-37650450-b77b-47a9-8a18-f53c213fa6d7.png">

상단의 사진처럼 Jacoco로 분석한 결과를 3개의 형태로 저장하며 하단에서 그 형태에 대한 설명을 진행한다.
- `html` 사용자가 읽기 편한 파일 형식
- `csv , xml` 소나큐브(SonarQube) 등 정적 분석 도구에 이용될 코드 커버리지 파일 형식
- 파일 형식 생성 여부(true,false)
  - 파일의 저장 위치를 커스텀 하는 것도 가능하다.
