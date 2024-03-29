## Chapter12 다형성
- 상속의 목적은 코드의 재사용이 아니다! 상속은 타입 계층을 구조화하기 위해 사용해야 한다
- 타입계층은 객체지향 프로그래밍의 중요한 특성 중 하나인 `다형성`의 기반을 제공한다
- 다형성은 런타임에 메시지를 처리하기에 적합한 메서드를 동적으로 탐색하는 과정을 통해 구현
- 상속은 이런 메서드를 찾기 위한 일종의 탐색 경로를 클래스 계층의 형태로 구현하기 위한 방법
- 다형성
    - 하나의 추상 인터페이스에 대해 코드를 작성하고 이 추상 인터페이스에 대해 서로 다른 구현을 연결할 수 있는 능력
    - 오버로딩 다형성
        - 동일한 이름의 메서드
    - 강제 다형성
        - 자동적인 타입 변환이나 사용자가 직접 구현한 타입 변환을 이용해 동일 연산자를 다양한 타입에
        - ex) + 연산자 : 정수끼리는 덧셈 연산자, 문자열인경우 연결 연산자로 사용
    - 매개변수 다형성
        - 제네릭
    - 포함 다형성
        - 서브타입 다형성
        - 흔히 알고 있는 다형성(상속등을 통한)
- 상속의 진정한 목표는 코드 재사용이 아니라 다형성을 위한 서브타입 계층을 구축
- 객체가 메시지를 수신하면 객체 지향 시스템은 메시지를 처리할 적절한 메서드를 `상속 계층 안에서 탐색`
- 상속의 메커니즘을 위한 개념
    - 업캐스팅
        - 부모 클래스 타입으로 선언된 변수에 자식클래스의 인스턴스를 할당하는 것이 가능
        - 자식 클래스는 제약 없이 부모 클래스를 대체할 수 있기 때문에 부모 클래스와 협력하는 클라이언트는 자식 클래스의 인스턴스와도 협력 가능
        - 이는 상속받는 어떤 자식 클래스와도 협력할 수 있는 무한한 확장 가능성을 가진다
    - 동적 메서드 탐색
        - 부모 클래스 타입에 대해 메시지를 전송하더라도 실행 시에는 실제 클래스를 기반으로 실행될 메서드가 선택
        - 자식 클래스에 선언된 메서드가 높은 우선순위
        - 두가지 원리
            - 자동적인 메시지 위임
                - 자신이 이해할 수 없는 메시지를 전송받은 경우 자동으로 부모클래스에게 처리를 위임
            - 동적인 문맥
                - 메시지를 수신했을 때 어떤 메시지가 실행될지 런타임으로 결정되며 메서드를 탐색하는 경로는 self 참조를 이용
    - 동적 바인딩
        - 선언된 변수의 타입이 아니라 메시지를 수신하는 객체의 타입에 따라 실행되는 메서드가 결정
        - 메시지를 처리할 적절한 메서드를 컴파일 시점이 아니라 런타임 시점에 결정한다
        - 전통적인 언어는 컴파일시점에 호출될 함수를 결정. 이를 정적 바인딩, 초기 바인딩, 컴파일타임 바인딩이라 함
        - 객체 지향 언어에서는 런타임시점에 호출될 코드가 결정. 이를 동적 바인딩, 지연 바인딩이라 함
    - self 참조
        - 객체가 메시지를 수신하면 컴파일러는 self 참조라는 임시 변수를 자동으로 생성한 후 메시지를 수신한 객체를 가리키도록 함
        - 자바에서는 this
        - class 포인터와 parent 포인터와 함께 self 참조를 이용해 메서드를 탐색
    - super 참조
- 데이터 관점의 상속
- 행동 관점의 상속
    - 상속과 다형성의 기본적인 개념을 이해하기 위해서는 상속 관계로 연결된 클래스 사이의 메서드 탐색 과정을 이해해야 한다
    - 객체의 경우 서로 다른 상태를 저장할 수 있도록 각 인스턴스별로 독립적인 메모리를 할당받아야 한다
    하지만 메서드의 경우 동일한 클래스의 인스턴스끼리 공유가 가능하기 때문에 클래스는 한번만 메모리에 로드하고 각 인스턴스별로
    클래스를 가리키는 포인터를 갖는 것이 경제적. 
    - 메시지를 수신한 객체는 class 포인터로 연결된 자신의 클래스에게 적절한 메서드가 존재하는지 찾는다
    존재하지 않는다면 클래스의 parent 포인터를 따라 클래스를 차례대로 훑는다
- 
---

## 정리
- 객체로 프로그래밍 세계를 구현하고 이러한 객체를 코드로 담을 수? 나타낼 수 있는게 클래스, 각 객체가 식별되는 인스턴스
- 예전에 자바를 처음 배울 때 이해 했던 클래스, 객체, 인스턴스가 뭔가 새롭게 느껴진다.
- 분명 다형성, 상속 등 다 배운 개념들인데 객체지향에 대해 깊은 이해가 없었구나~~

- 상속의 목적은 다형성을 활용하기 위해서
- 다형성은 런타임 시점에 메시지를 수신할 적절한 객체의 적절한 메서드를 탐색하는 과정으로 구현
