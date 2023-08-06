## ELK란?
- ElasticSearch , LogStash , Kibana의 앞글자를 딴 것을 의미한다.

### ELK를 사용하는 이유?
- 버그 및 프로덕션 문제를 진단하고 해결하는 데 사용할 수 있다.
- 게다가 시스템 상태 및 사용량에 대한 추가 지표를 확보하며 시각화하여 확인할 수 있다.

### ELK를 사용하기 위한 절차
1. Spring등 로그를 전달하기 위한 서비스에서 LogStash로 로그를 전달한다.
2. LogStash에서 Input에 등록한 서비스에서 로그가 전달되면 Codec에 적힌대로 인코딩한뒤 Output으로 결과를 전달한다.
3. ElasticSearch는 색인된 데이터를 전달받고 Kibana에선 해당 데이터를 쿼리를 날려 자료를 시각화한다.
