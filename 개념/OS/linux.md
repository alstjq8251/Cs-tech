### linux란?
- Linux®는 오픈소스 운영 체제(OS)이다.
  - 운영 체제(Operating System, OS)는 CPU, 메모리, 스토리지처럼 시스템의 하드웨어와 리소스를 직접 관리하는 소프트웨어이며 OS는 애플리케이션과 하드웨어 사이에서 모든 소프트웨어와 작업을 수행하는 물리적 리소스를 연결한다.

#### NameSpace
- 프로세스를 실행할 때 시스템의 리소스를 분리해서 실행할 수 있도록 도와주는 기능이다.
- 프로세스를 격리 시킬 수 있는 가상화 기술로 nameSpace는 자원의 종류를 제한한다.

### Cgroups
- Control Groups의 약자로 프로세스들의 자원 사용을 제한하고 격리시키는 리눅스 커널 모듈이다.
- 하나 또는 복수의 장치를 묶어서 하나의 그룹을 만들 수 있으며 개별 그룹은 시스템에서 설정한 값만큼 하드웨어를 사용할 수 있다.
