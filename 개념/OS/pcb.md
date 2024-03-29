### PCB란?
- `Process Control Block(PCB)`의 약자로서 정한 프로세스를 관리할 필요가 있는 정보를 포함하는, 운영체제 커널의 자료구조이다.
- 운영체제(OS)가 프로세스 스케쥴링을 위해 프로세스에 관한 모든 정보를 갖고 있는 데이터베이스가 PCB이다.
- 운영체제에서 프로세스는 PCB로 나타내지며 프로세스의 주요 정보를 갖고 있고 프로세스가 생성될 때 고유의 PCB가 생성되고 

  프로세스가 종료될때 각 PCB는 제거된다.
  
```
프로세스는 CPU를 점유하다가도 Context Switching , 상태가 전이됨에 따라 점유하고 있던 CPU를 반환하고 대기 , 종료하거나
추후 다시 CPU를 점유하게 됐을때 진행했던 작업 , 시점등을 저장하지 않으면 실행 시점을 알 수 없기 때문에 PCB란 고유의 db에 저장하는 것이다.
```

#### PCB 구조

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/228878230-0492bd4a-3211-4b47-8226-6f0d3eede831.png">

1. 프로세스 식별자(Process ID)
2. 프로세스 상태(Process State) 
   - 생성(create), 준비(ready), 실행 (running), 대기(waiting), 완료(terminated) 상태 
3. 프로그램 계수기(Program Counter) 
   - 프로그램 계수기는 이 프로세스가 다음에 실행할 명령어의 주소를 가리킨다. 
4. CPU 레지스터 및 일반 레지스터
5. CPU 스케줄링 정보
   - 우선 순위, 최종 실행시각, CPU 점유시간 등
6. 메모리 관리 정보
   - 해당 프로세스의 주소 공간 등
7. 프로세스 계정 정보
   - 페이지 테이블, 스케줄링 큐 포인터, 소유자, 부모 등
8. 입출력 상태 정보 
   - 프로세스에 할당된 입출력장치 목록, 열린 파일 목록 등
9. 포인터
   - 부모프로세스에 대한 포인터, 자식 프로세스에 대한 포인터, 프로세스가 위치한 메모리 주소에 대한 포인터, 할당된 자원에 대한 포인터 정보 등. 

#### Reference
<https://jwprogramming.tistory.com/16><br>
<https://velog.io/@nnnyeong/OS-Context-Switching-PCB-Process-Control-Block><br>
