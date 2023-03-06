### Spring Security란?
- Spring Security는 Java 에코시스템에서 보안 애플리케이션을 구축하기 위한 인기 있는 오픈 소스 보안 프레임워크이다.
- 인증 및 승인, CSRF(교차 사이트 요청 위조), 세션 관리 등과 같은 일반적인 보안 위협으로부터 애플리케이션을 보호하기 위한 포괄적인 보안 기능 세트를 제공한다.
- 개발자가 특정 보안 요구 사항을 충족하도록 기능을 사용자 지정하고 확장할 수 있는 유연하고 확장 가능한 아키텍처를 제공한다.
- 역할 기반 및 권한 기반 인증을 지원하므로 개발자가 애플리케이션의 다양한 리소스에 대해 세분화된 액세스 제어 규칙을 정의할 수 있다.


#### Spring Security의 핵심개념
- 들어오는 요청을 처리하고 보안 제약 조건을 적용하는 보안 필터 체인이 핵심 개념이다.
- 체인의 각 필터를 통해 인증,인가 등 특정 보안 작업을 수행하고 역할 , IP, 요청방법등 다양한 기준에 따라 보안규칙을 적용하도록 구성할 수 있다.

#### Spring Security의 고급 개념
- `Single Sign-On(SSO)`
- `2단계 인증(2FA)`
- `암호 인코딩`


### Spring Security Architecture
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/223129038-f14c697e-0951-4473-b334-d5b4ce36c312.png">

로그인 , 회원가입등의 유저의 요청이 들어왔을 때 Spring Security Architecture가 작동하는 흐름은 상단의 사진과 같다.(flow는 하단)
1. 유저의 요청을 Filter에서 검증한다.
   - SecurityConfig설정에서 요청을 패스하게 되어있는지? 혹은 url과 메소드가 맞는지 등의 여부를 거쳐 필터에서 요청을 가로채게 된다.
2. 기본적으로 로그인 시 `UsernamePasswordFilter`에서 요청을 받으며 검증하기 위한 객체로 `usernamepasswordtoken`을 만들어 사용한다.
3. 검증하기 위해 `AuthenticationManager`은 처리를 위임받고 자신이 갖고 있는(1...N)개의 `AuthenticationProvider`를 찾아 검증을 위임한다.
   - `AuthenticationProvider`는 `DaoAuthenticationProvider`를 상속받아 구현하는데 비밀번호 검증 , 인증객체 타입 비교 , 최종 인증객체 생성

   - 등의 책임을 갖고 있다.
5. `AuthenticationProvider`는 `UserDetailsService`에게 유저의 ID를 전달해 DB에 유저의 ID와 일치하는 정보가 있는지 검사를 시킨다.
6. `UserDetailService`는 전달된 ID로 DB에 유저 정보를 찾아 UserDetails라는 인증 객체를 만들어 반환한다.
7. 반환된 객체의 비밀번호를 비교해 틀릴 시 BadCredentialsException 예외를 발생시키며 성공하면 인증토큰을 만들어 반환한다.
8.1 Filter에선 인증이 성공적이면 SuccessHandler에게 처리를 위임한다.
   - Spring Security Inmemory 세션저장소인 SecurityContextHolder내부의 SecurityContext에 저장한다.
8.2 인증이 실패한다면 해당 예외를 `ExceptionTranslationFilter`에게 전달해 적절한 Handler를 찾아 처리를 위임한다.  


### Spring Security Filter Chain
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/223130384-b6383d98-e622-4839-af12-f70ee9fe5709.png">

### Spring Security의 필터들
`UsernamePasswordAuthenticationFilter`
  - 사용자 이름과 암호가 포함된 인증 요청을 처리한다.
  - 요청에서 사용자 이름과 암호를 추출하고 `Authentication` 개체를 생성한 다음 인증을 위해 `AuthenticationManager`에 전달한다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/223125681-3c6787d6-61b7-489d-b616-d19e59a507d4.png">

#### UsernamePasswordAuthenticationFilter Flow
- 기본적으로 UsernamePasswordAuthenticationFilter는 FormLogin방식에서 진행되는 필터이며 사용자의 Id, password를 검증한다.
- AbstractAuthenticationProcessingFilter라는 추상클래스를 상속해 사용하고 있으며 해당 클래스가 갖고있는

  RequestMatcher("login") url이 일치할 경우에 검증을 진행하고 아닐경우 다음 필터로 요청을 보낸다.
    - SecurityConfig에서 loginProcessingUrl()메소드를 통해 이 url을 바꾸거나 필터를 상속해 해당 필터의 url,메소드를 커스터마이징 하는것도 
    
      가능하다.
      
      <img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/223127126-37758706-4bde-4a9b-9ab7-5aa9a48efdc7.png">
- 요청으로 들어온 Id, passWord를 인증되지 않는 UsernamepasswordToken으로 만들어 AuthenticationManager에게 처리를 위임한다.

`BasicAuthenticationFilter`
  - 기본 인증 요청 처리를 담당한다.
  - `Authorization` 헤더에서 사용자 이름과 암호를 추출하고 `Authentication` 객체를 생성한 다음 인증을 위해 `AuthenticationManager`에 전달한다.

`ConcurrentSessionFilter`
  -  동일한 사용자에 대한 다중 세션을 방지한다
  -  각 사용자의 활성 세션 수를 추적하고 허용된 최대값을 초과하는 추가 세션을 무효화한다.

`CsrfFilter`
  - Csrf공격
  - 각 사용자 세션에 대한 CSRF 토큰을 생성하고 후속 요청에서 토큰의 유효성을 검사하여 요청이 합법적인지 확인한다.

`LogoutFilter`
  - Logout 요청 처리를 담당하는 필터이다.
  - 사용자의 세션을 무효화하고 모든 인증 관련 쿠키 또는 헤더를 지운다.

`AnonymousAuthenticationFilter`
  - 인증되지 않은 요청에 대해 익명 인증을 제공한다.
  - 사용자에 대한 `AnonymousAuthenticationToken`을 생성하고 체인의 다음 필터로 전달한다.

`ExceptionTranslationFilter`
  - 프로세스 중에 발생하는 예외 처리를 담당한다.
  - 모든 AuthenticationException을 포착하고 인증 , 인가에 관한 예외처리 핸들러로 위임한다.

`FilterSecurityInterceptor`
  - 인증 시행을 담당하는 필터이다.
  - 사용자의 역할 또는 권한에 기반한 규칙. 요청을 가로채고 AccessDecisionManager 및 SecurityMetadataSource를 기반으로 보안 규칙을 적용한다.

#### Reference
<https://catsbi.oopy.io/c0a4f395-24b2-44e5-8eeb-275d19e2a536><br>
