## HPA
- Horizontal Pod Autoscaler라는 kuberentes에서 WorkerNode의 리소스를 자동으로 업데이트 할 수 있는 하나의 오브젝트 

### 역할
- 워크로드 리소스(Deployment, StatefulSet)를 자동으로 업데이트 
- 워크로드의 크기를 수요에 맞게 자동으로 스케일링

### 준비사항
- `Metric Server`

#### Hpa시 적용되는 Request, Limit의 의미
`하드 쿼터`
    - 정해놓은 상한선 이상으로 더 이상 자원을 할당할 수 없게 만드는 자원의 기준-limit
`소프트 쿼터`
    - 경고성의 메세지, 파드 로그, 상태 값으로 자원의 경계선을 넘었어도 어느정도의 허용은 할 수 있는 기준점이 되는 자원의 기준-request

### Hpa 산정 사항 및 주의사항
- HPA의 기준은 request로 기준잡아 산정되게 된다.
- cpu는 보통 코어단위로 측정되지만 연산 처리를 하는 기준으로 코어 이하로 측정될 땐 mili형태로 해서 그 자원의 양을 산정할 수 있다.
    - 200m x 0.5(50%) = 100m(1Pod) - CPU사용량 1분 간격 체크
- 스케일 아웃은 1분 간격으로 수행되고 싱크를 15초 단위로 하는데 스케일 인은 스케일 아웃의 시간 대비 4~5배를 준다.
- 자원 사용량은 기복이 심하고 할당량에 비해 낮아졌다 다시 올라갈 수 있는데 일시적인 변동량에 따라 파드의 개수가 영향을 받는다면 안정적이지 않는다.
  - 또한 위 상황에선 자원 사용량을 추적하기 힘들기 때문에 4~5배를 줘서 안정적으로 서버를 운영하는 것이 좋다.

### Hpa 적용 명령어
- kubectl autoscale deployment <deployment명> --cpu-percent=50 --min=1 --max=10

#### Hpa 테스트(CPU)를 도와주는 수행 명령어
- kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://test-deploy; done"