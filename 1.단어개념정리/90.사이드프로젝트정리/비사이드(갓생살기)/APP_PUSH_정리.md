# APP PUSH

## APP PUSH??

👉 애플 UserNotification Framework

- [https://developer.apple.com/documentation/usernotifications](https://developer.apple.com/documentation/usernotifications)

👉 애플의 푸시 서비스 2가지 종류

- APNs를 이용한 푸시 - 사용할것
- 로컬(앱내)에서 푸시

👉 Push Notification을 통해 할수 있는 작업

1. 짧은 텍스트 메시지 표시
2. 짧게 소리 울리기
3. 앱 아이콘 배지 숫자 설정

---

## APP Push Process

👉 토큰 등록 프로세스

1. App → APNs : Device Token 요청
    - Device Token : 푸시가 전송되는 App의 고유 주소
        - 애플에서 정의한 고유한 식별자를 포함시킨 NSData 형태, 해독은 APNs만 할 수 있음
        - 각 앱 Instance는 APNs를 등록할 때마다 고유한 Device Token을 수신
2. APNs → App : Device Token 전송
3. App → Server : Server Device Token 전송
    - Server에서는 Device Token 정보를 관리

👉 푸시 발생 프로세스

1. Server → APNs : Device Token과 보내고싶은 데이터 전송
    - Server와 APNs는 token-based, certificate-based 연결 방식 두개중 하나 사용가능
        - [token-based](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_token-based_connection_to_apns)
        - [certificate-based](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns)
    - TLS 통신이기때문에 인증서가 준비되어야함
        - 인증서종류
            - p12 : 1년매다 갱신
            - p8 : 영구
2. APNs → APP : 데이터 전송
    - Background 상태면 OS에서 처리
    - Foreground 상태면 AppDelegate에 정의 해놓은 방식으로 처리

👉 APNs 란?

- Apple Push Notification service
- third party(Push Server) 개발자가 우리 앱에 푸시 알람을 보낼 수 있도록 Apple에서 만든 알림 서비스 플랫폼

```java
(X) Push Server → App

(O) Push Server → APNs → App

APNs 를 거쳐 Push 가능
```

---

## 사용가능 서비스

👉 FCM

- 제일 문서가 많아서 적용하기 쉬울듯 ?

👉 APNs를 이용해 직접 서버구현

- 음 ??

👉 SENS

- 네이버 클라우드 플랫폼
- [https://www.ncloud.com/product/applicationService/sens](https://www.ncloud.com/product/applicationService/sens)
- [https://medium.com/naver-cloud-platform/이렇게-사용하세요-sens를-활용한-push-app-구현-및-firebase-연동하기-a78792cba00d](https://medium.com/naver-cloud-platform/%EC%9D%B4%EB%A0%87%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%84%B8%EC%9A%94-sens%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-push-app-%EA%B5%AC%ED%98%84-%EB%B0%8F-firebase-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0-a78792cba00d)
- 장점
    - 모니터링, 대시보드 제공
    - 사용가이드 한글(뭐 FCM도 한글지원 되네잉)
- 단점
    - 유료

👉 Onesignal

- [https://blog.performars.com/ko/구글-파이어베이스-대신-원시그널-푸시-메시지를-써야하는-이유](https://blog.performars.com/ko/%EA%B5%AC%EA%B8%80-%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EB%8C%80%EC%8B%A0-%EC%9B%90%EC%8B%9C%EA%B7%B8%EB%84%90-%ED%91%B8%EC%8B%9C-%EB%A9%94%EC%8B%9C%EC%A7%80%EB%A5%BC-%EC%8D%A8%EC%95%BC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)

---

## 결론

1. FCM을 그냥 사용하면 될거같다?
2. BE쪽보다는 FE쪽이 손이 많이 갈거같다. (FE분들의 의견이 중요할듯?)
3. FCM 적용했을때 프로세스
    1. 회원가입시 FCM 토큰 서버에 전달
    2. 서버에서 사용자정보와 FCM 토큰 관리
    3. push시 서버에서 FCM에 푸시 요청
    4. FCM에서 APP으로 푸시
    5. APP에서는 주기적으로 신선한 FCM토큰을 받아와 서버에 전달
    6. 서버에서 전달받은 FCM토큰 갱신
4. FCM 토큰과 서비스 토큰 관리는 어떻게 ?
    - 처음에 잘못생각한듯…app ↔ Server 간 통신에 항상 header에 FCM 토큰이 있을필요는 없다.
    - 최초 앱 등록시 request를 통해 FCM 토큰을 서버에 등록한다
    - FCM토큰을 서버에서 관리한다.
        - 이때 토큰의 신선도는 어떻게 보장하는가
            - FCM에서는 앱에서 주기적으로 토큰을 확인하여 서버에 전달해주고 서버에서는 app으로 받은 토큰을 주기적으로 업데이트 하는 것을 권장
            - [https://firebase.google.com/docs/cloud-messaging/manage-tokens?hl=ko#ensuring-registration-token-freshness](https://firebase.google.com/docs/cloud-messaging/manage-tokens?hl=ko#ensuring-registration-token-freshness)
    - 이후 서버 → FCM 으로 푸시 request를 보낼때만 header에 FCM 토큰을 넣으면된다.