# basic-IoT-Network-Programming
IoT 개발자과정 블루투스 리포지토리

## 1일차
- TCP/IP -> cmd(명령 프롬포트)> ipconfig

- 전송방식 
    - TCP / UDP
    - TCP : 데이터를 잃어버리면 안되는 것(ex. 압축파일)
    - UDP : 데이터를 한두개 잃어버려도 되는 것 (ex. 영화 파일)

- 소.말.리.아. -> 소켓(socket), 바인더(bind), 리슨(listen), 엑셉트(accept)
    - 소켓 : 휴대폰을 고르는 것
    - 바인더 : 휴대폰 개통을 위해 개인정보와 전화번호가 필요함(할당)
    - 리슨 : 휴대폰 개통
    - 엑셉트 : 통화 버튼 누르고 통화하는 것

- Putty 사용법(192.168.5.3)
    - 아이디 : pi / 비번 : raspi

    ![Putty 실행화면](https://raw.githubusercontent.com/qkrskdusdlqslek/basic-IoT-Network-Programming/main/images/Putty.png)

    ![Putty 실행화면](https://raw.githubusercontent.com/qkrskdusdlqslek/basic-IoT-Network-Programming/main/images/Putty2.png)

- 리눅스 기반 파일 조작
    - 파일 디스크립터
        - 0 : 표준입력(Standard Input)
        - 1 : 표준출력(Standard Output)
        - 2 : 표준에러(Standard Error)

- 프로토콜 
    - 컴퓨터 상호간의 대화에 필요한 통신규약

- 소켓 타입
    - 연결지향형 소켓(TCP) / 비 연결지향형 소켓(UDP)

- 클래스 별 네트워크 주소와 호스트 주소의 경계
    - 클래스 A의 첫번째 바이트 범위 : 0이상 127이하 (0000 0000 ~ 0111 1111)
    - 클래스 B의 첫번째 바이트 범위 : 128이상 191이하 (1000 0000 ~ 1011 1111)
    - 클래스 C의 첫번째 바이트 범위 : 192이상 223이하 (1100 0000 ~ 1101 0010)
        - 포트번호 겹치면 안됨.

- 주소 정보의 표현
    - 구조체 sockaddr_in의 분석
        - AF_INET = PF_INET : IPv4 인터넷 프로토콜에 적용하는 주소체계
    - 바이트 순서와 네트워크 바이트 순서
        - 빅 엔디안 : 상위 바이트의 값을 작은 번지수에 저장
        - 리틀 엔디안 : 상위 바이트의 값을 큰 번지수에 저장

## 2일차
- 인터넷 주소의 초기화
```
    struct sockaddr_in addr;  // IP주소 문자열 선언
    char *serv_ip="211.217.168.13; // PORT번호 문자열 선언
    char *serv_port="9190" // 구조체 변수 addr의 모든 멤버 0으로 초기화
    memset(&addr, 0, sizeof(addr)) // 구조체 변수 addr의 모든 멤버 0으로 초기화
    addr.sin_family=AF_INET // 주소체계 지정
    addr.sin_addr.s_addr=inet_addr(serv_ip_) // 문자열 기반의 IP주소 초기화
    addr.sin_port=htons(atoi(serv_port)) // 문자열 기반의 PORT번호 초기화
```
- memset 함수는 동일한 값으로 바이트단위 초기화를 할 때 호출
- addr을 전부 0으로 초기화 하는 이유는, 0으로 초기화해야 하는 sockaddr_in 구조체 멤버 sin_zero를 0으로 초기화 하기 위함.

- bind(함수)
    - 초기화된 주소정보를 소켓에 할당

- LINK 계층
    - 물리적인 영역의 표준화에 대한 결과    
    - 네트워크 표준과 관련된 프로토콜을 정의하는 영역

- IP 계층 (비 연결지향적)
    - 복잡하게 연결되어 있는 인터넷을 통한 데이터의 전송을 위해 어떤 경로를 거쳐갈 것인가의 문제를 해결하는 계층
    - 프로토콜(오류발생에 대한 대비가 X)

- TCP/UDP 계층
    - IP 계층에서 알려준 경로정보를 바탕으로 데이터의 실제 송수신을 담당
    - UDP는 TCP에 비해 상대적으로 간단함
    - TCP는 신뢰성 있는 데이터의 전송 담당 / 확인절차를 통해 응답을 받아 신뢰성 없는 IP에 신뢰성을 부여한 프로토콜

- TCP(신뢰성 전송방식)
    - TCP 서버의 함수호출 순서
        - **socket() 소켓생성 -> bind() 소켓 주소할당 -> listen() 연결요청 대기상태 -> accept() 연결허용 -> read()/write() 데이터 송수신 -> close()         연결종료**

    - TCP 클라이언트 함수호출 순서
        - **socket() -> connect() -> read()/write() -> close()**

    - accept(함수)
        - '연결요청 대기 큐'에서 대기중인 클라이언트의 연결요청을 수락하는 기능
        - 내부적으로 데이터 입출력에 사용할 소켓 생성
        - 소켓의 파일 디스크립터를 반환
        - 소켓이 자동으로 생성되어, 연결요청을 한 클라이언트 소켓에 연결까지 이뤄짐

    - TCP 소켓에 존재하는 입출력 버퍼
        - 입출력 버퍼는 TCP 소켓 각각에 대해 별도로 존재함
        - 입출력 버퍼는 소켓생성시 자동으로 생성됨
        - 소켓을 닫아도 출력버퍼에 남아있는 데이터는 계속해서 전송이 이뤄짐
        - 소켓을 닫으면 입력버퍼에 남아있는 데이터는 소멸됨

- UDP(빠른 전송방식)
    - TCP보다 훨씬 간결한 구조로 설계되어 있음
    - TCP는 신뢰성이 없는 IP를 기반으로 신뢰성 있는 데이터의 송수신을 위해 '흐름제어'를 하지만, UDP는 '흐름제어'가 없음
    - 호스트로 수신된 패킷을 PORT 정보를 참조해 최종 목적지인 UDP 소켓에 전달
    - 서버와 클라이언트는 연결되어 있지 않음
    - 서버건 클라이언트건 하나의 소켓만 있으면 됨

## 3일차
- 





