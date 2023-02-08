# JWT

## 자료모으기

- JWT(Json Web Token)
- 수평으로 쉽게 확장 가능
    - 세션방식의 인증의 경우 중앙 세션 스토리지 시스템이 필요
    - 고정 세션 방식은 비용이 증대됨
- 유지 보수 및 디버그 용이
    - Base64 방식으로 데이터 인코딩되어 웹 토큰 자체에 포함됨
- 보안성
    - 암호화 알고리즘으로 암호화하여 클라이언트 변조를 막음
    - 특정 IP 주소로 JWT발행 및 브라우저 핑거 프린팅 등을 사용하여 토큰 탈취에 따른 보안 강화
- RESTful 서비스에 적합
    - 쿠키방식의 경우 쿠키의 출처가된 도메인에 대해서만 사용할 수 있는 단점 해소
- 토큰 자체 만료시간 기술
    - 토큰 만료시간을 미리 알고 갱싱 요청을 할 수 있음
- Refresh Token
    - Access Token을 재발급 하는데 필요한 정보를 포함
    - 만료될 수 있으며 일반적으로 오랜기간 사용됨
    - 누출되지 않도록 엄격한 관리가 필요함
    - 발급된 IP 기준으로 제한을 두거나 브라우저 핑거프린터 정보등을 통해 유요성 확인 필요
- Access Token
    - 자원에 직접 Access하는데 필요한 정보를 포함
    - 클라이언트의 권한 부여 여부를 판단
    - 만료날짜가 있으며 토큰 수명이 짧음

---

## What

---

## Why

---

## How

---

## Conclusion

---

## Reference

- [https://velog.io/@park2348190/JWT에서-Refresh-Token은-왜-필요한가](https://velog.io/@park2348190/JWT%EC%97%90%EC%84%9C-Refresh-Token%EC%9D%80-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80)
- [https://en.wikipedia.org/wiki/JSON_Web_Token](https://en.wikipedia.org/wiki/JSON_Web_Token)
- [https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-token-structure](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-token-structure)
- [https://jwt.io/introduction](https://jwt.io/introduction)
- [https://kukekyakya.tistory.com/entry/Spring-boot-access-token-refresh-token-발급받기jwt](https://kukekyakya.tistory.com/entry/Spring-boot-access-token-refresh-token-%EB%B0%9C%EA%B8%89%EB%B0%9B%EA%B8%B0jwt)
- [https://inpa.tistory.com/entry/WEB-📚-Access-Token-Refresh-Token-원리-feat-JWT](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT)
- [https://donologue.tistory.com/397](https://donologue.tistory.com/397)
- [https://tecoble.techcourse.co.kr/post/2021-10-20-refresh-token/](https://tecoble.techcourse.co.kr/post/2021-10-20-refresh-token/)
- [https://cotak.tistory.com/102](https://cotak.tistory.com/102)
- [https://llshl.tistory.com/32](https://llshl.tistory.com/32)