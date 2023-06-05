# TCP와 UDP

## What

TCP와 UDP는 인터넷 프로토콜 스택의 4계층에서 전송계층에 해당하는 프로토콜입니다. 

### TCP

TCP는 Transmission Control Protocol로 직역하면 전송 제어 프로토콜 입니다.

TCP는 연결지향 프로토콜로 발신지와 수신지를 연결하여 패킷을 전송하기 위한 논리적 경로가 연결된 상태에서 데이터를 주고 받는 프로토콜입니다. 이때 3-way handshaking 과정을 통하여 통신 선로가 고정되고 순차적으로 데이터를 전달합니다. 

- **TCP 3 way handshaking**

<img width="654" alt="스크린샷 2022-06-23 오후 12 34 20" src="https://user-images.githubusercontent.com/77914416/175547090-e9e3e6d9-9e44-4041-a481-71efec712703.png">

1. SYN
2. SYN + ACK
3. ACK + (함께 데이터 전송가능)
- **데이터 전달 보증**

<img width="631" alt="스크린샷 2022-06-23 오후 12 37 04" src="https://user-images.githubusercontent.com/77914416/175547335-c91c78ca-16bb-4a7c-8123-f821965e3433.png">

1. 데이터 전송
2. 데이터 수신
- **순서 보장**

<img width="626" alt="스크린샷 2022-06-23 오후 12 40 32" src="https://user-images.githubusercontent.com/77914416/175547374-631e6e45-063c-4f1b-80fb-f544ba71018e.png">

1. 패킷1, 패킷2, 패킷3 순서로 전송
2. 패킷1, 패킷3, 패킷2 순서로 도착
3. 패킷2부터 다시 전송 요청

### UDP

UDP는 User Datagram Protocol로 사용자 데이터그램 프로토콜 입니다.

UDP는 비연결지향 프로토콜로 TCP와 달리 서버와 클라이언트가 연결된 상태가 아니라 비연결 상태에서 클라이언트가 일방적으로 데이터를 전송합니다. 순차적으로 데이터를 전송하더라도 다른 통신선로를 거칠 수 있어 서버에 도착하는 순서가 달라질 수 있습니다. 이렇게 데이터 전달을 보증하지 않고 신뢰성이 떨어지지만 장점으로는 단순하며 속도가 빠른 특징이 있습니다. 

---

## Conclusion

### 공통점

- 데이터 전송에 PORT번호를 사용
- 데이터 검증을 위해 CHECKSUM 사용

### 차이점

| 프로토콜 | TCP | UDP |
| --- | --- | --- |
| 연결방식 | 연결지향형 프로토콜 | 비 연결지향형 프로토콜 |
| 신뢰성 | 높음 | 낮음 |
| 속도 | 상대적으로 느림 | 상대적으로 빠름 |
| 패킷 교환 방식 | 세그먼트 패킷 | 데이터그램 패킷 |
| 데이터 전달 보증 | 데이터 전달 보장O | 데이터 전달 보장 X |
| 순서 보장 | 순서 보장 O | 순서 보장 X |
| 사용예 | 데이터 전송에 신뢰성이 중요한 경우HTTP, Email, File … | 데이터 전송에 신뢰성보다 속도가 중요 경우 DNS, Broadcasting … |

---

## Reference

- [https://coding-factory.tistory.com/614](https://coding-factory.tistory.com/614)
- [https://mangkyu.tistory.com/15](https://mangkyu.tistory.com/15)
- 인프런 김영한님 HTTP 강의
