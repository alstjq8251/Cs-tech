### Open Feign이란?
`특징`
  - Open Feign이란 Netflix에 의해 처음 만들어진 `Declarative(선언적인) Http Client 도구`이다.
  - 외부 API 호출을 쉽게 할 수 있도록 도와주며 Spring Cloud 프로젝트 안에 속해있다.
    - 스프링 클라우드 프로젝트는 Spring MVC 어노테이션을 지원하고 Spring Web에서 기본적으로 사용하는 HttpMessageConverter를 똑같이 사용한다. 
  - 선언형 방식은 어노테이션을 붙여 사용하는 방식을 의미하는데 기존의 WebMvc의 어노테이션을 그대로 사용할 수 있어 학습이 크게 필요하지 않다.
  - 인터페이스에 어노테이션만 붙이면 사용할 수 있으며 편하게 개발을 도와준다.
  - Feign web mvc 어노테이션 및 JAX-RS 어노테이션을 포함하여 플러그 가능한 주석을 지원한다.
  - Feign은 플러그형 인코더와 디코더도 지원합니다.

`역사`
  - OpenFeign의 초기 모델인 Feign은 Eureka, Ribbon등을 포함한 Netflix OSS 프로젝트의 일부로 Netflix에 의해 만들고 공개됐다.
  - 이후 Spring Cloud진영에선 Spring Cloud Netflix란 프로젝트로 Netflix OSS를 Spring Cloud 생태계로 포함시키고

    Feign은 별도의 starter로 제공되었다.

  - 넷플릭스가 개발을 중단한 이후 OpenFeign이라는 이름과 함께 오픈소스 커뮤니티로 옮겨졌고 Spring Cloud는 OpenFeign역시 기존의 생태계로 통합했다.
  - 생태계로 통합되는 과정에서 Feign 자체 어노테이션, JAX-RS 어노테이션만 지원되는 부분에서 Spring MVC 어노테이션을 지원하도록 추가되었다.
  - Spring에서 사용되는 HttpMessageConverters를 사용하도록 확장했고 다른 Cloud 기술과 통합해 load-balanced된 http client를 제공하고자 했다.

`장점`
  - 인터페이스와 어노테이션을 기반으로 코드 가독성 향상 및 코드 수가 줄어 생산성을 향상시킨다.
  - Spring MVC 어노테이션을 지원하기에 사용하는데 있어 많은 학습이 필요하지 않다.
  - 다른 Spring Cloud(Euraka, Circuit Braker, LoadBalancer)와 통합이 쉽다.

`단점`
  - 테스트 도구를 제공하지 않는다.
    - 별도의 설정파일로 해당 문제를 해결해야 한다.
  - 공식적으로 Reactive 모델을 지원하지 않는다.
  - 기존 Http Client가 Http2를 지원하지 않는다.
    - 추가적인 설정을 해줘야 한다.  

### Reference
<https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/#spring-cloud-feign><br>
<https://mangkyu.tistory.com/278><br>
