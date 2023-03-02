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
  - 요청에서 사용자 이름과 암호를 추출하고 `Authentication` 개체를 생성한 다음 인증을 위해 `AuthenticationManager`에 전달합니다.

`BasicAuthenticationFilter`
  - 기본 인증 요청 처리를 담당한다.
  - `Authorization` 헤더에서 사용자 이름과 암호를 추출하고 `Authentication` 객체를 생성한 다음 인증을 위해 `AuthenticationManager`에 전달합니다.
