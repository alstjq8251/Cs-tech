### Rest란?
`Representational State Transfer`의 약자로 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.
자원(resource)의 표현(representation) 에 의한 상태 전달이라고 할 수 있다.
- 자원: 해당 소프트웨어가 관리하는 모든 것
  -> Ex) 문서, 그림, 데이터, 해당 소프트웨어 자체 등
- 자원의 표현: 그 자원을 표현하기 위한 이름

Rest의 구체적 개념
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, 

  HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.

#### Rest의 장단점

#### Rest가 필요한 

API란?
- API(Application Programming Interface)란
데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 하는 것
REST API의 정의
REST 기반으로 서비스 API를 구현한 것
최근 OpenAPI(누구나 사용할 수 있도록 공개된 API: 구글 맵, 공공 데이터 등), 
마이크로 서비스(하나의 큰 애플리케이션을 여러 개의 작은 애플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍처) 등을 제공하는 업체 대부분은 REST API를 제공한다.
