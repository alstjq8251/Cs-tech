## Harbor
- CNCF제단에 속한 프로젝트이며 Graduated 등급의 프로젝트이기 때문에 많은 레퍼런스를 소유하고 신뢰도가 높은 오픈소스다.
- 오픈소스 컨테이너 레지스트리다.
- RBAC기반의 접근 제어, 컨테이너 취약성 검사, 웹 UI, 레지스트리 레플리케이션 등 다양한 컨테이너 레지스트리 관리 기능들을 제공하고 있다.

### Architecture
![image](https://github.com/user-attachments/assets/361d681a-df24-464e-b000-81cdaca04f7a)

- Core service
    - Harbor의 API와 인증, 웹 GUI를 담당하는 Component입니다. 이미지의 Pull/Push에 필요한 Token을 발행하며 웹 환경의 GUI 및 Webhook을 제공한다.
- Job Service:
  - 이미지의 Replication과 같은 작업을 담당하는 Components
- Database
  - Harbor의 모든 Components들은 Stateless인데 이 때문에 데이터들은 모두 Redis나 PostgreSql같은 외부 저장소에 저장한다.
  - 이런 메다데이터같은 정보들을 저장하는 역할을 한다.

### Reference
<https://nangman14.tistory.com/78><br>
<https://kimmj.github.io/harbor/what-is-harbor>