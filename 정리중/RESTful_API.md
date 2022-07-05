## What

REST는 HTTP의 주요저자 중 한명인 로이필딩이 HTTP의 설계의 우수성에 비해 제대로 활용되지 못하는 모습에 안타까워하며 HTTP의 장점을 최대한 활용할 수 있는 아키텍처 REST를 발표했다.

REST(Representational State Transfer)란? 간단히 HTTP URI를 통해 리소스를 명시하고 HTTP METHOD를 통해 리소스에 대한 행위를 명시하는 것을 의미한다.

즉 RESTful API는 REST 아키텍처를 준수하는 애플리케이션 프로그래밍 인터페이스이다.

---

## Why

- 장점
    - HTTP 인프라를 그대로 사용하므로 별도의 인프라를 구축할 필요가 없다.
    - HTTP 표준을 따르는 모든 플랫폼에서 사용가능
    - URI와 METHOD를 통해 의도하는 바를 쉽게 파악할 수 있다.
    - 확장성
    - 유연성
    - 독립성
- 단점
    - METHOD의 형태가 제한적
    - 표준이 존재하지 않음
    - 구형 브라우저의 경우 제대로 지원해주지 못함

---

## How

RESTful API의 기본 기능은 인터넷 브라우징과 동일하다.

1. 클라이언트가 서버에 요청을 전송(API 문서에 따라 서버가 이해하는 방식으로 요청)
    - Request 요청
        - 헤더
        - 고유 리소스 식별자
        - HTTP 메서드
        - JSON, HTML, XML 등
2. 서버가 클라이언트를 인증하고 해당 요청을 수행할 수 있는 권한이 있는 클라이언트인지 확인
3. 서버가 요청을 수신하고 내부적으로 처리
4. 서버가 클라이언트에 응답을 반환. 응답에는 요청의 성공여부와 리소스 정보가 포함
    - Response 응답
        - 헤더
        - 응답코드
        - JSON, HTML, XML 등
    
    REST API 요청 및 응답의 세부사항은 API개발자가 설계하는 방식에 따라 다르다.
    

### REST API 설계 규칙

1. 소문자를 사용한다
2. 언더바대신 하이픈을 사용한다.
3. 마지막에는 슬래시를 포함하지 않는다.
4. 계층관계를 나타낼 때는 슬래시 구분자를 사용한다.
5. 파일확장자는 URI에 포함시키지 않는다.
6. 전달하고자 하는 자원의 명사를 사용하되, 컨트롤 자원을 의미하는 경우 예외적으로 동사를 허용한다.
7. 영어를 복수형으로 작성한다.

### REST API 설계 예시

| 행위 | HTTP METHOD | HTTP URI |
| --- | --- | --- |
| 리소스들을 조회 | GET | /resources |
| 특정 리소스를 조회 | GET | /resources/{id} |
| 리소스 생성 | POST | /resources |
| 리소스 수정 | PUT | /resources/{id} |
| 리소스 삭제 | DELETE | /resources/{id} |

---

## Feature

- Uniform Interface (인터페이스 일관성)
    - URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
    - HTTP를 따르는 모든 플랫폼에 사용가능 (특정 언어나 기술에 종속 X)
- Stateless (무상태)
    - HTTP을 사용하는 REST역시 무상태성을 갖는다.
    - 클라이언트의 정보를 서버에 저장하지 않는다.
        - 세션, 쿠키등
    - 서버는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
        - 요청1, 요청2 등의 요청이 연관되어서는 안된다.
        - 서버의 처리 방식에 일관성을 부여하고, 서버의 자유도가 올라간다.
        - 쉽게 확장할 수 있다.
- Cacheable (캐시 처리 가능)
    - HTTP의 강력한 특징 중 하나인 캐싱을 적용할 수 있다.
- Layerd System(계층화)
    - 서버는 다중 계층으로 구성될 수 있다.
        - 보안, 로드밸런싱, 암호화, 인증, Proxy, 게이트웨이 등
- Code on demand(optional)
    - 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.
- Server-Client 구조
    - 리소스가 있는 쪽이 Server, 리소스를 요청하는 쪽이 Client

---

## Conclusion

---

## Reference

- [https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
- [https://www.redhat.com/ko/topics/api/what-is-a-rest-api](https://www.redhat.com/ko/topics/api/what-is-a-rest-api)
- [https://ko.wikipedia.org/wiki/REST](https://ko.wikipedia.org/wiki/REST)
