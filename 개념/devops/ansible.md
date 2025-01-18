## ansible

`역할`
- 오픈소스 IT 자동화 도구로 사용자가 수작업으로 진행하던 프로비저닝, 환경 설 정, 애플리케이션 배포 등의 IT 업무를 코드 기반으로 작성하여 여러 환경에 동일하게 적용될 수 있도록 돕는 역할을 한다.

### ansible의 각 요소

`playbook`
- 사용하는 명령을 ad-hoc 명령으로 실행하지 않고 스크립트로 만든 것이다. 

### role

role의 기본 구조
```shell

├── README.md
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
└── main.yml

```
