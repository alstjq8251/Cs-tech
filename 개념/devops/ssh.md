### ssh란?
- Secure Shell Protocol로서 SSH 프로토콜을 이용한 통신은 기본적으로 서버 클라이언트 모델에 기반한다
- SSH 접속을 위해서는 기본적으로 서버에 SSH 서버(= SSH 데몬)가 설치되어 있어야 하고, 클라이언트에는 SSH 클라이언트가 설치되어 있어야 한다.

### 동작 원리
1. 클라이언트-서버 연결 요청
2. 서버 인증
3. 키 교환을 통한 세션키 생성
4. 클라이언트 인증
5. 암호회된 세션 시작

#### 자세한 동작 과정
`클라이언트-서버 연결 요청`

1. 서버에서 공개키와 비밀키를 생성한다.
2. 클라이언트가 서버에 처음 접속 시도하면 서버의 공개키를 받아서 클라이언트에 있는 .ssh/known_hosts 파일에 저장한다.
3. 클라이언트는 난수값을 생성하여 난수의 해시값을 만들어 저장한다.
4. 난수 값을 서버의 공개키로 암호화하여 서버에 전송한다.
5. 서버에서는 암호화된 데이터를 비밀키로 복호화하여 난수 값을 얻어 낸다.
6. 난수 값으로 해시값을 만들어 클라이언트에 전송한다.
7. 클라이언트에서 저장하고 있는 3번 단계의 해시값과 서버로부터 전달받은 6번 단계의 해시값을 비교해 서버를 인증하게 된다.

`서버 인증`

1. 클라이언트가 서버에 처음 접속 시도시 연결을 진행할지 여부를 물어본다.(yes /no)
2. 서버가 자신의 공개키를 클라이언트에게 전송
   - 클라이언트가 서버에 접속을 시도하면, 서버는 자신의 **공개키(public key)**를 클라이언트에게 전송
3. 클라이언트는 서버의 공개키를 확인
4. 사용자가 서버의 공개키를 신뢰한다고 선택(yes)하면, 서버의 공개키는 클라이언트의 ~/.ssh/known_hosts 파일에 저장
5. 서버의 공개키가 이미 known_hosts 파일에 저장되어 있으면, 클라이언트는 이를 신뢰하고 자동으로 인증을 수행
6. 클라이언트가 이미 저장된 서버의 공개키와 접속하려는 서버의 공개키가 다르다면, 경고 메시지가 표시되며 중간자 공격 가능성이나 서버 변경 여부를 사용자에게 알림

`키 교환을 통한 세션키 생성`
1. 클라이언트와 서버는 각각 **난수(랜덤 값)**를 생성. 이 난수는 세션키를 안전하게 교환하는 데 중요한 역할을 한다.
2. 클라이언트와 서버는 자신들의 공개값을 교환한다. 공개값을 교환하더라도, 양측은 각각 비밀값을 가지고 있기 때문에 제3자는 세션키를 알 수 없다.
   - 키를 교환하는 알고리즘은 Diffie-Hellman이며 세션키를 안전하게 교환할 수 있도록 설계되었고 양측이 공개값을 서로 교환하여, 제3자가 세션키를 도청하지 못하게 보장한다.
3. 클라이언트와 서버는 서로의 공개값을 수신하고, 자신의 비밀값과 상대방의 공개값을 조합해 공통의 세션키를 생성
4. 세션키가 양측에서 동일하게 생성되면, 이후 통신은 세션키로 암호화되어 안전하게 이루어지고 세션키를 이용해 양측은 데이터를 암호화하여 전송하고, 복호화하여 수신할 수 있게 된다.


`사용자 인증`
1. 클라이언트가 서버에 접속할 때, 서버는 사용자 인증을 요구하고 이 과정에서 접속하려는 사용자임을 인증해야 한다.
2. 서버는 클라이언트에 지원하는 인증 방법을 전달하고 SSH는 여러 인증 방식을 지원하는데, 대표적으로 비밀번호 인증, 공개키 인증, GSSAPI 인증 등이 있다.
3. 사용자는 인증 방식을 선택하여 인증하는데 일반적으론 패스워드, 공개키 방식이 많이 사용된다.
   - `password`
     - 클라이언트는 사용자 이름과 비밀번호를 입력하고 서버에선 내부 저장된 비밀번호와 비교하여 인증에 성공하면 서버에 접속할 수 있다.
     - 패스워드가 암호화되지만 패스워드의 복잡성 설정의 한계가 있기 때문에 일반적으로 이 방법을 사용하지 않는 것이 좋다.
     - 자동화된 스크립트를 통해 일반적인 길이의 패스워드는 공격에 의해 해제될 수 있다.
   - `공개키 방식((Public Key Authentication))`
     - 클라이언트가 공개키와 비밀키 쌍을 사용하여 인증하는 방식이며 클라이언트가 소유한 비밀키를 통해 인증을 수행한다.
     1. 클라이언트는 인증할 키 쌍의 ID를 서버에 전송
     2. 서버는 클라이언트가 접속하고자 하는 계정의 .ssh/authorized_keys 파일을 확인
     3. ID에 매칭되는 공개키가 있을 시, 서버는 난수를 생성하고 클라이언트의 공개키로 암호화
     4. 서버는 클라이언트에게 암호화된 메시지 전송
     5. 클라이언트의 개인키를 통해 암호화된 메시지를 복호화하여 난수 추출
     6. 클라이언트는 난수를 세션키와 결합하여 해시값 계산 후 서버 전송
     7. 서버는 저장된 난수와 세션키를 결합하여 해시값 계산 후 비교
     8. 일치할 시 클라이언트 인증

### client에서 ssh연결이 안될경우
1. ~/.ssh/known_hosts
   - 해당 파일에서 접속하고자 하는 서버의 호스트(서버)의 공개 키와 호스트명(IP 주소 or 도메인 이름)을 저장된 구문을 지우고 다시 연결 시도

### Reference
<https://limvo.tistory.com/21>