## Chapter15 디자인패턴과 프레임워크
- 디자인 패턴
    - 소프트웨어 설계에서 반복적으로 발생하는 문제에 대해 반복적으로 적용할 수 있는 해결방법
    - 다양한 변경을 다루기 위해 반복적으로 재사용할 수 있는 설계의 묶음
    - `설계를 재사용`하기 위한 것
    - 특정한 변경을 일관성 있게 다룰 수 있는 협력 템플릿을 제공
    - 협력을 일관성 있게 만들기 위해 재사용할 수 있는 설계의 묶음
- 프레임 워크
    - `설계와 코드를 함께 재사용`하기 위한 것
    - 애플리케이션의 아키텍처를 구현 코드의 형태로 제공
    - 특정한 변경을 일관성 있게 다룰 수 있는 확장 가능한 코드 템플릿을 제공
    - 일관성 있는 협력을 제공하는 확장 가능한 코드
- 패턴
    - 하나의 실무 컨텍스트에서 유용하게 사용해 왔고 다른 실무 컨텍스트에서도 유용할 것이라고 예상되는 아이디어
    - 패턴은 개발자들이 다른 컨텍스트에서도 유용할 것이라고 생각하는 어떤 것
    - 패턴의 가장 큰 가치는 경험을 통해 축적된 실무 지식을 효과적으로 요약하고 전달할 수 있다는 점
    - 패턴은 경험의 산물
    - 패턴의 이름은 높은 수준의 대화를 가능하게 하는 원천
        - 인터페이스 추가 하고 ~~~해라 -> 전략패턴을 적용해라
    - 분류
        - 아키텍처 패턴
            - 미리 정의된 서브시스템들을 제공
            - 각 서브시스템들의 책임을 정의하며, 서브 시스템들 사이의 관계를 조직화하는 규칙과 가이드라인
        - 분석 패턴
            - 도메인 내의 개념적인 문제를 해결하는 데 초점
        - 디자인 패턴
            - 특정 정황 내에서 일반적인 설계 문제를 해결
            - 협력하는 컴포넌트들 사이에서 반복적으로 발생하는 구조를 서술
        - 이디엄
            - 특정 언어에 국한된 하위 레벨 패턴
- 객체지향 설계에서 가장 중요한 일은 올바른 책임을 올바른 객체에게 할당하고 객체 간의 유연한 협력관계를 구축하는 일
- 패턴은 공통으로 사용할 수 있는 역할, 책임, 협력의 템플릿
- 패턴의 구성 요소는 클래스가 아닌 `역할`
    - 패턴을 구성하는 요소가 클래스가 아니라 역할이라는 사실은 패턴 템플릿을 구현할 수 있는 다양한 방법이 존재한다는 말
- 디자인 패턴의 구성요소가 클래스와 메서드가 아니라 `역할과 책임`이라는 사실을 이해하는 것이 중요!!
- 어떤 구현 코드가 어떤 디자인 패턴을 따른다고 이야기할 때는 역할, 책임, 협력의 관점에서 유사성을 공유한다는 것이지 특정한 구현 방식을 강제하는 것은 아니라는 점을 이해하는 것 역시 중요
- 디자인 패턴은 단지 역할과 책임, 협력의 템플릿을 제안할 뿐 구체적인 구현 방법에 대해서는 제한을 두지 않는다
- 대부분의 디자인 패턴은 협력을 일관성 있고 유연하게 만드는 것을 목적으로 한다
- 각 디자인 패턴은 `특정한 변경을 캡슐화`하기 위한 독자적인 방법을 정의
- 어떤 디자인 패턴이 어떤 변경을 캡슐화하는지를 이해하는 것이 중요 그리고 각 디자인 패턴이 `변경을 캡슐화하기 위해 어떤 방법을 사용하는지`를 이해하는 것이 더중요
- 디자인 패턴은 맹목적으로 사용해서는 안된다 
    - 해결하려는 문제가 아니라 패턴이 제시하는 구조를 맹목적으로 따르는 것은 유지보수하기 어려운 시스템을 낳는다
- 프레임워크
    - 설계를 재사용하면서도 유사한 코드를 반복적으로 구현하기 위한 문제를 해결!
    - 추상 클래스나 인터페이스를 정의하고 인스턴스 사이의 상호작용을 통해 시스템 전체 혹은 일부를 구현해 놓은 재사용 가능한 설계
    - 애플리케이션 개발자가 현재의 요구사항에 맞게 커스터마이징할 수 있는 애플리케이션의 골격
    - 프레임워크는 애플리케이션의 아키텍처를 제공하며 문제 해결에 필요한 설계 과정과 이에 필요한 기반 코드를 함께 포함
    - 또한 애플리케이션을 확장할 수 있도록 부분적으로 구현된 추상클래스와 인터페이스 집합뿐만 아니라 추가적인 작업 없이도 재사용 가능한 다양한 종류의 컴포넌트도 함께 제공
- 프레임워크의 핵심은 추상 클래스나 인터페이스와 같은 추상화
- 협력을 일관성있고 유연하게 만들기 위해서는 추상화를 이용해 변경을 캡슐화
- 프레임워크는 여러 애플리케이션에 걸쳐 재사용 가능해야 하기 때문에 변하는 것과 변하지 않는 것들을 서로 다른 주기로 배포할 수 있도록 별도의 배포단위로 분리해야한다
    - 변하는 부분과 변하지 않는 부분을 별도의 패키지로 구분
    - 중요한 것은 패키지 사이의 의존성 방향
    - 세부 사항을 구현한 패키지는 항상 상위 정책을 구현한 패키지에 의존
    - 상위 정책을 구현하고 있는 패키지가 충분히 안정적이고 성숙되었다면 하위 정책 패키지로부터 완벽히 분리하여 별도의 배포 단위로 만들 수 있다
- 상위 정책을 재사용한다는 것은 결국 도메인에 존재하는 핵심 개념들 사이의 협력 관계를 재사용한다는 것을 의미
- 객체 지향 설계의 재사용성은 개별 클래스가 아니라 객체들 사이의 공통적인 협력흐름으로부터 나온다
- 훌륭한 객체지향 설계는 의존성이 역전된 상태
- 의존성 역전 원리는 프레임워크의 가장 기본적인 설계 메커니즘
- 의존성 역전은 의존성의 방향뿐만 아니라 제어 흐름의 주체 역시 역전시킨다 -> 프레임워크가 애플리케이션에 속하는 서브클래스의 메서드를 호출
- 이를 제어의 역전(Inversion Of Control), 할리우드 원리라고 한다
- 협력을 제어하는 것은 프레임 워크
---

## 정리
- 디자인 패턴은 변경을 캡슐화!, 변경을 캡슐화하기 위해 어떤 방법을 사용하는지 파악하라! -> 디자인패턴 공부 다시 하자
- 프레임워크
- 별도의 모듈로 배포
- 