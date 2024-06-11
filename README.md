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
- 