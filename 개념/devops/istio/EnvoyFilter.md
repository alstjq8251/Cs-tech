### EnvoyFilter란?
- Istio Pilot에서 생성된 Envoy 구성을 맞춤설정하는 매커니즘을 제공한다.
- EnvoyFilter를 사용해 특정 필드 값을 수정하거나 , 필터 추가, 새로운 리스너, 클러스터 등을 추가할 수 있다.
  - proxy에서 응답헤더를 추가하는 등의 커스텀한 필터를 추가할 수 있다.
- 잘못된 구성을 적용하게 되면 전체 메시가 불안정해질 수 있어 주의해서 사용해야 한다.
- EnvoyFilter는 수량 제한없이 추가로 적용되며 순서는 EnvoyFilters와 워크로드에 일치하는 모든 EnvoyFilters순이다.

### Reference
<https://istio.io/latest/docs/reference/config/networking/envoy-filter/>