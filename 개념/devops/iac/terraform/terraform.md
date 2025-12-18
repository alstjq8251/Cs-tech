## terraform

- terraform이란 cloud에서의 인프라를 생성하는데 가장 널리 알려지고 성숙된 iac 도구이다.
- harshicorp사에서 개발하여 오픈소스로 오픈되어있으며 azure, aws등의 벤더와 종속되지않고 클라우드를 provisioning할 수 있는 장점이 존재한다.

### terraform의 장점

- terraform은 infra를 빠른 시간내에 재사용가능한 형태로 형상을 구축할 수 있는 장점이 존재한다.
- cloud의 특성상 콘솔로 접근한다면 의도를 알기 어렵고 누가 했는지를 알기 어려울 수 있는데 코드로 문서화하여 인프라를 한눈에 알아볼 수 있게 할 수 있다.

### terraform 핵심 개념

`provider`
- Terraform이 외부 시스템(AWS, Azure, GCP, SaaS, 온프렘 API 등)과 통신할 수 있게 해 주는 플러그인 단위입니다.
- provider "aws" { ... }처럼 설정하면, 해당 provider가 VPC, Route Table, VPN, ALB 같은 AWS 리소스 타입과 API 호출 방법을 제공합니다.

`resource`

- 실제 인프라 객체 한 개를 나타내는 선언 단위입니다. ( ex: aws_vpc, aws_subnet)
- resource "<TYPE>" "<NAME>" { ... } 형식으로 쓰고, 어떤 타입을 쓸 수 있는지는 provider가 결정합니다.

`state`

- “이 Terraform 설정의 각 resource 인스턴스가 실제로 어떤 원격 객체(예: AWS 리소스 ID)와 연결되어 있는지”를 저장하는 스냅샷 파일이다. 일반적으로 terraform.tfstate라는 JSON 파일로 관리됩니다.
- Terraform은 매번 코드 vs state vs 실제 인프라를 비교해서, 어떤 리소스를 생성/변경/삭제할지 계산한다. 이 매핑이 없으면 Terraform이 정상적으로 동작할 수 없습니다.

`backend`

- state 데이터를 어디에, 어떤 방식으로 저장할지를 정의하는 설정 블록입니다.
- root module 안의 terraform { backend "<TYPE>" { ... } }에서 정의하며, 예를 들어 backend "s3"를 쓰면 state를 S3 버킷에 저장하고, 필요하면 DynamoDB 락을 써서 동시에 둘이 apply하는 상황을 막을 수 있습니다.

`remote state`

- 의미 1: state 파일을 로컬이 아니라 S3/GCS/HCP Terraform 같은 원격 저장소에 두고, 여러 실행/여러 사람/여러 구성에서 공유하는 패턴 전체를 가리킵니다.
- 의미 2: 다른 Terraform 구성에서, 특정 state의 root module outputs를 읽어오기 위해 쓰는 data "terraform_remote_state" "..." { ... } data source를 가리킬 때도 사용합니다,

`module`

- 하나의 디렉터리 안에 있는 .tf 파일 묶음 전체를 module이라고 부릅니다.
- 간단한 구성도 “root module 1개”인 module이고, 여기서 module "vpn_onprem_a" { source = "./modules/vpn_onprem" ... }처럼 child module들을 호출해서 VPC/VPN/ALB 같은 패턴을 재사용할 수 있게 됩니다.

`variables (input variables)`

- variable "name" { ... } 블록으로 선언하는 입력 인터페이스 입니다.
- 루트/child module 어디에나 선언할 수 있고, 실제 값은 *.tfvars, CLI -var, 환경 변수(TF_VAR_...), 상위 module의 module "..." { some_var = ... }에서 주입됩니다.

`output (output values)`

- output "name" { value = ... } 블록으로 선언하며, **모듈 실행 결과 중 “밖으로 보여주고 싶은 값”**을 정의합니다.
- terraform apply 후 CLI에 출력되고, remote state를 통해 다른 구성에서 읽어갈 수 있습니다.






출처: https://bluedreamer-twenty.tistory.com/14 [성장하는 개발자:티스토리]
