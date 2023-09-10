## 스프링 어노테이션 정리
#### Validation Annotation
- `implementation 'org.springframework.boot:spring-boot-starter-validation'` Validation 의존성 주입 코드
  - 자바에서는 Bean validation 이라는 유효성 검사 프레임 워크를 제공하고 사용하기 위해선 `gradle`버전에서 위 코드의 의존성을 주입 받아야 한다.
  
    - 1. `@Not Null` 유효성 검사 프레임워크중 Null값을 체크하기 위한 어노테이션이며 사용하기 위해선 값을 받는 과정 중 <br>@Valid 어노테이션을 붙여야 사용 가능하다. 
    - 2. `@Not Empty` 유효성 검사 프레임워크를 주입받아 사용하며 Null값과 빈칸을 체크가능하며 사용하기 위해선 값을 받는 과정 중 <br>@Valid 어노테이션을 붙여야 판별 가능하다.
    - 3. `@Not Blank` 유효성 검사 프레임워크를 주입받아 사용하며 Null값 빈칸 공백을 체크할 수 있어 가장 강력하며 사용하기 <br>위해선 @Valid 어노테이션으로 검사해야 가능하다.
    - 4. `@Email` 유효성 검사 프레임워크를 주입받아 사용하며 이메일 형식의 입력값인지 검사한다. 


---
#### 생성자 관련 어노테이션
- `@AllArgsConstructor` 클래스에 존재하는 모든 필드에 대한 생성자를 자동으로 생성한다.
  - 장점 : 모든 필드에 대한 생성자를 자동으로 생성하기 때문에 코드를 간략화 시켜줄 수 있고 따로 필드의 맴버들로 작성해줄 필요가 없다.
  - 단점 : 두 개의 같은 타입 인스턴스 멤버를 선언한 상황에서 개발자가 선언된 인스턴스 멤버의 순서를 바꾸면, 개발자도 인식하지 못하는 사이에 
         lombok이 생성자의 파라미터 순서를 필드 선언 순서에 따라 변경하게 된다. 이때 타입이 동일하면 IDE가 리팩토링을 해주지 않으며 
         기존 소스에서도 오류가 발생하지 않아 동작 하는 과정에서 값이 의도하는 대로 흘러가지 않을 수 있어
         편한 점 외에 가급적 사용을 지양하고 생성자에 @Builder를 붙여 리팩토링에 유연하게 대처하는것을 추천한다.
- `@NoArgsConstructor` 파라미터가 없는 생성자를 생성한다.
  - 장점: @AllArgsConstructor와 조합하여 사용시 파라미터가 없는 생성자와 필요한 모든 필드에대한 생성자를 자동으로 생성하여 개발자가 
    필드를 하나하나 지정하여 만들어주거나 코드를 따로 작성할 필요가 없게 만들어준다/
  - 단점: 필드들이 final로 생성되어 있는 경우에는 필드를 초기화할 수 없기 때문에 생성자를 만들 수 없고 에러가 발생한다.
    - @NoArgsConstructor(force=true)로 final 필드를 강제로 초기화 시킨다면 해당 필드에도 생성자를 만들어 줄 수 있다.
  - 주의사항: @NonNull 어노테이션이 설정되어 있다면 초기화가 진행되기 전까지 Null-Check가 생성되지 않기에 주의해야 한다.
- `@Builder`
  - 장점 : 위 어노테이션들이 가질 수 있는 주의사항을 모두 해결하는 생성자 관련 어노테이션이며 @Builder를 사용시 객체 생성을 명확히 할 수
          있고 휴먼 오류에 대하여 파라미터 순서가 아니라 필드값에 대응하는 값들을 지정하여 생성하기 때문에 유연하게 대처 가능하며 
          필드 순서에도 무관해진다는 장점이 존재한다.
 ##### 가급적 @Builder는 클래스가 아니라 직접 만든 생성자 혹은 객체 생성 메소드에 붙여주길 권고한다.
 
-`@Repository`

#### 테스트 관련 어노테이션
- `@WebMvcTest`
  - @Controller, @RestController가 설정된 클래스들을 찾아 메모리에 생성한다. 
- `@AutoConfigureMockMvc`
  - @Controller , @Service, @Repository가 분은 객체들도 모두 메모리에 생성한다.
  - @SpringBootTest와 함께 사용하면 
  - SpringBootTest(webEnvironment=WebEnvironment.MOCK)설정으로 모킹한 객체를 의존성 주입 받으려할때 사용한다. 
- `@ExtendWith(SpringExtension.class)`
  - Junit5에서는 ExtendWith 어노테이션을 통해 테스트 클래스 또는 메서드의 기능을 확장할 수 있다
    - Junit4에선 runwith 어노테이션이 동일한 기능을 제공한다.
  - Spring TestContext Framework의 기능을Junit5의 Jupiter 프로그래밍 모델에 통합하는 역할을 한다.  
    - @ExtendWith(SpringExtension.class)를 @ExtendWith(MockitoExtension.class) 로 작성해도 동일하게 작용한다.

    - MockitoExtension은 SpringExtension 보다는 적지만, 다양한 기능을 구현하여 테스트 클래스의 기능을 확장한다.
- `@DataJpaTest`
  - JPA에 관련된 요소들만 테스트하기 위한 어노테이션으로 JPA 테스트에 관련된 설정들만 적용해준다.
  - 메모리상에 내부 데이터베이스를 생성하고 @Entity 클래스들을 등록하고 JPA Repository 설정들을 해준다. 각 테스트마다 테스트가 완료되면 관련한 설정들은 롤백된다.
- `@Mock`
  - 테스트 중에 가짜 객체를 생성하게 해주는 어노테이션이며 @Mockito와 함께 사용하여 테스트를 수행할 수 있다.
  - @Mock으로 생성된 객체는 필드, 메서드가 모두 null값으로 생성되기 때문에 테스트를 하기 위해선 Mockito.when혹은 Mockito.given으로 미리 정의해야 가능하다.    

#### 주석 관련 어노테이션
- `@Override`
  - 인터페이스 , 추상클래스의 메소드를 상속하고 있음을 알려주는 주석의 성격을 띈 어노테이션이다. 

#### Spring Batch 어노테이션
- `@EnableBatchProcessing`
  - 스프링 배치가 시작되기 위해 선언되어야 하는 어노테이션으로서 총 4개의 설정 클래스를 실행시키며 스프링 배치의 모든 초기화 및

    실행 구성이 이뤄진다.
  - 스프링 부트 배치의 자동 설정 클래스가 실행됨으로써 빈으로 등록된 모든 Job을 검색해서 초기화와 동시에 Job을 수행하도록 구성됨
