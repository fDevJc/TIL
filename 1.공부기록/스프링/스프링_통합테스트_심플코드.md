# [SpringBoot] 스프링부트 통합테스트 심플코드

샘플 코드 및 테스트 코드를 최대한 심플하게 작성하였습니다.

## 샘플 패키지 및 코드

### 패키지

![스크린샷 2022-05-26 오후 6.12.36.png](https://user-images.githubusercontent.com/77914416/170467085-8073c941-716c-43ef-8b61-9b69690884c3.png)

### 컨트롤러

```java
package ironyang.jpa.sample.controller;

import ironyang.jpa.sample.domain.Hello;
import ironyang.jpa.sample.service.HelloService;
import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RequiredArgsConstructor
@RestController
public class HelloController {
    private final HelloService helloService;

    @PostMapping("/hello")
    public String save(@ModelAttribute Hello hello) {
        Long savedId = helloService.save(hello);
        return "ok!! id: "+savedId.toString();
    }
}
```

### 서비스

```java
package ironyang.jpa.sample.service;

import ironyang.jpa.sample.domain.Hello;
import ironyang.jpa.sample.repository.HelloRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@RequiredArgsConstructor
@Transactional(readOnly = true)
@Service
public class HelloService {
    private final HelloRepository helloRepository;

    @Transactional
    public Long save(Hello hello) {
        Hello save = helloRepository.save(hello);
        return save.getId();
    }
}
```

### 레파지토리

```java
package ironyang.jpa.sample.repository;

import ironyang.jpa.sample.domain.Hello;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface HelloRepository extends JpaRepository<Hello, Long> {}
```

### 도메인

```java
package ironyang.jpa.sample.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Getter @Setter
@Entity
public class Hello {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
}
```

## 통합테스트 진행

### 테스트코드

```java
package ironyang.jpa.sample.acceptance;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;
import org.springframework.transaction.annotation.Transactional;

@Transactional
@AutoConfigureMockMvc
@SpringBootTest
class HelloAcceptanceTest {
    @Autowired
    MockMvc mvc;

    @Test
    void hello() throws Exception {
        //given
        String name = "hello!";

        //when
        ResultActions resultActions = mvc.perform(MockMvcRequestBuilders.post("/hello")
                .param("name", name))
                .andDo(MockMvcResultHandlers.print());

        //then
        resultActions.andExpect(
                MockMvcResultMatchers.status().isOk()
        );
    }
}
```

**Annotation**

@Transactional

- 해당 어노테이션을 지정하면 스프링이 트랜잭션을 지원해준다.
- 테스트 코드에 작성시 자동 롤백이 된다.
    - 롤백이 실행되지 않도록 하려면 `@Rollback(false)` 혹은 `@Commit` 을 추가해주면된다.
    
    ```java
    @Commit
    @Rollback(false)
    ```
    
    - 참고로 `@Commit` 어노테이션을 보면 `@Rollback(false)` 가 포함되어있다.
    
    ```java
    @Target({ElementType.TYPE, ElementType.METHOD})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Inherited
    @Rollback(false)
    public @interface Commit {}
    ```
    

@AutoConfigureMockMvc

- `MockMvc`를 이용해 테스트를 진행하기 위해 필요한 어노테이션이다.
- 참고로 Web 계층(Controller)을 테스트하기 위한 `@WebMvcTest`의 경우 애노테이션이 포함되어있다.

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@BootstrapWith(WebMvcTestContextBootstrapper.class)
@ExtendWith(SpringExtension.class)
@OverrideAutoConfiguration(enabled = false)
@TypeExcludeFilters(WebMvcTypeExcludeFilter.class)
@AutoConfigureCache
@AutoConfigureWebMvc
@AutoConfigureMockMvc
@ImportAutoConfiguration
public @interface WebMvcTest {}
```

@SpringBootTest

- 테스트를 위해 ApplicationContext를 로딩하며 기본값으로는 가짜 웹환경을 만들어 실행된다.
- embedded server를 원하는 경우 설정을 통해 실행할 수 있다.

```java
//random port 로 실행
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
```

**MockMvc**

(출처 - [스프링](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#spring-mvc-test-framework))

MockMvc

- MockMvc는 Spring MVC 애플리케이션의 대부분의 기능을 확인할 수 있는 서버 측 테스트 프레임워크이다.

MockMvcRequestBuilders

- GET, POST, PUT, DELETE 등 request테스트를 진행할 수 있다.

MockMvcResultHandlers

- HTTP 요청에 대한 결과를 확인할 수 있다.

MockMvcResultMatchers

- status, header, content 등 http응답과 관련된 결과를 테스트할 수 있다.

ResultActions

- 실제 결과가 기대했던 결과인지 확인할 수 있다.

## 통합테스트

통합테스트는 아래와 같은 장단점을 가지고 있기때문에 적절하게 단위테스트와 함께 사용해야 한다.

### 장단점

**장점**

- 단위테스트로 확인할 수 없는 부분까지 커버하여 테스트할 수 있다.
- 실제 웹환경과 유사한 환경에서 테스트를 진행할 수 있다.

**단점**

- 단위테스트보다 속도가 오래 걸린다.
