# 스프링 MVC 1편 - 백엔드 웹 핵심 기술 정리

인프런 김영한님의 스프링 MVC 1편을 듣고, 정리 겸 복습한 내용입니다.

## 정리

- @ServletComponentScan
    - 스프링 부트는 서블릿을 직접 등록해서 사용할 수 있도록 애노태이션 지원
- 아래와 같이 서블릿 등록

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {}
```

![스크린샷 2022-05-10 오전 10.12.18.png](https://user-images.githubusercontent.com/77914416/167566878-2d0322b4-5f7c-4676-af98-d5e61c454201.png)

- HTTPServletRequest 역할
    - 서블릿은 HTTP 요청 메시지를 개발자 대신 파싱한다.
    - 그 결과를 HttpServletRequest 객체에 담아 제공한다.
    
    ```java
    httpServletRequest.getMethod();
    httpServletRequest.getProtocol();
    httpServletRequest.getRequestURL();
    httpServletRequest.isSecure();
    httpServletRequest.getHeaderNames();
    httpServletRequest.getCookies();
    httpServletRequest.getContentType();
    ...
    //요청 메시지를 확인할 수 있는 여러 메서드 제공
    ```
    
- HTTP 요청
    - GET: 쿼리 파라미터
        - /url?query=value
        - 메시지 바디없이, URL의 쿼리 파라미터에 데이터 전달
        - 서버에서 조회방법
        
        ```java
        //단일
        request.getParameter(name);
        //복수
        request.getParameterValues();
        reqeust.getParameterNames();
        ```
        
    - POST: HTML Form
        - content-type: application/x-www-form-urlencoded
        - 메시지 바디에 쿼리 파라미터 형식으로 전달
        - 서버에서 조회방법: 쿼리파라미터 형식으로 위의 쿼리파라미터 방식으로 동일하게 조회가능
    - HTTP 메시지 바디에 데이터를 직접 담아 전달
        - JSON, XML, TEXT 등
        - 주로 JSON 사용
        - 서버에서 조회방법: InputSteam을 사용해서 직접 읽을 수 있다.
        
        ```java
        import com.fasterxml.jackson.databind.ObjectMapper;
        
        ServletInputStream inputStream = request.getInputStream();
        //인코딩 방식을 utf-8로
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF-8);
        
        //messageBody가 JSON형식일 경우 jackson 라이브러리의 ObjectMapper클래스를 활용하여 객체로 받을 수 있다.
        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
        
        ```
        
- HTTPServletResponse 역할
    - HTTP 응답메시지 생성: 응답코드, 헤더, 바디
    - 기본 사용법
    
    ```java
    response.setStatus(HttpServletResponse.SC_OK);
    
    response.setHeader("Content-Type", "text/plain;charset=utf-8");
    ...
    
    response.setContentType("text/plain");
    response.setCharacterEncoding("utf-8");
    ...
    
    Cookie cookie = new Cookie("name","value");
    cookie.setMaxAge(600);
    response.addCookie(cookie);
    ...
    
    response.sendRedirect("/.../...");
    ```
    
- HTTP 응답 데이터
    - 단순 텍스트 응답
    - HTML 응답
        - content-type을 text/html로 지정해줘야 한다.
    - HTTP API: MessageBody JSON 응답
        - content-type을 application/json로 지정해줘야 한다.
        - Jackson 라이브러리 사용  객체 → 문자(JSON)
        
        ```java
        HelloData helloData = new HelloData();
        String result = objectMapper.writeValueAsString(helloData);
        ```
        
- 템플릿 엔진의 필요성
    - 서블릿과 자바 코드만으로 HTML을 동적으로 생성하여 클라이언트에 제공 가능
    - 하지만 매우 복잡하고 비효율 적
    - 그래서 템플릿 엔진이 필요, HTML문서에서 필요한 곳만 코드를 적용해 동적으로 변경가능
    - JSP, Thymeleaf, Freemarker, Velocity 등이 있다.
- 서블릿과 템플릿 엔진의 조합 한계
    - 서블릿만으로 동적 HTML을 만들때 자바 코드가 지저분
    - JSP를 사용했을 때 JSP에 자바코드, 화면 코드 지저분
    - MVC 패턴의 등장, JSP는 화면(View)에 집중하도록
- MVC 패턴 개요
    - 너무 많은 역할
        - 하나의 서블릿이나 JSP에서 화면 + 비즈니스 로직 처리
    - 변경의 라이프 사이클
        - UI 로직 변경, 비즈니스 로직 변경이 같은 변경 라이프 사이클에 들어있다.(유지보수 힘듬)
    - 기능 특화
        - 위의 문제로 JSP등과 같은 뷰 템플릿은 화면을 렌더링하는 역할만 담당 하도록
- MVC ( Model + View + Controller )
    - 컨트롤러: HTTP 요청 검증, 비즈니스 로직 실행, 뷰에 데이터 전달
    - 뷰: 전달받은 데이터 렌더링에 집중
    - 모델: 뷰에 출력할 데이터를 담아둠
    - 컨트롤러가 너무 많은 일을 담당하기에 service, resository 등 별도의 계층을 만듬
    - 서버 소스
    
    ```java
    Member member = new Member(username, age);
    
    //Model에 데이터를 보관한다.
    //컨트롤러: request.setAttribute로 데이터 보관
    //뷰    : request.getAttribute로 데이터 조회
    request.setAttribute("member", member);
    
    String viewPath = "/WEB-INF/views/new-form.jsp";
    RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
    //다른 서블릿이나 JSP로 이동할 수 있는 기능, 서버 내부에서 다시 호출이 발생
    dispatcher.forward(request, response);
    ```
    
- 현 MVC 패턴의 한계
    - 컨트롤러 역할과 뷰를 렌더링하는 역할이 구분 되어짐
    - 하지만 중복코드가 많이 발생
    
    ```java
    //포워드 중복
    RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
    dispatcher.forward(request, response);
    
    //뷰패스 중복
    String viewPath = "/WEB-INF/views/new-form.jsp";
    ```
    
    - 사용하지 않는 코드
        - HttpServletRequest , HttpServletResponse 등은 사용할때도 있고, 사용하지 않을때도있다
        - 해당 파라미터들의 경우 테스트 케이스 작성도 어렵다
    - 공통 처리가 어렵다
        - 공통기능 즉 수문장 역할이 필요하다.
        - 이러한 이유로 프론트 컨트롤러(Front Controller)패턴이 도입되었다
        - 스프링 MVC의 핵심도 이 프론트 컨트롤러에 있다.
- 프론트 컨틀롤러 도입 전
    
    ![스크린샷 2022-05-10 오전 11.18.26.png](https://user-images.githubusercontent.com/77914416/167566892-b543f518-08eb-4a23-a73d-d43830e3a81e.png)
    
- 프론트 컨트롤러 도입 후
    
    ![스크린샷 2022-05-10 오전 11.18.38.png](https://user-images.githubusercontent.com/77914416/167566904-ca34cdff-5f2e-4bfb-beff-804cc5efc704.png)
    
- 프론트 컨트롤러 패턴 특징
    - 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
    - 프론트 컨트롤러 요청에 맞는 컨트롤러를 찾아서 호출
    - 공통처리 가능
    - 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨
- 스프링 웹 MVC와 프론트 컨트롤러
    - 스프링 웹 MVC 패턴의 DispatcherServlet이 FrontController 패턴으로 구현되어있음
- 구현
    - V1: 프론트 컨트롤러 구현
        - 프론트 컨트롤러에서 url 패턴으로 분기하여 각 컨트롤러를 호출 각 컨트롤러는 인터페이스 구현
    - V2: view 구현
        - View분리 뷰패스 관련 중복 제거
    - V3: model 구현, view 중복제거
        - 컨트롤러의 서블릿 종속성(HttpServletRequest, HttpServletResponse) 제거
        - Model을 대신 사용하여 종속성 제거, 구현코드 단순, 테스트 코드 작성 쉬움
        - prefix, suffix 뷰 패스 코드 중복 제거 ( 논리 뷰 이름 )
            - 뷰 리졸버: 논리 뷰 이름을 실제 물리 뷰 경로로 변경
    - V4: ModelAndView를 반환하는 것이아니라 논리 뷰 이름만 반환
    - V5: 어댑터 패턴 적용
        - 다양한 컨트롤러 인터페이스를 이용할 수 있도록
        - 컨트롤러 → 핸들러
            - 이전 버전에서는 컨트롤러를 직접 매핑 사용
            - 현재는 어댑터를 사용, 컨트롤러 뿐 아니라 어댑터가 지원하면 어떤것이라도 가능
            - 더 넓은 범위의 핸들러라는 개념으로 이름 변경
    - 프레임워크나 공통 기능은 수고로워야 사용하는 개발자가 편리하다!!!!!!!!!!!!!!!!!!
- SpringMVC 구조
    
    ![스크린샷 2022-05-10 오전 11.40.39.png](https://user-images.githubusercontent.com/77914416/167566910-bd508fda-4668-4727-a5b8-67e419f32f0d.png)
    

- 동작순서
    1. 핸들러 조회: 핸들러 매핑을 통해 URL에 매핑된 핸들러 조회
    2. 핸들러 어댑터 조회: 핸들러를 실행할 수 있는 핸들러 어댑터 조회
    3. 핸들러 어댑터 실행:
    4. 핸들러 실행:
    5. ModelAndView 반환: 핸들러가 반환한 정보를 핸들러 어댑터가 ModelAndView로 변환 반환
    6. ViewResolver 호출: 뷰 리졸버를 찾고 실행
        - JSP의 경우: InternalResouceViewResolver가 자동 등록, 사용됨
    7. View 반환: 뷰 리졸버는 뷰 논리이름을 물리 이름으로 바꾸고 뷰 객체를 반환
    8. 뷰 렌더링: 반환된 뷰 객체를 통해 뷰를 렌더링
- DispatcherServlet
    - 프론트 컨트롤러 패턴으로 구현
    - DispatcherServlet→FrameworkServlet→HttpServletBean→HttpServlet
    - 요청 흐름
        - 서블릿 호출이 호출되면서 httpServlet의 service() 호출
        - framworkServlet에서 오버라이드한 service()호출
        - 여러 메서드가 호출되면서
        - dispatcherServlet의 doDispatch() 호출
        - doDispatch() 디버깅으로 따라가보기
- 스프링 부트가 자동 등록하는 핸들러 매핑과 핸들러 어댑터
    - HandlerMapping
        1. RequestMappingHandlerMapping
            - 애노테이션 기반 컨트롤러인 @RequestMapping에서 사용
        2. BeanNameUrlHandlerMapping
            - 스프링 빈의 이름으로 핸들러를 찾음
    - HandlerAdapter
        1. RequestMappingHandlerAdapter
            - 애노테이션 기반의 컨트롤러 @RequestMapping에서 사용
        2. HttpRequestHandlerAdapter
            - HttpRequestHandler 처리
        3. SimpleControllerHandlerAdapter
            - Controller 인터페이스( 애노테이션 X, 과거 사용 ) 처리
- 뷰 리졸버
    - 스프링 부트는 InternalResourceViewResolver라는 뷰 리졸버를 자동으로 등록
    - application.properties에 등록한 정보 사용
    
    ```java
    spirng.mvc.view.prefix=/WEB/INF/views
    spring.mvc.view.suffix=.jsp
    ```
    
- 스프링 부트가 자동 등록하는 뷰 리졸버
    1. BeanNameViewResolver
        - 빈 이름으로 뷰를 찾아서 반환. ( 예: 엑셀 파일 생성 기능에 사용 )
    2. InternalResourceViewResolver
        - JSP를 처리할 수 있는 뷰 반환
    - InternalResourceViewResolver는 JSTL 라이브러리가 있으면
    - InternalResouceView를 상속받은 JstlView반환
    - 다른 뷰는 실제 뷰를 렌더링하지만 JSP의 경우 forward()를 통해 해당 JSP이동(실행)
    - Thymeleaf 뷰 템플릿의 경우 ThymeleafViewResolver등록
    - 최근에는 라이브러리만 등록하면 스프링 부트가 자동화
- @RequestMapping
    - 스프링은 해당 애노테이션을 사용하는 실용적인 컨트롤러를 만들었다.
    - GetMapping, PostMapping 등
    - 파라미터
        - consume
        
        ```java
        @PostMapping(value = "/mapping-consume", consumes = "application/json")
        //HTTP 요청의 Content-Type 헤더를 기반으로 미디어 타입으로 매핑
        //만약 맞지않으면 HTTP 415(Unsupported Media Type)을 반환
        ```
        
        - produces
        
        ```java
        @PostMapping(value = "/mapping-produce", produces = "text/html")
        //HTTP요청의 Accept 헤더를 기반으로 미디어 타입으로 매핑
        //만약 맞지않으면 HTTP 406(Not Acceptable)을 반환
        ```
        
    - 컨트롤러의 파라미터: 다양한 파라미터 제공
        
        ```java
        @RequestMapping("/headers")
        public String headers(
        	HttpServletRequest request,
        	HttpServletResponse response,
        	HttpMethod httpMethod,
        	Locale locale,
        	@RequestHeader MultiValueMap<String, String> headerMap
        	@RequestHeader("host") String host,
        	@CookieValue(value = "myCookie", required = false) String cookie
        ){}
        ```
        
- RequestMappingHandlerMapping
    - 스프링 빈 중 @RequestMapping, @Controller가 클래스 레벨에 붙어있는경우 매핑정보 인식
- ModelAndView
    - 스프링이 제공하는 ModelAndView
    - Model데이터를 추가할때 addObject()사용
    
    ```java
    ModelAndView mv = new ModelAndView("save-result");
    mv.addObject("member", member);
    ```
    
- @RequestParam()
    - request.getParameter() 같은효과
    
    ```java
    public String save(@RequestParam("username") String name){}
    //변수명과 HTTP파라미터 이름이 같으면 생략가능
    public String save(@RequestParam String username){}
    //String, int 등 단순 타입이면 다 생략가능
    public String save(String username){}
    ```
    
    - RequestParam(required = true, false , defaultValue = “”)
        - required: 파라미터 필수 여부, 기본값은 true
        - defalutValue: 디폴트 값
    - Map 사용
        
        ```java
        public String paramMap(@RequestParam Map<String, Object> paramMap) {}
        ```
        
- 스프링부트를 사용하면 로깅라이브러리 spring-boot-starter-logging 자동등록
- 스프링부트 기본 로깅 라이브러리
    - SLF4J(통합인터페이스) - [http://www.slf4j.org](http://www.slf4j.org/)
    - Logback(구현라이브러리) - [http://logback.qos.ch](http://logback.qos.ch/)
    - 로그 레벨 설정
    
    ```java
    #전체 로그 레벨 설정(기본 info) 
    logging.level.root=info
    #hello.springmvc 패키지와 그 하위 로그 레벨 설정 
    logging.level.hello.springmvc=debug
    ```
    
    - 올바른 로그 사용법
        - log.info(”data= ” + data) : 사용 X, 문자 더하기 연산이 추가로 발생
        - log.info(”data={}”, data) : 사용 권장, 의미없는 연산 발생 X
    - 스프링부트가 제공하는 로그 기능
        - [https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging)
- @RestController
    - @Controller는 반환 값이 String이면 뷰이름으로 인식
        - @ResponseBody와 같이 사용할경우 RestController와 동일한 효과
    - @RestController는 HTTP 메시지 바디에 바로 입력
- @PathVariable()
    - URL경로에 매칭되는 부분을 조회
    - 사용
    
    ```java
    @GetMapping("/mapping/{userId}")
    public String mapping(@PathVariable("userId") String id){}
    //변수명과 이름이 같을경우 생략가능
    public String mapping(@PathVariable String userId){}
    ```
    
- @ModelAttribute()
    - 요청 파라미터를 객체에 바로 바인딩 해준다.
    - 사용
    
    ```java
    public String modelAttribute(@ModelAttribute HelloData helloData) {}
    //생략 가능
    public String modelAttribute(HelloData helloData) {}
    ```
    
    - 요청 파라미터의 이름으로 객체의 프로퍼티를 찾는다
    - 그리고 해당 프로퍼티의 setter를 호출해서 값을 바인딩 한다.
    - BindException
        - ex) 숫자가 들어가야 할 곳에 문자를 넣는 등 할때 에러 발생
- HttpEntity
    - 메시지 바디 정보를 직접 조회, 반환 할 수 있다.
    - HttpMessageConverter 사용 → StringHttpMessageConverter 적용
    - 사용
    
    ```java
    public HttpEntity<String> requestBodyString(HttpEntity<String> httpEntity) {
    		String messageBody = httpEntity.getBody();
    		return new HttpEntity<>("ok");
    }
    ```
    
    - HttpEntity를 상속받은 객체
        - RequestEntity
        - ResponseEntity
    - 객체 반환 사용
    
    ```java
    public String requestBodyJson(HttpEntity<HelloData> httpEntity) {
          HelloData data = httpEntity.getBody();
          log.info("username={}, age={}", data.getUsername(), data.getAge());
          return "ok";
    }
    ```
    
- @RequestBody
    - 사용
    
    ```java
    public String requestBodyString(@RequestBody String messageBody) {
        log.info("messageBody={}", messageBody);
        return "ok";
    }
    ```
    
    - 편리하게 HTTP 메시지 바디 정보를 조회할 수 있다.
    - 헤더 정보가 필요하면 HttpEntity 또는 @RequestHeader를 사용하면 된다.
    - 객체 변환 사용
    
    ```java
    //@RequestBody 생략 불가능
    //생략할경우 @ModelAttribute가 되버림
    //HttpMessageConverter 사용 
    //->MappingJackson2HttpMessageConverter 적용(content-type=application/json일때)
    public String requestBodyJson(@RequestBody HelloData data) {
        log.info("username={}, age={}", data.getUsername(), data.getAge());
        return "ok";
    }
    ```
    
- @ResponseBody
    - 응답결과를 HTTP 메시지 바디에 직접 담아서 전달
    - 객체 반환 사용
    
    ```java
    @ResponseBody
    @PostMapping("/request-body-json-v5")
    public HelloData requestBodyJsonV5(@RequestBody HelloData data) {
        log.info("username={}, age={}", data.getUsername(), data.getAge());
        return data;
    }
    ```
    
- HTTP 메시지 컨버터
    - 뷰 템플릿으로 HTML을 생성해서 응답하는 것이 아니라,
    - HTTP API 처럼 JSON 데이터를 HTTP메시지바디에 읽거나 쓰는경우
    - HTTP 컨버터를 사용하면 편리하다.
    - 스프링 부트 기본 메시지 컨버터
        1. ByteArrayHttpMessageConverter
        2. StringHttpMessageConverter
        3. MappingJackson2HttpMessageConverter
- RequestMappingHandlerAdapter 동작 방식
    
    ![스크린샷 2022-05-10 오후 3.35.40.png](https://user-images.githubusercontent.com/77914416/167566921-6ad51def-32ea-411e-a009-0421c718717f.png)
    
- ArgumentResolver ( HandlerMethodArgumentResolver )
    - 애노테이션 기반의 컨트롤러가 매우 다양한 파라미터를 사용할 수 있는 이유
    - 파라미터 목록 참고: [https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments)
    - @RequestBody, @ResponseBody가 있으면
        - RequestResponseBodyMethodProcessor (ArgumentResolver)
    - HttpEntity가 있으면
        - HttpEntityMethodProcessor (ArgumentResolver)
- ReturnValueHandler ( HandlerMethodReturnValueHandler )
    - 응답 값 목록 참고: [https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-return-types](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-return-types)

---

## 참고

- [application.properties](http://application.properties) 아래와 같이 등록하면 HTTP 요청 메시지를 출력할 수 있다.

```java
logging.level.org.apache.coyote.http11=debug
```

- HTTP 응답에서 Content-Length는 웹 애플리케이션 서버가 자동으로 생성
- JSON 자바 객체 변환
    - JSON 결과를 자바 객체로 변환하려면 Jackson, Gson과 같은 JSON 변환 라이브러리 필요
    - 스프링 부트로 Spring MVC를 선택하면 Jackson 라이브러리 기본 제공
- JSP 라이브러리 추가
    - build.gradle
    
    ```java
    implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
    implementation 'javax.servlet:jstl'
    ```
    
- redirect vs forward
    - 리다이렉트는 클라이언트에 응답이 나갔다가, 클라이언트가 redirect경로로 다시 요청
    - 포워드는 서버내부에서 일어나는 호출, 클라이언트는 인지하지 못함
- 스프링 기능 확장이 필요한경우
    - WebMvcConfigurer를 상속받아 스프링빈으로 등록하면 된다.
- 부트스트랩 참고 : [https://getbootstrap.com/](https://getbootstrap.com/)

출처 : 인프런 김영한 스프링MVC1편 - 백엔드 웹개발 핵심 기술
