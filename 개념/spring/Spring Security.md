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

### Spring Security의 필터들
`UsernamePasswordAuthenticationFilter`
  - 사용자 이름과 암호가 포함된 인증 요청을 처리한다.
  - 요청에서 사용자 이름과 암호를 추출하고 `Authentication` 개체를 생성한 다음 인증을 위해 `AuthenticationManager`에 전달한다.

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
