## 가상머신(Virtual Machine)

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643994-e2021fc1-1056-4c42-9489-4fcd422b464c.png">

하나의 하드웨어(CPU, Memory등)에 다수의 운영체제를 설치하고, 개별 컴퓨터 처럼 동작하도록 하는 프로그램  
하드웨어를 소프트웨어로 에뮬레이터하여(모사) 마치 여러개 처럼 보이도록 하는 기술

---
### 가상머신 종류
#### Virtual Machine Type1(베어메탈 하이퍼 바이저(Hypervisor)형 가상화

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643826-93e3aabb-9dac-4704-a1cc-a0c66351d72b.png">

- 하이퍼 바이저(VNM) : 운용 체제와 응용 프로그램을 물리적 하드웨어에서 분리하는 프로세스
- 하이퍼 바이저 또는 벌츄얼 머신 모니터(VNM) 라고 하는 소프트웨어가 HardWare에서 직접 구동<br>
-  **TYPE1(Bare Metal, KVM(AWS))** 하드웨어 위에 VNM을 설치하여 복수의 가상머신을 실행하는 방식<br>
-  **Type1예** Oracle VM server X86 VM ware의 ESX/ESX i server 마이크로소프트-> Hiper-V , Virtual Iron 등등 <br>


#### Virtual Machine Type1 - 전가상화란(Full - Virtualization?

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643815-209208d7-6a21-4999-b45a-de7542d935bc.png">

- 하드웨어를 가상화 하는 방식. 하이퍼 바이저를 베어메탈에 구동하면, DOMO라고 하는 관리형 가상머신이 구동됨
- 전가상화는 나머지 모든 가상머신들의 하드웨어 접근이 DOMO를 통해서 이뤄져 모든 명령에 DOMO가 개입하게 되는 형태를 띄게된다.<br>
- > > 하드웨어를 완전히 가상화하여 별다른 게스트 운영체제의 수정없이 사용할 수 있다는 장점이 있다. 
- > > 하이퍼 바이저가 모든 명령을 중재하여 성능이 비교적 느리고 특정 인스트럭션의 경우 하이퍼 바이저가 반드시 처리해줘야 하며 트랩처리를 해야하는 단점이 있다.

#### Virtual Machine Type1 - 반가상화란(Para - Virtualization)?

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643819-d513201b-4e3a-4a29-b51d-65f9387a128e.png">

 - 일반적으로 TYPE1은 터프라이즈 데이터 센터와 서버 기반 환경에서 가장 일반적으로 사용됩니다.
 - 하드웨어를 완전히 가상화 하지 않으며 전가상화의 단점인 성능 저하를 해소하고자 DOMO를 통해 하이퍼바이저에게 요청을 보내지 않는다
 - 하이퍼 콜(Hyper-Call)이라는 인터페이스로 하이퍼 바이저에게 직접 요청을 날리는 방식이다.
 - > > 전가상화에 비해 성능이 빠르다는 장점이 있다.
 - > > 하지만 하이퍼 콜을 이용한 요청은 기존OS에선 할 수 없으며 게스트 운영체제가 하드웨어의 가상여부를 알고 있어야 한다.
 - > > 필요한 경우 하이퍼 바이저에게 하이퍼 콜을 요청하도록 운영체제의 커널을 수정해야 하며 오픈소스 운영체제가 <br>아니라면 반가상화를 이용하기 쉽지 않다는 단점이 있다.

#### Virtual Machine Type2(호스트형 가상화)

<img width="690" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643813-f09229ac-858c-4457-b3c8-e5e0e3611395.png">

- TYPE2는 기존의 운영 체제에서 소프트웨어 레이어 또는 애플리케이션으로서 구동됩니다.
- 하이퍼 바이저 또는 버츄얼 머신 모니터(VNM)라고 하는 소프트웨어가 Host OS 상위에 구동<br>
- **TYPE2(VMWare)** OS 위에 하나의 application처럼 VMM을 설치하여 사용하는 방식
- > > software ex) VM ware - VMware Workstation VMware Player , Microsoft - Virtual server 2005 등이 있다.
- > > 기존 운용체제를 사용하는 시스템에 손쉽게 가상 머신을 설치할 수 있다는 특징이 있다.
- > > 호스트 운영체제 위에 하이퍼 바이저가 구동되고 가상의 하드웨어를 에뮬레이팅 해서 오버헤드가 크다는 단점이 있다.  
- > > 가상의 하드웨어를 에뮬레이팅 하기 때문에 호스트 운영체제에 크게 제약이 없다는 장점이 있다. <br> 리눅스에서 윈도우를 윈도우에서 유닉스를 구동하는 등

##### 하이퍼 바이저란?
- 호스트 컴퓨터에서 다수의 운영 체제(operating system)를 동시에 실행하기 위한 논리적 플랫폼(platform)을 말한다
- 가상 머신 모니터라고도 불리며 가상 머신(VM)을 생성하고 실행하는 프로세스이다.
- 하이퍼 바이저는 메모리 및 처리와 같은 단일 호스트 컴퓨터의 리소스를 가상으로 공유하여 호스트가 여러 게스트 가상 머신을 지원할 수 <br>있도록 하는 것이다.
 - 하이퍼 바이저로 사용되는 물리 하드웨어를 **호스트** 리소스를 사용하는 여러 VM을 **게스트**라고 한다.
##### 베어 메탈(하이퍼 바이저의 첫번째 유형)

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643822-0c348f42-33e3-40c2-8ac1-2909e8a">

- 호스트의 하드웨어에서 직접 실행됨

##### 호스팅(하이퍼 바이저의 두번째 유형)

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643830-2cade862-0ba1-4e37-a2ba-c6224a2a86fe.png">

- 다른 컴퓨터 프로그램처럼 운영 체제에서 소프트웨어 계층으로 실행됨 

##### 하이퍼 바이저를 사용해야 하는 이유?
- 게스트 가상 머신이 호스트 하드웨어와 독립되어 시스템의 가용 리소스를 더 많이 활용하고 IT 모빌리티를 향상시킬 수 있음 <br> 이를 통해 여러 가상머신을 여러 서버 간에 쉽게 이동시킬 수 있다.

---
## JVM 이란?
- Java Virtual Machine의 줄임말로서 java는 OS에 종속적이지 않는 특성을 지니고 있는데 OS에서 종속 받지 않게  <br> OS위에서 java를 실행시키는 프로그램이 JVM이다.

<img width="800" height="350" alt="image" src="https://user-images.githubusercontent.com/98382954/180643832-c7885c9b-d5be-4f3f-87f9-40fd642061a4.png">

- java의 소스코드, 원시코드(.java)는 CPU가 인식을 하지 못하므로 기계어로 컴파일을 해줘야 하는데 <br> java는 jvm을 이용하여 os에 전달하기 때문에 jvm이 인식할 수 있는 java bytecode(*.class)로 변환된다

- java complier 가 .java를 .class로 변환시켜 주는데 여기서 java compiler는 JDK를 설치하면 bin에 존재하는 javac.exe를 의미한다 
- 여기서 .class는 기계어가 아니기 때문에 os에서 실행이 불가하며 JVM이 OS가 bytecode를 이해할 수 있도록 해석해준다 <br> 따라서 bytecode는 JVM위에서 OS에 상관없이 실행될 수 있는 것이다. 그래서 java 파일 하나만 만들면 어느 OS에서든 JVM을 설치하여 <br>실행이 가능해진다.


### Reference
<https://dora-guide.com/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%B0%94%EC%9D%B4%EC%A0%80><br>
<https://velog.io/@walker/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-14.-%EA%B0%80%EC%83%81%EB%A8%B8%EC%8B%A0><br>
<https://blog.naver.com/complusblog/220990379931><br>
<https://sc.greenart.co.kr/community/greenDesignNews_view?idx=1679><br>
<https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80>
