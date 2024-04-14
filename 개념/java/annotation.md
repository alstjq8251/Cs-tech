## Annotation이란?
- 주석처럼 프로그래밍 언어에 영향을 미치지 않으며, 유용한 정보를 제공

### Annotation이 나오게 된 배경
- 소스코드와 개발 문서가 존재할 때 코드 변경과 문서 최신화가 이뤄지지 않아 불일치가 발생하게 되어
  문서 내용을 주석으로 만들게 되었다.
- 해당 내용을 javadoc.exe가 주석을 추출해 문서를 만들어 코드와 개발 문서간의 버전 차이를
  해결했었다.
- 마찬가지로 소스코드(.java)와 설정 파일(.xml)파일로 관리할 때 파일을 관리하는 어려움이 있어
  주석을 해결했던 방식처럼 설정에 대한 정보를 @Test와 같은 에너테이션에 넣게 된 것이다.

### Annotation의 장점
- 기존 문법을 변경하지 않고도 ex(Junit)이라는 특정 프로그램이 필요한 정보(설정정보)제고을 위한
  것
- 기존 설정파일은 파일을 공유해야 하는 단점이 있지만 Annotation은 각자 필요한 메타정보만
  추가하면 되는 편리함이 있다.

#### 사용 예

``` java
    @Test // 이 메서드가 테스트 대상임을 테스트 프로그램에게 알린다.
    public void init(){}
```

### 표준 Annotation
- Java에서 제공하는 Annotation

| 애너테이션 | 설명 |
| :--: | :--: |
| @Override | 컴파일러에게 오버라이딩하는 메서드라는 것을 알린다. |
| @Deprecated | 앞으로 사용하지 않을 것을 권장하는 대상에 붙인다. |
| @SuppressWarnings | 컴파일러의 특정 경고메시지가 나타나지 않게 해준다. |
| @SafeVarargs | 지네릭스 타입의 가변인자에 사용한다.(JDK1.7) |
| @FunctionalInterface | 함수형 인터페이스라는 것을 알린다.(JDK1.8)
| @Native | native메서드에서 참조되는 상수 앞에 붙인다.(JDK1.8) |
| @Target* | 애너테이션이 적용가능한 대상을 지정하는데 사용한다. |
| @Documented* | 애너테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다. |
| @Inherited* | 애너테이션이 자손 클래스에 상속되도록 한다. |
| @Retention* | 애너테이션이 유지되는 범위를 지정하는데 사용한다. |
| @Repeatable* | 애너테이션을 반복해서 적용할 수 있게 한다.(JDK1.8) |

> 자바에서 기본적으로 제공하는 표준 애너테이션(*가 붙은 것은 메타 애너테이션)
> 메타애너테이션 - 애터네이션을 만들 때 사용

**javac.exe가 사용하는 애너테이션**

1. @Override
- 오버라이딩을 올바르게 했는지 컴파일러가 체크하게 한다.
- 오버라이딩 할 때 메서드 이름을 잘못적는 실수를 하는 경우가 많다.
  - <details>
    <summary><strong>오버라이딩의 잘못된 예와 붙여야 하는 이유</strong></summary>
    <div>
    
    - class Parent{ void parentMethod() {} }
    - class Child extends Parent { void parentmethod() {}}
    - 위 같이 오버라이딩을 하려했으나 실수하는 경우 컴파일러가 오류를 잡아낼 수 없기 때문에 
  
      컴파일러에게 오버라이딩 한다고 알리고 컴파일 단계에서 에러를 검출하는 것이다.
    
    </div>
    </details>
- 오버라이딩할 때는 메서드 선언부 앞에 @Override를 붙이자.

2. @Deprecated - 컴파일러가 사용하는 애너테이션
- 앞으로 사용하지 않을 것을 권장하는 필드나 메서드에 붙인다.
- @Deprecated의 사용 예, Date클래스의 getDate()
- @Deprecated가 붙은 대상이 사용된 코드를 컴파일하면 나타나는 메시지
- <details>
  <summary><strong>컴파일 했을때 나오는 메시지</strong></summary>
  <div>

  - Note: AnnotationEx2.java uses or overrieds a deprecated API.
  - Node: Recompile with -Xlint:deprecation for details.
    - javac -xlint:deprecation AnnotationEx2.java ->
    - AnnotationEx2.java:21: warning: [deprecation] oldField in NewClass has been deprecated
    - nc.oldField = 10;

  </div>
  </details>
- java는 하위 호환성을 중요하게 여겨 jdk 전 버전들에서 사용하는 메서드가 장애가 나지 않게 끔 만들지만
 
  향후 해당 메서드를 사용하지 않도록 권장하기 위해서 사용하는 애너테이션이다.
3. @FuntionalInterface - 컴파일러가 사용하는 애너테이션
- 함수형 인터페이스에 붙이면, 컴파일러가 올바르게 작성했는지 체크
- 함수형 인터페이스에는 하나의 추상메서드만 가져야 한다는 제약이 있음
  - <details>
    <summary><strong>함수형 인터페이스의 예</strong></summary>
    <div>

    - @FunctionalInterface
      
      public interface Runnable { public abstract void run(); | // 추상 메서드

    </div>
    </details>
4. @SuppressWarnings
- 컴파일러의 경고메시지가 나타나지 않게 억제한다.
- 괄호()안에 억제하고자하는 경고의 종류를 문자열로 지정
- ```java
    @SuppressWarnings("unchecked") // 지네릭스와 관련된 경고를 억제
    ArrayList list = new ArrayList(); // 지네릭 타입을 지정하지 않았음.
    list.add(obj); // 여기서 경고 발생
  ```
- 둘 이상의 경고를 동시에 억제하려면 다음과 같이 해야 한다.
  - @SuppressWarnings ({"deprecation","unchecked"})