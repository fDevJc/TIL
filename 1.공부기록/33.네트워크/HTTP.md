# HTTP

- HTTP 란?
    - Hypertext Protocol
    - 서버-클라이언트 메시지 교환 프로토콜
- 프로토콜이란?
    - 서로 다른 하드웨어 기기 간 데이터 통신 규약
- TCP/IP 4계층
    - HTTP
    - TCP
        - 트랜스포트 계층 속의 프로토콜
        - 서버와 클라이언트 사이 통신 연결 제어
        - 바이트 스트림을 사용하여 전송
        - 3way-handshaking, 4way-handshaking
        - 신뢰성을 담당
    - IP
        - 인터넷 계층
        - 데이터 패킷 배송
        - MAC 주소로 배송
        - IP 주소는 계층형 이기 떄문에 방향을 알 수 있다.
        - ARP
        - DNS
    - 네트워크
- Request, Response
    - Request
    메서드 URI 프로토콜 버전
    호스트 등 헤더
    바디
    - Response
    프로토콜버전 상태코드 상태코드설명
    헤더
    바디
- Stateless
    - 상태가 없다
    - 상태가 없으니 확장이 쉽다
    - 상태 유지를 하기 위해 세션과 쿠키 같은 기술이 있음
- URI로 리소스 식별
- 지속연결
    - 초기 HTTP는 비지속 연결
- HTTP 메서드
    - GET, POST, PUT, DELETE …
    - 멱등성
        - 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미
- HTTP 상태 코드
    - 1XX
        - 처리중
    - 2XX
        - 처리 성공
    - 3XX
        - 리다이렉트
    - 4XX
        - 잘못된 요청
    - 5XX
        - 서버에러
- Header 종류