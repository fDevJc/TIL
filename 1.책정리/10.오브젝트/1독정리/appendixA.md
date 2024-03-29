## 부록A 계약에 의한 설계
- 인터페이스만으로는 객체의 행동에 관한 다양한 관점을 전달하기 어렵다. 그래서 명령의 부수효과를 쉽고 명확하게 표현할 수 있는 커뮤니케이션 수단이 필요하다. 계약에 의한 설계는 이러한 수단이 될 수 있다
- 계약에 의한 설계는 클래스의 부수효과를 명시적으로 문서화하고 명확하게 커뮤니케이션할 수 있을뿐만 아니라 실행 가능한 검증 도구로써 사용할 수 있다 
- 인터페이스는 객체가 수신할 수 있는 메시지를 정의할 수 있지만 객체 사이의 의사소통 방식은 명확하게 정의할 수 없다
- 서버는 자신이 처리할 수 있는 범위의 값들을 클라이언트가 전달할 것이라고 기대한다
- 클라이언트는 자신이 원하는 값을 서버가 반환할 것이라고 예상한다.
- 계약에 의한 설계를 구성하는 세가지 요소
    - 사전조건
        - 클라이언트의 의무
        - 서버는 사전조건이 만족되지 않을 경우 메서드를 실행할 의무가 없다
    - 사후조건
        - 서버의 의무
    - 불변식
        - 인스턴스가 생성된 후에 만족돼야 한다
        - 메서드 실행 전과 메서드 종료 후에는 항상 불변식을 만족하는 상태가 유지돼야 한다
- 서브타입이 리스코프 치환 원칙을 만족시키기 위해서는 클라이언트와 슈퍼타입 간에 체결된 계약을 준수해야한다
- 계약규칙
    - 서브타입에 더 강력한 사전조건을 정의할 수 없다
    - 서브타입에 더 완화된 사후조건을 정의할 수 없다
    - 슈퍼타입의 불변식은 서브타입에서도 반드시 유지돼야 한다
- 가변성 규칙
    - 서브타입은 슈퍼타입이 발생시키는 예외와 다른 타입의 예외를 발생시켜서는 안된다
    - 서브타입의 리턴타입은 공변성을 가져야한다
    - 서브타입의 메서드 파라미터는 반공변성을 가져야한다
- 일찍 실패하기
    - 문제의 원인을 파악할 수 있는 가장 빠른 방법은 문제가 발생하자마자 프로그램이 일찍 실패하게 만드는 것

---

## 정리
- 참고
    - https://happy-coding-day.tistory.com/entry/%EA%B3%84%EC%95%BD%EC%97%90-%EC%9D%98%ED%95%9C-%EC%84%A4%EA%B3%84Contract-By-Design-%EB%8D%94-%EC%9E%98-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0with-java
    - https://javacan.tistory.com/entry/79