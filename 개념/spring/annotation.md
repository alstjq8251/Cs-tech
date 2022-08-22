## 스프링 어노테이션 정리
#### Validation Annotation
- `implementation 'org.springframework.boot:spring-boot-starter-validation'` Validation 의존성 주입 코드
  - 자바에서는 Bean validation 이라는 유효성 검사 프레임 워크를 제공하고 사용하기 위해선 `gradle`버전에서 위 코드의 의존성을 주입 받아야 한다.
  
    - 1. `@Not Null` 유효성 검사 프레임워크중 Null값을 체크하기 위한 어노테이션이며 사용하기 위해선 값을 받는 과정 중 @Valid 어노테이션을 붙여야 사용 가능하다. 
    - 2. `@Not Empty` 유효성 검사 프레임워크를 주입받아 사용하며 Null값과 빈칸을 체크가능하며 사용하기 위해선 값을 받는 과정 중 @Valid 어노테이션을 붙여야 판별 가능하다.
    - 3. `@Not Blank` 유효성 검사 프레임워크를 주입받아 사용하며 Null값 빈칸 공백을 체크할 수 있어 가장 강력하며 사용하기 위해선 @Valid 어노테이션으로 검사해야 가능하다.
