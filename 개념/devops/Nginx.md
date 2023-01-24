### Nginx란?
```
Nginx는 경량 웹 서버이자 리버스 프록시 서버로 활용된다.
고성능 , 안정성 및 낮은 자원 소비로 유명하며 로드 밸런서 , 리버스 프록시 및 HTTP 캐시로도 사용된다.
클라이언트로부터 요청을 받았을 때 요청에 맞는 정적 파일을 응답해주는 HTTP Web Server로 활용되기도 하고,
Reverse Proxy Server로 활용하여 WAS 서버의 부하를 줄일 수 있는 로드 밸런서로 활용되기도 한다.
```

#### Nginx가 나오게 된 배경
- Nginx와 같은 웹서버들은 apache , lls 등이 존재하며 Nginx이전까진 Apache웹 서버가 대중적으로 이용되었다.
- `Nginx`는 `Apache`가 갖고 있던 단점을 보완하며 나오게 되었는데 `C10k`라는 문제점을 해결하며 나오게 되었다.

#### apache의 구조와 C10K?

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214032014-2892314b-5ff5-4014-811a-e2f842560ba2.png">

- apache는 상단의 사진과 같이 unix의 os가 네트워크 커넥션을 생성하는 방식과 유사하게 요청하나에 프로세스 하나를 할당하는 방식으로 처리한다.
  - process를 생성하는 비용은 부하가 상당하기에 미리 생성해서 할당하는 `PREFORK`방식으로 요청을 처리했다.
  - 허나 PREFORK방식의 프로세스가 모두 할당되고 나면 새로운 요청은 새로 Process를 만들어 처리하게 되며 유저의 요청을 동적으로 처리하기에 적합했다.
    - 이를 MPM(Multi Proccess Module)이라 한다. 
  - 이렇게 요청 하나에 프로세스 하나씩을 할당하는 방식을 `Process-Driven`방식이라 한다.

**C10K란?**
```
Concurrent 10,000 clients/connections의 줄임말로 10000개의 요청을 동시에 처리하는 것을 의미한다.
```
      
#### Apache의 단점

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214029072-4cbb68ae-1264-49de-be94-895cd1c86705.png">

- 상단의 사진처럼 C10K의 문제인데 10000개 정도의 많은 요청이 몰리면 처리하기가 어렵다는 단점이 존재한다.
  - 10000개 정도의 요청이 들어오면 처리가 힘들어진다고 한다.
- 기술의 발전해감에 따라 하드웨어 성능이 향상되어 1만개 이상의 요청을 받을 수 있었으나 Apache의 내부구조의 단점 때문에 이를 처리하기가 어려웠다.
  - 쉽게 오버라이딩하여 개발자들마다 설정을 추가하고 바꾸는등 모듈을 쉽게 얹을 수 있는 장점에 비해 프로세스가 무거웠으며 이러한 프로세스를 

    각 connection마다 생성하니 자원이 쉽게 고갈되고 메모리가 감당하지 못해 많은 요청을 다중 제어 , 동시 제어하지 못하는 단점이 존재했다.
    
    - 이로 인해 MultiProcess -> MultiThread로 방식을 바꾸거나 PREFORK의 설정을 바꾸는 등 성능 향상을 하며 발전하긴 했으나 근본적인 구조는 바뀌지  
    
      못해 다중 커넥션 제어에는 여전히 약한 모습을 보인다.
  - Apache는 프로세스가 각 요청마다 할당되는 방식때문에 수많은 프로세스가 cpu를 컨텍스트 스위칭하는 비용도 많이 발생했다.

##### CPU 컨텍스트 스위칭이란?
- CPU 컨텍스트 스위칭은 하단의 링크를 들어가 확인이 가능하다.

- <>

##### Multiprocess, MultiThread의 차이?
- Multiprocess와 MultiThread의 차이는 기술했던 하단의 링크를 들어가 확인이 가능하다.

- <https://github.com/alstjq8251/Cs-tech/tree/main/%EA%B0%9C%EB%85%90/OS>

#### Nginx의 역할
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214041177-2a0347b5-9554-4c91-995f-694da33adb6c.png">

- Nginx는 기존의 Apache가 다중 커넥션을 제어하지 못하는 단점때문에 Apache 앞단에서 요청을 받아 커넥션을 줄여주려는 목적에서 나오게 되었다.
  - 여러 WAS들에게 트래픽을 분산시켜주는 로드밸런서 역할을 수행하는 것을 의미하는데 로드밸런서란 트래픽을 분산시켜주는 주체를 의미하고 

    행위를 로드밸런싱이라 한다. 
  - 다중 커넥션들을 빠르게 처리하기 위한 목적에서 요청을 앞단에서 받아주기 위해 나오게 된 배경으로 프록시 서버 역할이 가능하다.
    - 프록시란 대리자라는 뜻을 가지는데, 일반적인 프록시는 클라이언트가 누군지를 숨겨주는 역할을 한다. 즉 서버에선 클라이언트가 누구인지 식별할 수 없다.
    - 리버스 프록시란 반대로 클라이언트에서 서버가 누구인지 식별할 수 없다. 단지 서버를 대리하는 존재만을 알게 된다. 서버가 누군지를 숨겨주는 역할을 한다.
    - 요청하는 클라이언트에서는 서버에 직접 요청하는게 아니라 Nginx 서버에 요청을 하므로 실제 요청을 처리하는 서버는 알 수 없으며 보안성이 향상된다.

> 리버스 프록시 및 포워드 프록시 등 Nginx의 역할들은 하단에서 추후 기술한다.

#### Nginx의 구조?
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214029449-09724352-2d1d-4693-a7ff-08436fcd4799.png">   

```
Nginx는 하나의 Master Process와 다수의 Worker Process로 구성되어 실행된다(멀티프로세스 , 싱글 쓰레드). 
Nginx는 설정파일을 읽고 워커 프로세스를 생성하는 역할을 하는 마스터 프로세스와, 실제로 유저 요청을 처리하는 워커 프로세스로 이루어져 있다.
모든 요청은 Worker Process에서 처리합니다. Nginx는 이벤트 기반 모델을 사용하고, Worker Process 사이에 요청을 효율적으로 분배하기 위해 OS에 의존적인 메커니즘을 사용한다.
Worker Process의 개수는 설정 파일에서 정의되며, 정의된 프로세스 개수와 사용 가능한 CPU 코어 숫자에 맞게 자동으로 조정된다.
Nginx가 구동될 때 Master Process는 정해진 수만큼 Worker Process를 생성하고 리슨 소켓(워커 프로세스와 통신할 수 있는 소켓 디스크립터)을 배정하게 된다.
```

#### Nginx의 Event Driven?

- Nginx에선 TCP,UDP의 connection과 Http Request 처리, Connection의 종료까지의 모든 절차를 `이벤트` 라는 개념으로 취급하고 처리한다.
- Worker Process에게 working queue라는 이름의 처리해야할 작업이 순차적으로 담긴 큐를 처리하도록 하며 끊임없이 이벤트를 처리하게 된다.
  - Apache에선 프로세스가 많아짐에 따라 keep alive의 상황에서 커넥션이 종료되지 않았을 때 자원이 낭비되는 것과 대조적인 모습인데
  
    반대로 이땐 이벤트의 처리에 따라 Blocking이 될 수 도 있다.
      - 동기 방식으로 DB의 응답을 처리하는 경우
      - 라이브러리 함수 호출등의 경우
      - 이벤트 처리가 오래 걸리는 경우
  - 이로인해 Nginx는 Thread Pool이라는 개념을 도입하게 되었다.

#### Nginx의 Thread Pool

`Thread Pool`은 생성비용이 비싼 스레드를 필요할 때 편하게 늘리기 위해 스레드를 미리 만들어놓고 필요한 작업에게 할당해주는 개념이다.

- 하나의 Worker Process가 모든 요청을 처리한다는 것은 요청을 처리하는 속도가 지연됨에 따라 다른 요청들이 block되는 것을 의미하는데 

  Nginx에선 이것을 방지하기 위해 오래 걸리는 작업(Disk 읽기, 파일 전송하기)을 처리하는 Thread Pool을 따로 만들어 놓았다.(1.7.11버전 이후) 

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214042020-a9faca9e-b39f-405c-a5bf-db1c71b1accb.png">

- Worker Process가 요청을 Task Queue에 넣으면 각 요청을 Thread Pool 에 있는 Thread들이 처리해 수행 결과를 다시 Worker Process에게 전달한다.

> Thread pool 개념을 도입했다고 해서 기본적으로 적용되지 않는다.<br>
> 1.7.11 버전 이상에서 별도의 설정이 필요하다는 점에 유의하자. (적용 방법은 공식문서에 나와있다.)

#### Nginx의 Worker Process구조
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214043855-4e74be1d-7c71-49a4-a0d6-40c658b669cd.png">

`Master Process` 
  - 설정을 읽거나 포트에 바인딩을 하는 권한이 필요한 작업을 수행하고 3개의 Child Process를 생성한다.
`Cache Load Process` 
  - 서버 시작 시 실행되고, 디스크의 캐시를 메모리에 올려놓는 역할을 한다.
`Cache Manager process` 
  - 주기적으로 실행되고, 캐시에서 항목을 정리하여 정해진 크기 내에서 유지한다.
`Worker Process` 
  - Nginx로 들어오는 모든 요청을 처리한다.
  - 네트워크 연결 처리 , 디스크에 Write처리 ,  Upstream 서버와 통신하는 작업을 처리한다.
  - Worker Process는 싱글 스레드로 동작하며 Non-Blocking 방식으로 여러 요청을 처리하며 CPU 컨텍스트 스위칭 비용을 최소화한다.

##### Nginx가 CPU 컨텍스트 스위칭 비용을 최소화 하는 방식
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214043427-d6374116-1641-433a-94f9-bdd26ad4cd43.png">

- Nginx는 각 프로세스마다 하나의 코어를 이용하도록 배정하기 때문에 CPU에선 컨텍스트 스위칭하는 비용이 발생하지 않게되어 부하가 최소화 되게 된다.

#### Nginx의 장점
- 동일한 자원을 가지고 처리하는 커넥션 양이 아파치와 비교하여 최소 10배 이상 증가(일반적으론 100~1000배까지 증가)
- 동일 커넥션 수로 통신 시 2배 이상의 처리 속도 향상
  - Nginx는 비동기적으로 한번에 모든 요청을 처리하기 때문(asynchronous)
  - Non-Blocking방식으로 Blocking되어 프로세스의 처리 속도가 지연되는 것을 방지한다.
- 효율적인 설정 갱신
  - Nginx는 서버가 동작하는 와중에도 설정을 변경하는 것이 가능하다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214049549-48b06d25-5944-4d34-a5bb-519b90ccbf34.png">

- 보통의 서버 프로그램들(Apache)등은 설정이 바뀐 경우 프로그램을 재구동 시켜야 하지만 Nginx는 `Event-Driven`방식으로 처리하기 때문에 

  서버가 동작하는 와중에도 설정을 변경할 수 있다. 
  - service reload, docker exec <name> nginx service reload (docker 기반)
  - 상단의 사진과 같이 설정을 바꾸고 명령어를 실행하면 Master Process는 설정을 읽어 새로운 워커 노드를 생성한다.
    
  - 기존의 워커 프로세스(초록색)로는 새로운 이벤트를 할당하지 않고 기존의 작업이 모두 끝나면 새로운 워커 프로세스(노란색)에 이벤트를 전달하고
  
    기존의 워커 프로세스를 종료하는 방식으로 동작하는 와중에도 설정이 바뀌었을때 Nginx는 바로 적용이 가능하다.

#### Nginx의 단점
- Nginx는 기본적으로 Web Server이기 때문에 동적 컨텐츠를 처리할 수 없다.
  - 동적 컨텐츠를 처리해야 하는 경우 Web Application Server(was)와 같이 사용해야 한다.
- Nginx는 서버 구성을 아직 개발자가 자유롭게 커스텀하기엔 제약이 있다.
  
#### Nginx와 Apache의 차이
  
| Apache | NginX |
|:---|:---|
요청 당 스레드 또는 프로세스가 처리하는 구조	 | 비동기 이벤트 기반으로 요청
CPU/메모리 자원 낭비 심함	| CPU/메모리 자원 사용률 낮음
NginX보다 모듈이 다양	| Apache에 비해 다양한 모듈이 없음
PHP 모듈 등 직접 적재 가능	| 많은 접속자들 대응 가능
안정성, 확장성, 호환성 우세	| 성능 우세
동적 컨텐츠 단독 처리 가능	| 동적 컨텐츠 단독 처리 불가능
  
##### Apache는 이제 Nginx로 모두 바뀌는 것인가?
  
- Apache는 안정되지 않는 웹서버 시장을 해결하기 위해 나온것인 만큼 안정성 , 호환성이 매우 뛰어나며 다양한 모듈을 이식할 수 있다.
- Nginx는 속도처리의 향상 Apache는 안정성 , 호환성등으로 각 어플리케이션을 구동하는 환경(트래픽)에 따라 적절히 사용해야 한다.

##### Apache의 장점은?
1. `OS 안정성`
  - Apache는 Window에서도 Unix와 동일한 성능을 보장한다.(Nginx는 Window를 완벽하게 지원하지 않는다.)
2. `디렉토리별 추가 구성`
  - .htaccess 파일을 통해 디렉토리별로 추가 서버 구성을 하는 것을 허용한다.
    - 메인 아키텍처는 수정하지 못하는, 특정 페이지만 관리하는 관리자 권한을 만들 수 있으며 각 디렉토리 별로 환경 설정을 따로 구성할 수 도 있다.
    - Nignx는 성능 향상을 위해 추가 구성을 허용하지 않으며 파일을 검색하는 비용 , 커스텀한 설정파일들을 읽지않기 때문에 좀 더 빠르다
3. `동적 모듈 지원`
  - Apache는 Apache가 지원하는 모듈별로 서버 구성을 자유롭게 커스텀 할 수 있는 유연성을 갖추고 있다.

### Nginx가 수행하는 기능
1. `Forward Proxy`

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/214060288-7e59f1ba-1a19-40e0-9e52-43c55d937c43.png">

- Forward Proxy로 사용하는 경우는 클라이언트의 요청이 직접적으로 전송되지 않고 중개자인 Proxy Server를 거쳐 요청이 전송되는 것을 의미한다. 
- 정해진 사이트의 요청만을 연결하도록 제한할 수 있어 보안이 중요한 경우 사용된다.
- Nginx는 성능 향상을 위해 나온 것인만큼 원래는 Reverse Proxy만 지원하나 ngx_http_proxy_connect_module 설정을 추가해야 가능하다.
  
> 포워드 프록시서버를 사용하는 경우 클라이언트의 정보를 감출 수 있고 대개 캐싱 기능을 사용할 수 있어 요청*응답간 성능 향상을 시킬 수 있지만<br>
> Nginx는 고성능 웹 서버로 사용하며 대개 Reverse Proxy서버로 사용하기에 추가적인 정보는 기술하지 않는다.

2. `Reverse Proxy`
  
<img width="1106" alt="image" src="https://user-images.githubusercontent.com/98382954/214062375-1560ddd2-75a8-4b6d-80ae-04cc74f8c90a.png">
  
- 클라이언트의 요청을 받아 내부 서버로 전달해주는 방식을 의미한다.
  - 호출한 Ip를 알아낸다고 해도 내부적으로 전달하는 IP를 알 수 없어 서버를 숨겨주는 역할을 한다.
  - 이러한 점들로 `보안` , `Load Balancing`의 역할을 수행할 수 있게 된다.
    - 프록시 서버를 거쳐 내부 서버로 접속해야 하기 때문에 공격시 2개의 서버를 모두 뚫어야만 가능하므로 보안이 한층 강해진다.
    - 요청이 왔을 때 내부 서버로 전달해주는 방식때문에 여러가지 방식을 정해 각 서버들로 트래픽을 분산시켜 줄 수 있게 되며 
      내부 서버를 유연하게 증가 시킬 수 있게 되는 장점을 갖게 된다.

3. `SSL termination 지원`

```
server {
    listen 80;
    server_name test-nexus.domain.com fail_timeout=0;
    return 301 https://test-nexus.domain.com$request_uri;
}

server {
    server_name  test-nexus.domain.com;
    listen 443 ssl;
    server_name_in_redirect off;
    proxy_set_header Host $host:$server_port;

    ssl_certificate /etc/nginx/certs/nexus-test/test-nexus.domain.com.cer;
    ssl_certificate_key /etc/nginx/certs/nexus-test/test-nexus.domain.com.key;
    ssl_session_timeout         5m;
    ssl_protocols                       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers                         HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers   on;
```
  
  - Nginx는 요청이 왔을때 내부적인 서버 앞단에서 요청을 받아 분산시켜 주는 역할을 하기 때문에 이때 받아주는 요청과 응답을 
  
    Nginx에서 SSL인증서 처리등을 하게 되면 내부 서버에선 ssl인증을 하지 않아도 Https통신을 할 수 있게 된다.

4. `HSTS, CORS처리`

5. `TCP,UDP 커넥션 부하 분산(로드밸런싱)`
- 로드밸런싱을 하는 소프트웨어는 Nginx말고도 HAProxy가 있지만 현재는 Nginx만 알아본다.
- Nginx에선 로드밸런싱을 할때 각각 정해준 알고리즘을 사용하여 트래픽을 분산시키게 되는데 이 때 특정 알고리즘은 Nginx Plux에서만 된다고 한다.

**Nginx에서 로드밸런싱을 적용하기 위해 Nginx.conf파일을 추가/변경 시킨다**
```
upstream backend {  // backend자리에 이름
    least_conn;     //알고리즘을 적어준다. (기본: 라운드 로빈)
    server localhost:8801; 
    server localhost:8802; //클라이언트가 Nginx로 요청 시
    server localhost:8803; //우회시켜줄 Server 정보
}

server {
  listen 80; //클라이언트가 요청하는 포트
  
  location / {
    proxy_set_header Host $host; //클라이언트의 호스트 설정
    proxy_set_header Connection ""; //upstream서버를 사용하겠다 지정(⭐중요)
    proxy_pass http://backend; //설정한 이름으로 요청 보내기
  }
}
```
##### Nginx의 로드밸런싱 알고리즘
| Nginx | 방법 | 설명 |
|:---|:---|:---|
|| 라운드로빈(기본값) | 요청을 순서대로 처리한다.
||	least_conn(최소 연결) | 각 요청을 서버에 할당된 가중치를 고려해 연결 수가 가장 적은 서버로 전송
|| ip_hash | 요청이 클라이언트 IP주소로 해싱 > 한번 요청 받은 서버가 있을 때 해당 서버에만 요청을 분배
|| least_time | 	연결 수가 가장 적으면서 평균 응답시간이 가장 적은 쪽을 선택해서 분배 (Nginx Plus에서만 가능)
	

  
#### Reference
<https://jizard.tistory.com/306><br>
<https://velog.io/@wijihoon123/Nginx%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80><br>
<https://sihyung92.oopy.io/server/nginx_feat_apache><br>
<https://applefarm.tistory.com/105><br>
<https://earth-95.tistory.com/138><br>
<https://velog.io/@kimjiwonpg98/Nginx-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EA%B5%AC%EC%B6%95#-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%A2%85%EB%A5%98>

