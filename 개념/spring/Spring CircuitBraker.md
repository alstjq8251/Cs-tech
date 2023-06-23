### Spring Cloud CircuitBraker란?
- Fault Tolerance(=장애 허용 시스템) 에서 사용되는 대표적인 패턴으로써 서비스에서 타 서비스 호출 시 에러, 응답지연, 무응답,
  일시적인 네트워크 문제 등을 요청이 무작위로 실패하는 경우에 Circuit를 오픈하여 메세지가 다른 서비스로 전파되지 못하도록 막고
  미리 정의해놓은 Fallback Response를 보내어 서비스 장애가 전파되지 않도록 하는 패턴

### Spring Circuit Braker Library
`Netfix Histrix`
  - Netfix에서 개발했으며 MSA 환경에서 분산된 서비스간 통신이 원활하지 않은 경우에 각 서비스가 장애 내성과 지연 내성을 갖게하도록 도와주는 라이브러리이다.
    - 현재는 더 이상의 개발 지원이 중단됐으며 Spring Boot 2.4버전부터는 지원되지 않는다. [링크](https://spring.io/blog/2018/12/12/spring-cloud-greenwich-rc1-available-now)
    - Histrix에서도 Resilience4j 사용을 권고하고 있다. [링크](https://github.com/Netflix/Hystrix)
  
`Resilience4j`
  - 함수형 프로그래밍으로 설계된 경량(lightweight) 장애 허용(fault tolerance) 라이브러리로, 서킷브레이커 패턴을 위해 사용된다.
  - Resilience4j 는 Netflix Hystrix 로 부터 영감을 받은 Fault Tolerance Libray
  - 사용하기 가볍고 다른 라이브러리 의존성이 없음
  - Java 전용으로 개발된 경량화된 Fault Tolerance Libray
    - 여기서 Fault Tolerance는 하나 이상의 구성 요소에 문제가 생겨도 시스템이 중단없이 계속 작동할 수 있는 시스템을 의미하며

      해당 시스템을 지원하는 라이브러리라고 할 수 있다.

### Reference
<https://mangkyu.tistory.com/289><br>
<https://bkjeon1614.tistory.com/711><br>
