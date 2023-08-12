## ELK란?
<img width="753" alt="image" src="https://github.com/alstjq8251/Cs-tech/assets/98382954/0a598860-851d-47a5-b807-a2176bd87d14">

- ElasticSearch , LogStash , Kibana의 앞글자를 딴 것을 의미하며 상단의 사진처럼 파이프라인을 구성하는 것을 말한다.

### ELK를 사용하는 이유?
- 버그 및 프로덕션 문제를 진단하고 해결하는 데 사용할 수 있다.
- 게다가 시스템 상태 및 사용량에 대한 추가 지표를 확보하며 시각화하여 확인할 수 있다.

### ELK를 사용하기 위한 절차
1. Spring등 로그를 전달하기 위한 서비스에서 LogStash로 로그를 전달한다.
2. LogStash에서 Input에 등록한 서비스에서 로그가 전달되면 Codec에 적힌대로 인코딩한뒤 Output으로 결과를 전달한다.
3. ElasticSearch는 색인된 데이터를 전달받고 Kibana에선 해당 데이터를 쿼리를 날려 자료를 시각화한다.

### ELK 각각이 하는 역할

1. `LogStash`
   - 로그같은 데이터 수집 후 변환하여 ElasticSearch같은 Stash로 전달하는 데이터 파이프라인
2. `ElasticSearch`
   - 검색 및 분석용 엔진 및 데이터 저장소
3. `Kibana`
   - Elasticsearch에서 차트와 그래프를 이용해 데이터 시각화를 가능하게 해주는 도구.

> ELK는 기본적으로 버전이 같아 모두 똑같은 버전을 사용하며 설치시 java 8버전이 설치되어 있어야 한다.

### ELK Stack 이란?

### Reference
<https://fubabaz.tistory.com/12>
