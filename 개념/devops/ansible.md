## ansible

`역할`
- 오픈소스 IT 자동화 도구로 사용자가 수작업으로 진행하던 프로비저닝, 환경 설 정, 애플리케이션 배포 등의 IT 업무를 코드 기반으로 작성하여 여러 환경에 동일하게 적용될 수 있도록 돕는 역할을 한다.

### ansible의 각 요소

`playbook`
- 사용하는 명령을 ad-hoc 명령으로 실행하지 않고 스크립트로 만든 것이다. 

### role

├── README.md                  # 역할(Role)의 설명서: 목적, 사용법, 요구 사항 등을 문서화
├── defaults                   # 기본 변수(default variables)를 정의하는 디렉터리
│   └── main.yml               # Role에서 사용하는 기본 변수 설정 파일 (우선순위가 가장 낮음)
├── files                      # 정적 파일을 저장하는 디렉터리
│                              # 예: 인증서, 스크립트, 설정 파일 등
├── handlers                   # 이벤트 핸들러 정의 디렉터리
│   └── main.yml               # 상태 변경 후 트리거되는 핸들러 정의 (예: 서비스 재시작)
├── meta                       # 메타데이터 정보를 정의하는 디렉터리
│   └── main.yml               # Role의 의존성, 갤럭시 정보, OS 호환성 등을 기술
├── tasks                      # 주요 작업을 정의하는 디렉터리
│   └── main.yml               # 작업 목록(Task List)의 기본 파일
├── templates                  # Jinja2 템플릿 파일을 저장하는 디렉터리
│                              # 예: 동적으로 생성되는 설정 파일
├── tests                      # Role 테스트 파일을 저장하는 디렉터리
│   ├── inventory              # 테스트용 인벤토리 파일 (테스트 대상 서버 그룹 정의)
│   └── test.yml               # Role의 테스트 플레이북
└── vars                       # 고정 변수(static variables)를 정의하는 디렉터리
└── main.yml               # Role에서 사용하는 변수 설정 파일 (우선순위가 defaults보다 높음)