### Open Feign이란?
`특징`
  - Open Feign이란 Netfix에 의해 처음 만들어진 `Declarative(선언적인) Http Client 도구`이다.
  - 외부 API 호출을 쉽게 할 수 있도록 도와주며 Spring Cloud 프로젝트 안에 속해있다.
    - 스프링 클라우드 프로젝트는 Spring MVC 어노테이션을 지원하고 Spring Web에서 기본적으로 사용하는 HttpMessageConverter를 똑같이 사용한다. 
  - 선언형 방식은 어노테이션을 붙여 사용하는 방식을 의미하는데 기존의 WebMvc의 어노테이션을 그대로 사용할 수 있어 학습이 크게 필요하지 않다.
  - 인터페이스에 어노테이션만 붙이면 사용할 수 있으며 편하게 개발을 도와준다.
  - Feign web mvc 어노테이션 및 JAX-RS 어노테이션을 포함하여 플러그 가능한 주석을 지원한다.
  - Feign은 플러그형 인코더와 디코더도 지원합니다.


### Reference
<https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/#spring-cloud-feign><br>
<https://mangkyu.tistory.com/278><br>
