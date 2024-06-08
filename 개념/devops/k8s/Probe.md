## Probe
- Probe는 컨테이너에서 kubelet에 의해 주기적으로 수행되는 진단이다.
- Probe를 통해 쿠버네티스는 각 컨테이너의 상태를 주기적으로 체크한 후, 문제가 있는 컨테이너를 자동으로 재시작하거나 또는 문제가 있는 컨테이너를 서비스에서 제외할 수 있다.


### Handler
- 컨테이너의 상태를 진단하기 위해 어떻게 진단할 것인지를 명시한 게 Handler다.

### Haproxy HA
- Haproxy는 기본적으로 VRRP(Virtual Router Redundancy Protocol)를 지원한다.
  - Haproxy를 이중화하여 Master의 장애시 Slave가 Master의 VIP를 가져와 Master로 승격된다.

`동작원리`