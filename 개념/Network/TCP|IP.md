## TCP/IP(Transmission Control Protocol/Internet Protocol)
`정의` 
- 인터넷에 연결된 **서로 다른 기종의 컴퓨터들이 데이터를 주고받을 수 있도록 하는 표준 프로토콜**이다.

`프로토콜 및 설명`

| 프로토콜 | 내용 |
| :--: | :--: |
| TCP<br>(Transmission Control Protocol) | OSI 7계층의 전송 계층에 해당함 <br> 가상 회선 방식을 기반으로 하는 연결형 서비스를 제공함 <br> 패킷의 다중화, 순서 제어, 오류 제어, 흐름 제어 기능을 제공함 <br> 신뢰성 보장, 연결 지향적 특징 , 흐름 제어, 혼잡 제어의 4가지 특징을 갖고 있다. |
| IP<br>(Internet Protocol) | OSI 7계층의 네트워크 계층에 해당함 <br> 데이터그램 방식을 기반으로 하는 비연결형 서비스를 제공함 <br> 패킷의 분해/조립, 주소 지정, 경로 선택 기능을 제공함 |

`구조`

| OSI | TCP/IP | 기능 |
| :--: | :--: | :--: |
| 응용 계층 <br> 표현 계층 <br> 세션 계층 | 응용 계층 | 응용 프로그램 간의 데이터 송 * 수신 제공 <br> `TELNET`,  `FTP`, `SMTP`, `SNMP`, `DNS`, `HTTP` 등 |
| 전송 계층 | 전송 계층 | 호스트들 간의 신뢰성 있는 통신 제공 <br> `TCP`, `UDP`, `RTCP` 등 |
| 네트워크 계층 | 인터넷 계층 | 데이터 전송을 위한 주소 지정, 경로 설정을 제공함 <br> `IP`, `ICMP`, `IGMP`, `ARP`, `RARP`
| 데이터 링크 계층 <br> 물리 계층 | 네트워크 액세스 계층 | 실제 데이터(프레임)를 송 * 수신 하는 역할 <br> `Eternet`, `IEEE 802`, `HDLC`, `X.25`, `RS0232C`, `ARQ` 등


<details>
<summary><strong>응용 계층 주요 프로토콜</strong></summary>
<div>

| 프로토콜 | 내용 |
| :--: | :--: |
| FTP<br>(File Transfer Protocol) | 컴퓨터와 컴퓨터 또는 인터넷 사이에서 파일을 주고 받을 수 있도록 하는 원격 파일 전송 프로토콜 |
| SMTP<br>(Simple Mail Transfer Protocol) | 전자 우편을 교환하는 서비스 |
| TELNET | 멀리 떨어져 있는 컴퓨터에 접속해 자신의 컴퓨터처럼 사용할 수 있도록 해주는 서비스 <br> 프로그램을 실행하는 등 시스템 관리 작업을 할 수 있는 가상의 터미널(Virtual Terminal) 기능 수행 |
| SNMP(Simple Network Management Protocol) | TCP/IP의 네트워크 관리 프로토콜로, 라우터나 허브 등 네트워크 기기의 네트워크 정보를 네트워크 관리 시스템에 보내는데 사용되는 표준 통신 규약 |
| POP3(Post Office Protocol Version 3) | TCP 110번 포트를 사용하고, 응용 계층 인터넷 프로토콜 중 하나로, 원격 서버로부터 TCP/IP 연결을 통해 이메일을 가져오는 데 사용하는 프로토콜 |
| IMAP(Internet Messaging Access Protocol) | TCP 143번 포트를 사용하고, 원격 서버로부터 TCP/IP 연결을 통해 이메일을 가져오고, 온라인 및 오프라인을 모두 지원하는 프로토콜 |
| DNS(Domain Name System) | 도메인 네임을 IP 주소로 매핑(Mapping)하는 시스템 |
| HTTP(HyperText Transfer Protocol) | 월드 와이드 웹(www)에서 HTML 문서를 송수신 하기 위한 표준 프로토콜

</div>
</details>

<details>
<summary><strong>전송 계층 주요 프로토콜</strong></summary>
<div>

| 프로토콜 | 내용 |
| :--: | :--: |
| TCP <br> (Transmission Control Protocol) | 양방향 연결(Full Duples Connection)형 서비스를 제공 <br> 가상 회선 연결(Virtual Circuit Connection) 형태의 서비스 제공 <br> 스트림 위주의 전달(패킷 단위)을 함 <br> 신뢰성 있는 경로를 확립하고 메시지 전송을 감독함 <br> 순서 제어, 오류 제어, 흐름 제어 기능을 함 <br> 패킷의 분실, 손상, 지연이나 순서가 틀린 것 등이 발생할 때 투명성이 보장되는 통신을 함 <br> TCP 프로토콜의 헤더는 기본적으로 20Byte에서 60Byte까지 사용할 수 있는데, 선택적으로 40을 추가해 100Byte까지 크기 확장 가능 |
| UDP <br> (User Datagram Protocol) | 데이터 전송 전 연결을 설정하지 않는 비연결형 서비스 제공 <br> TCP에 비해 상대적으로 단순한 헤더 구조로 오버헤드 적고, 흐름 제어나 순서 제어 없어 전송 속도가 빠름 <br> 고속의 안정성 있는 전송 매체를 사용해 빠른 속도를 필요로 하는 경우, 동시에 여러 사용자에게 데이터를 전달할때 정기적으로 반복해 전송할 경우 사용 <br> 실시간 전송에 유리하며, 신뢰성보단 속도가 중요시되는 네트워크에서 사용 <br> UDP 헤더에는 Source Port Number, Destinaion Port Number, Length, Checksum 등 포함 |
|RTCP <br> (Real-Time Control Protocol) | RTP(Real-time Transport Protocol) 패킷의 전송 품질을 제어하기 위한 프로토콜 <br> 세션(Session)에 참여한 각 참여자들에게 주기적으로 제어정보를 전송함 <br> 하위 프로토콜은 데이터 패킷과 제어 패킷의 다중화(Multiplexing)를 제공 <br> 데이터 전송을 모니터링하고 최소한의 제어와 인증 기능만을 제공 <br> RTCP 패킷은 항상 32비트의 경계로 끝남 |

</div>
</details>

<details>
<summary><strong>인터넷 계층의 주요 프로토콜</strong></summary>
<div>

| 프로토콜 | 내용 | 
| :--: | :--: |
| IP<br>(Internet Protocol) | 전송할 데이터에 주소를 지정하고, 경로를 설정하는 기능을 함 <br> 비연결형인 데이터그램 방식을 사용하는 것으로 신뢰성이 보장되지 않음 |
| ICMP<br>(Internet Control Management Protocol) | IP와 조합해 통신중 발생하는 오류 처리와 전송 경로 변경 등을 위한 제어 메시지를 관리하는 역할을 함 <br> 헤더는 8Byte로 구성 |
| IGMP<br>(Internet Group Management Protocol, 인터넷 그릅 관리 프로토콜) | 멀티캐스르를 지원하는 호스트나 라우터 사이에서 멀티캐스트 그룹 유지를 위해 사용됨 |
| ARP<br>(Address Resolution Protocol, 주소 분석 프로토콜) | 호스트의 IP 주소를 호스트와 연결된 네트워크 접속 장치의 물리적 주소(Mac Address)로 바꿈 |
| RARP<br>(Reverse Address Resolution Protocol) | ARP와 반대로 물리적 주소를 IP 주소로 변환하는 기능을 함 |

</div>
</details>

<details>
<summary><strong>네트워크 액세스 계층의 주요 프로토콜</strong></summary>
<div>

| 프로토콜 | 내용 |
| :--: | :--: |
| Eternet(IEEE 802.3) | CSMA/CD 방식의 LAN |
| IEEE 802 | LAN을 위한 표준 프로토콜 |
| HDLC | 비트 위주의 데이터 링크 제어 프로토콜 |
| X.25 | 패킷 교환망을 위한 DTE와 DCE 간의 인터페이스를 제공하는 프로토콜 |
| RS-232C | 공중 전화 교환망(PSTN)을 통한 DTE와 DCE간의 인터페이스를 제공하는 프로토콜 |

</div>
</details>
