### Sonarqube란?
- `소나큐브(SonarQube, 이전 이름: 소나/Sonar)`란 는 20개 이상의 프로그래밍 언어에서 버그 , 코드 스멜 보안 취약점을 발견할 

   목적으로 정적 코드 분석으로 자동 리뷰를 수행하기 위한 지속적 코드 품질 검사용 오픈 소스이다.
- `소나소스(SonarSource)`에서 개발했으며 중복 코드 , 코드 표준 , 유닛 테스트 , 코드 커버리지 , 코드 복잡도

   주석 , 버그 및 보안 취약점의 보고서를 제공하는 오픈 소스 및 정적 코드 분석 도구이다.
   
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/227702017-f6350bef-b02b-4739-88f5-083ca8f9c4a3.png">

 - 소나큐브는 6개의 분류로 코드를 분석해 유지 보수성 , 보안 취약점등을 등급으로 나눠 분석해주는 도구이며 상단의 사진처럼 확인해볼 수 있다.

#### Sonarqube와 같은 정적 코드 분석 도구를 사용하는 이유?
개발자들이 코드의 신뢰성 , 생산성을 높이는 방법으로 `TDD` 를 사용하지만 TDD가 그 자체로 코드의 품질을 보장하진 못한다.

##### 코드의 품질이란?
- 소프트웨어 상 기능 * 구조 상의 품질이라 정의할 수 있다.
1. `기능 상의 품질`
   1. 기능 요건 , 사양에 기반해 주어진 설계를 얼마나 잘 반영하고 있는지를 의미한다.
   2. 소프트웨어의 목적을 부합하는 코드인지 기능을 반영한 코드가 경쟁력이 있는지를 의미한다. 
2. `구조 상의 품질`
   - 소프트웨어 즉 기능이 올바르게 개발될 수 있는지를 가늠하는 척도로서 **내구성**이나 **유지보수성**을 의미한다. 

> 위와 같은 두가지의 기준을 만족하는 코드는 곧 신뢰도가 높은 코드로 인식된다.

`신뢰성`
   - 높은 품질의 코드로 개발된 기능의 경우 기능이 동작하는데 있어 더 많은 상황에서 잘 동작하는 것을 기대하며 기능이 더 안전하게 동작한다는 것을 보장할 수 있게 된다.

`경제성`
   - 높은 품질의 코드의 경우에도 이슈가 발생할 수 있으며 코드 수정의 경우 더 적은 비용으로도 코드를 유지보수 할 수 있게 된다.

#### 코드의 품질을 높이기 위해선?
1. `코딩 컨벤션`
   - 협업하며 코드를 개발하는 경우 동일한 규칙을 가지고 작성하는 것을 의미한다. (클래스,메서드,네이밍 규칙 등)
2. `코드 리뷰`
   - 작성한 코드에 관해 개발자간 리뷰를 통해 오류를 검출하고 수정하거나 사이드 이펙트를 예측 , 보안 취약점등을 

     파악해 개선하는 활동을 의미하며 PR을 통해 진행하거나 미팅을 통해 진행한다.
3. `단위 테스트`
   - 각 메서드 단위로 테스트 코드를 작성 구현 메서드가 기능적으로 문제가 없는지 주어진 설계를 충족하는지 실행하며

     오류존재여부를 검출하고 수정하는 행위를 의미한다.
   - 테스트 과정에서 오류를 검출해 설계상의 이슈를 해결하는 과정이며 중요한 활동이기 때문에 테스트 코드 , 테스트

     커버리지가 적은 경우 이슈를 검출하지 못하기 때문에 테스트 코드는 신중하게 작성해야 한다.
   > TDD를 지향하여 코드를 작성하는 경우 3번의 활동을 진행하고 있는 것이다.

4. `코드 커버리지`
   - 단위 테스트 시 코드의 각 라인이 몇번 실행되는지 분석하는 것을 의미하며 이를 통해 해당 단위 테스트가 실제 

     기능의 코드 범위를 파악해 취약점을 발견 및 보완할 수 있게 한다.

   - 코드 커버리지가 충분하지 않다면 기능이 동작하는데 영향을 주는 요소를 충분히 파악하지 못하기 때문에 개선하여
   
     좀 더 견고한 테스트 코드를 작성할 수 있게 만들어준다.
     
5. `정적 코드 분석`
   - 소스 코드 , 바이너리 파일등을 분석해 잠재적 결함을 찾아내는 활동이다.
   - 문법 , 묵시적 형 변환 , 성능 등 개발자가 놓칠 수 있는 부분들을 분석 및 제공해 개선할 수 있도록 도와주며 
   
     컴파일 단계에서만 파악하기 때문에 런타임시의 동적인 부분까지는 검출하지 못하므로 사이드 이펙트는 다른 활동을 통해
     
     보완하는 등 다른 활동을 필수적으로 병행해 진행해야 한다.
6. `중복 코드 제거`
   - 코드내 중복 코드를 제거 추후 발생하는 유지보수 상 문제점을 방지하는 활동이다.
   - 코드를 수정하는 경우 중복 코드를 전부 검출해 동일하게 맞춰야 하며 이때 예상하지 못한 이슈를 검출해 내는 등

     문제가 발생할 수 있기 때문에 코드의 일관성과 안전성을 보장하며 생산성을 높이는 활동이라고 할 수 있다.
```
 신뢰성을 높이기 위해 6개의 활동을 진행해 코드의 품질을 높이는 것은 개발자의 필수 덕목이지만 코드의 품질을 파악하고 향후 유지 보수성
 보안 취약점을 파악해 진행하며 개발에도 집중하는 것은 현실적으로 매우 힘들다.
 이런 과정들을 자동화하여 진행해야 하는데 소나큐브와 같은 정적 코드 분석 도구는 자동화 하여 관리하고 각 부분별 등급 , 레포트를 보여주기 때문에 
 코드 리뷰의 생산성을 높여주고 코드의 품질을 높이는 시간을 앞도적으로 단축하고 자동화하도록 도와주기 때문에 사용해야 한다.
```

##### sonarqube는 코드 커버리지 분석을 진행할 수 없기 때문에 Jacoco같은 코드 커버리지 분석 도구를 활용해 진행해야 한다.
[Jacoco설정법](https://github.com/alstjq8251/Cs-tech/blob/main/%EA%B0%9C%EB%85%90/devops/jacoco.md)
