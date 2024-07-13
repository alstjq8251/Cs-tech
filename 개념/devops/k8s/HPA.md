## HPA
- Horizontal Pod Autoscaler라는 kuberentes에서 WorkerNode의 리소스를 자동으로 업데이트 할 수 있는 하나의 오브젝트 

### 역할
- 워크로드 리소스(Deployment, StatefulSet)를 자동으로 업데이트 
- 워크로드의 크기를 수요에 맞게 자동으로 스케일링

### 준비사항
- `Metric Server`