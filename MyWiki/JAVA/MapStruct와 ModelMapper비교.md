# MapStruct와 ModelMapper 비교

레이어 간 데이터를 주고 받을 때 하나의 객체를 타입이 다른 객체로 변환하거나 여러 객체를 다른 객체로 합치는 일을 개발자가 직접 처리하게 되면 발생하는 문제점

- 코드 중복
- 실수 발생 가능성 높음
- 필드 추가나 수정, 삭제가 일어날 경우 변환하는 로직에 대한 수정이 필요
- 비즈니스 로직이 섞일 경우 코드가 복잡
- 생산성 저하

이러한 문제점을 해결하기 위한 Object Mapping 라이브러리

- ModelMapper , MapStruct

MapStruct와 ModelMapper의 차이

- MapStruct는 컴파일 시점에서 어노테이션을 읽어 구현체를 만들어내기 때문에 리플렉션이 발생하지 않지만 ModelMapper의 경우 Mapping이 일어날 때 리플렉션이 발생
    
    > 리플렉션? 
    객체를 통해 클래스의 정보를 분석해 내는 프로그래밍 기법. 구체적 클래스 타입을 알지 못해도 컴파일된 바이트 코드의 정보를 역으로 알아내어 클래스를 사용할 수 있다.
    리플렉션 기법을 통해 객체의 타입을 모르는 상태에서 객체의 메서드 호출 가능
    동적 바인딩이 되지 않던 자바에서 리플렉션이라는 기법을 통해 동적 바인딩을 제공
    > 
- MapStruct의 처리속도가 압도적으로 빠름
- MapStruct는 컴파일 시 오류를 확인할 수 있음
- MapStruct는 디버깅이 쉬움
- MapStruct는 생성된 매핑 코드를 눈으로 직접 확인할 수 있음

---

예시는 개발에 사용해보면서 추가하도록하겠음

MapStruct

👉 gradle 의존성 추가

```java
implementation 'org.mapstruct:mapstruct:1.5.3.Final'
annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.3.Final'
```

👉 Entity

```java
@Entity
public class Mindset extends BaseEntity{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long mindsetId;

    @Lob
    private String content;
}
```

👉 DTO

```java
public class MindsetServiceDto {
    private Long mindsetId;
    private String content;
}
```

👉 Mapper

```java
@Mapper
public interface MindsetMapper {
    MindsetMapper INSTANCE = Mappers.getMapper(MindsetMapper.class);
    Mindset serviceDtoToEntity(MindsetServiceDto mindsetServiceDto);
    MindsetServiceDto entityToDto(Mindset mindset);
}
```