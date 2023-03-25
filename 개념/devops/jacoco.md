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

#### JacocoTestCoverageVerification Task 
`JacocoTestCoverageVerification`은 코드 커버리지 분석을 진행할 당시 룰을 정의하는 테스크이며 룰에 정의된 기준을 만족하지 못하면 Task가 실패한다.

> 하단의 사진은 프로젝트에 적용되고 있는 코드 커버리지 분석 기준이다.
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227700924-7e0e5401-6d0a-4f8d-b462-b9f5a1fa072a.png">

`violationRules` 메서드를 통해 커버리지 기준을 설정하는 룰을 정의하며 `rule` 을 통해 메서드의 전달할 규칙들을 상세하게 정의하여 전달합니다.
  1. `rule - enable`  룰별로 해당 rule의 활성화 여부르 boolean으로 나타냄 - Default 값은 True
  2. `rule - element`  커버리지를 체크할 기준(단위)를 정의하며 총 6개의 기준이 존재한다.
      <details>
      <summary>element</summary>
        <div markdown="1">
          
        1. Bundle 패키지 번들(프로젝트 모든 파일 합친 것)

        2. Class 클래스

        3. Group 논리적 번들 그룹

        4. Method 메서드

        5. Package 패키지

        6. Sourcefile 소스파일

          - 지정하지 않을 경우 Default 값은 Buldle로 설정된다.
        </div>
      </details>
  3. `rule - includes`  rule 적용 대상을 package 수준으로 정의하며 Default 값은 전체 package 를 적용한다.<br>
  **`limit` 하위의 메서드 3개는 rule블럭 내 limit으로 블럭을 지정하여 메서드를 지정해 규칙을 전달한다.**
      <details>
      <summary>counter</summary>
        <div markdown="1">
          
          커버리지 측정의 최소 단위를 의미한다. 
          측정은 java byte code가 실행된 것을 기준으로 측정하며 , 총 6개의 단위가 존재합니다.
          
        1. `Branch` 조건문 등의 분기 수

        2. `Class` 클래스 수, 내부 메서드가 한 번이라도 실행된다면 실행된 것으로 간주
          
        3. `Instruction java` 바이트코드 명령 수 

        4. `Complexity` 복잡도
          
        5. `Method` 메서드 수, 메서드가 한번이라도 실행된다면 횟수로 간주
          
        6. `Line` 빈 줄을 제외한 실제 코드의 라인 수 

          지정하지 않을 경우 Default 값은 Instruction로 설정된다.
        </div>
      </details> 
      <details>
      <summary>value</summary>
        <div markdown="1">
          
          측정한 커버리지를 어떠한 방식으로 보여줄 것인지 설정한다.
          
        1. `COVERECOUNT` 커버된 갯수

        2. `COVEREDRATIO` 커버된 비율, 0부터 1사이의 숫자로 1 → 100%
          
        3. `MISSEDCOUNT` 커버되지 않은 갯수

        4. `MISSEDRATIO` 커버되지 않은 비율,  0부터 1사이의 숫자로 1 → 100%
          
        5. `TOTALCOUNT` 전체 개수

          값을 지정하지 않으면 Default 값은 COVEREDRATIO로 설정
        </div>
      </details>
      <details>
      <summary>minimum</summary>
        <div markdown="1">
          
          count값을 value에 맞게 표현했을 때 최솟값을 의미하며 이를 통해 코드 커버리지 분석의 성공 여부가 결정된다.
          
        1. BigDecimal 타입의 값을 사용하며 표기한 자리수만큼 value가 출력됩니다.

          - 0.8로 설정했다면 0.87로 설정했어도 0.8로 표시되는것을 의미

        **Default값이 존재하지 않아 꼭 설정해야 한다.**
        </div>
      </details>
 4. `excludes`
    - 커버리지 측정 시 제외할 클래스를 지정할 수 있다.
    - 패키지 레벨의 경로로 지정해야 하며 경로엔 *, ?와 같은 와일드카드를 포함시킬 수 있다.
  > `finalizedBy` 메서드로 Jacoco 플러그인 Task간 의존성을 설정할 수 있다.
    
      
