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

<img width="771" alt="image" src="https://user-images.githubusercontent.com/98382954/214032014-2892314b-5ff5-4014-811a-e2f842560ba2.png">

- apache는 상단의 사진과 같이 unix의 os가 네트워크 커넥션을 생성하는 방식과 유사하게 요청하나에 프로세스 하나를 할당하는 방식으로 처리한다.
  - process를 생성하는 비용은 부하가 상당하기에 미리 생성해서 할당하는 `PREFORK`방식으로 요청을 처리했다.
  - 허나 PREFORK방식의 프로세스가 모두 할당되고 나면 새로운 요청은 새로 Process를 만들어 처리하게 되며 유저의 요청을 동적으로 처리하기에 적합했다.
  - 이렇게 요청 하나에 프로세스 하나씩을 할당하는 방식을 `Process-Driven`방식이라 한다.

**C10K란?**
```
Concurrent 10,000 clients/connections의 줄임말로 10000개의 요청을 동시에 처리하는 것을 의미한다.
```
      
#### Apache의 단점
- 위에서 설명한 것과 같이 C10K의 문제인데 10000개정도의 많은 요청이 몰리면 처리하기가 어렵다는 단점이 존재한다.
- 기술의 발전해감에 따라 하드웨어 성능이 향상되어 1만개 이상의 요청을 받을 수 있었으나 Apache의 내부구조의 단점 때문에 이를 처리하기가 어려웠다.
  - 쉽게 오버라이딩하여 개발자들마다 설정을 추가하고 바꾸는등 모듈을 쉽게 얹을 수 있는 장점에 비해 프로세스가 무거웠으며 이러한 프로세스를 

    각 connection마다 생성하니 자원이 쉽게 고갈되고 메모리가 감당하지 못해 많은 요청을 다중 제어 , 동시 제어하지 못하는 단점이 존재했다.
    
    - 이로 인해 MultiProcess -> MultiThread로 방식을 바꾸거나 PREFORK의 설정을 바꾸는 등 성능 향상을 하며 발전하긴 했으나 근본적인 구조는 바뀌지  
    
      못해 다중 커넥션 제어에는 여전히 약한 모습을 보인다.
  - Apache는 프로세스가 각 요청마다 할당되는 방식때문에 수많은 프로세스가 cpu를 컨텍스트 스위칭하는 비용도 많이 발생했다.

##### CPU 컨텍스트 스위칭이란?
- 

> Multiprocess와 MultiThread의 차이는 기술했던 하단의 링크를 들어가 확인이 가능하다.
<>


#### Nginx와 Apache의 차이점?

`Apache`는 Proccess-Driven방식으로 요청을 처리하며 10000개의 동시요청을 처리하기 어렵다,.
- Apache 웹 서버는 Proccess-Driven방식을 취하고 있는데 Apache의 MPM(Multi-Processing Module)은 클라이언트에

  요청이 많을 경우 요청시마다 Proccess 또는 Thread를 생성하며 처리하고 이것은 자원이 쉽게 고갈된다는 것을 의미한다.

  - 10000개의 요청이 있을경우 10000개의 해당하는 Process또는 Thread를 만들어야 하는데 만들때 생기는 비용과 오버헤드는 

    기존의 웹서버가 갖고 있던 단점이었고 대량의 트래픽을 처리하지 못하는 것을 의미했다.
    
 <img width="720" alt="image" src="https://user-images.githubusercontent.com/98382954/214029072-4cbb68ae-1264-49de-be94-895cd1c86705.png">
   

`Nginx`는 Event-Driven방식을 사용하고 있는데

#### Nginx의 구조?
<img width="705" alt="image" src="https://user-images.githubusercontent.com/98382954/214029449-09724352-2d1d-4693-a7ff-08436fcd4799.png">


#### Reference
<https://jizard.tistory.com/306><br>
<https://velog.io/@wijihoon123/Nginx%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80><br>
<https://sihyung92.oopy.io/server/nginx_feat_apache>

