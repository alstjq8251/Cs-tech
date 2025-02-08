## keda
- Kubernetes Event-Drived Autoscaling의 줄임말로
  
  KEDA는 다양한 소스로 부터 이벤트를 받아 애플리케이션 파드들을 오토 스케일링 하기 위한 목적으로 개발된 경량의 쿠버네티스 컴포넌트이다.

### Keda Architecture

![Image](https://github.com/user-attachments/assets/3da9d0a7-82a3-4290-80dd-159b1111b5ea)

KEDA는 다양한 소스로 부터 이벤트를 받아 애플리케이션 파드들을 오토 스케일링 하기 위한 목적으로 개발된 경량의 쿠버네티스 컴포넌트이다.

`3가지 역할`

1. agent

2. metrics

3. Admission Webhooks

### 동작 방식