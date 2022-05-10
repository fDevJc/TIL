# 스프링 MVC 2편 - 검증 정리

**인프런 김영한님의 스프링 MVC 2편을 듣고, 정리 겸 복습한 내용입니다.**

---

## 정리

### 검증

- 컨트롤러의 중요한 역할중 하나는 HTTP 요청이 정상인지 검증하는 것
- 클라이언트 검증, 서버 검증
    - 클라이언트 검증은 조작할 수 있으므로 보안에 취약
        - ex) 간단히 postman으로도 조작 가능하다
    - 서버만으로 검증하면, 즉각적인 고객 사용성이 부족
    - 두가지 검증을 다 사용하되, 서버 검증은 필수 !!
    - API 방식일 경우 API스펙을 잘 정의해서 응답 결과를 넘겨주어야 함
- BindingResult
    - 코드
    
    ```java
    public String addItem(@ModelAttribute Item item, 
    	BindingResult bindingResult,
      RedirectAttributes redirectAttributes) {
    	//필드에러의 경우	
    	bindingResult.addError(new FieldError("item", "itemName", "아이템/네임 필드에러"));
    	//글로벌에러의 경우
    	bindingResult.addError(new ObjectError("item", "아이템 글로벌에러"));
    }
    ```
    
    - FieldError
        
        ```java
        public FieldError(String objectName, String field, String defaultMessage);
        public FieldError(String objectName, String field, 
        	@Nullable Object rejectedValue, 
        	boolean bindingFailure, 
        	@Nullable String[] codes, 
        	Nullable Object[] arguments, 
        	@Nullable String defaultMessage)
        ```
        
        - objectName: @ModelAttribute 이름
        - field: 오류가 발생한 필드 이름
        - defaultMessage: 오류 기본 메시지
        - rejectedValue: 사용자가 입력한 값( 거절된 값 )
            - 사용자가 입력 폼에서 입력한 값이 검증 실패된경우
            - 서버에서 다시 화면을 렌더링할때 값이 없어지게 된다.
            - 그럴때 값을 보관하다가 화면에 다시 전달한다.
        - bindingFailure: 바인딩 실패인지, 검증 실패인지 구분값
        - codes: 메시지 코드
            - 오류 메시지를 찾을 수 있도록 도와준다
        - arguments: 메시지 사용 인자
    - ObjectError
        
        ```java
        public ObjectError(String objectName, String defaultMessage) {}
        ```
        
        - objectName: @ModelAttribute 이름
        - defaultMessage: 오류 기본 메시지
    - BindingResult 파라미터의 위치는 확인할 클래스 바로 다음에 와야한다.
        - ex) 위의 코드의 Item 바로 뒤
    - 스프링이 제공하는 검증 오류 보관 객체
    - BindingResult가 있으면 @ModelAttribute데이터 바인딩시 오류가 발생해도 컨트롤러 호출
        - 없으면 → 400오류 발생하면서 컨트롤러 호출 X , 오류 페이지 이동
        - 있으면 → 오류정보를 BindingResult에 담아서 컨트롤러 정상 호출
    - BindingResult는 Errors 인터페이스 상속
    - 실제 넘어오는 구현체는 BeanPropertyBindingResult
    
- 스프링 부트 메시지 설정추가
    - application.properties
    
    ```java
    string.messages.basename=message,errors
    #message.properties, errors.properties 두개의 프로퍼티를 인식
    ```
    
    - errors.properties
    
    ```java
    required.item.itemName=상품 이름은 필수입니다.
    range.item.price=가격은 {0} ~ {1} 까지 허용합니다. 
    max.item.quantity=수량은 최대 {0} 까지 허용합니다. 
    totalPriceMin=가격 * 수량의 합은 {0}원 이상이어야 합니다. 현재 값 = {1}
    ```
    
- FieldError 두번째 생성자 사용법
    - 코드
    
    ```java
    new FieldError("item", "price", item.getPrice(), false, new String[] {"range.item.price"}, new Object[]{1000, 1000000}
    ```
    
    - 배열을 이용하여 메시지코드를 지정
    - 위의 코드 실행시 **range.item.price=가격은 1000 ~ 10000 까지 허용합니다.** 에러메시지 확인 가능
    - 너무 사용하기 어렵고 번거롭다.
    - 이때 BindingResult가 제공하는 rejectValue(), reject()를 사용할 수 있다.
- rejectValue() , reject()
    - rejectValue(): 코드
    
    ```java
    void rejectValue(@Nullable String field, String errorCode,
            @Nullable Object[] errorArgs, @Nullable String defaultMessage);
    
    //사용
    bindingResult.rejectValue("quantity", "max", new Object[]{9999}, null);
    ```
    
    - reject(): 코드
    
    ```java
    void reject(String errorCode, 
    				@Nullable Object[] errorArgs, @Nullable String defaultMessage);
    //사용
    bindingResult.reject("totalPriceMin", new Object[]{10000,resultPrice}, null);
    ```
    
    - field: 오류
    - errorCode: 오류코드 ( messageResolver 를 위한 오류 코드)
    - errorArgs: 메시지 인자
    - defaultMessage: 기본 메시지
    - BindingResult는 대상이 되는 객체 바로 뒤에 오므로 대상을 알고 있다.
    
- 오류 코드와 메시지 처리
    
    ```java
    required.item.itemName: 상품 이름은 필수 입니다.
    range.item.price: 상품의 가격 범위 오류입니다.
    //또는 단순하게
    required: 필수 값입니다.
    range: 범위 오류 입니다.
    ```
    
    - 위와 같이 단순하게 만들면 범용성이 올라가 공통으로 사용할 수 있지만 세밀하게 작성하기 어렵다.
    - 반대로 너무 자세하게 만들면 범용성이 떨어진다.
    - 가장 좋은 방법은 범용으로 사용하다 세밀한 메시지가 필요한경우 세밀한 내용이 적용되게 하는것이다.
    - 이렇게 우선 순위에 따라 메시지를 관리할 수 있게 해주는게 **MessageCodesResolver**이다.
- MessageCodesResolver
    - 기본 구현체는 DefaultMessageCodesResolver
    - DefaultMessageCodesResolver의 기본 메시지 생성 규칙
        - 객체오류
        
        ```java
        다음 순서로 2가지 생성
        1. code + "." + object name
        2. code
        ex) 오류 코드: required, object name: item
        1. required.item
        2. required
        ```
        
        - 필드오류
        
        ```java
        1. code + "." + object name + "." field name
        2. code + "." + field name
        3. code + "." + field type
        4. code
        ex) 오류 코드: typeMismatch, object name: user, filed name: age, filed type: int
        1. typeMismatch.user.age
        2. typeMismatch.age
        3. typeMismatch.int
        4. typeMismatch
        ```
        
        - 동작 방식
        - rejectValue(), reject()는 내부에서 MessageCodesResolver를 사용, 여기서 메시지 코드 생성
        - FiledError, ObjectError 생성자를 보면 오류코드를 하나가 아니라 배열로 받음
        - MessageCodesResolver를 통해서 생성된 순서대로 오류 코드를 보관
        - 생성된 메시지를 순서대로 찾아서 활용가능
        - 범용성있는 에러의 경우 덜 구체적으로, 특별한경우 구체적으로 적어 관리할 수 있다.

- Validator
    - 컨트롤러에서 검증로직을 분리하기 위하여 도입
    - 스프링은 검증을 체계적으로 제공하기 위해 다음 인터페이스를 제공한다.
    
    ```java
    public interface Validator {
    		//해당 검증기를 지원하는 여부 확인
        boolean supports(Class<?> clazz);
    		//검증 대상 객체와 BindingResult
        void validate(Object target, Errors errors);
    }
    ```
    
    - 활용
    
    ```java
    public class ItemValidator implements Validator {
    		@Override
        public boolean supports(Class<?> clazz) {
            return Item.class.isAssignableFrom(clazz);
        }
    
    		@Override
        public void validate(Object target, Errors errors) {
            Item item = (Item) target;
    				if(...검증로직) {
    						//통과못할경우
    						errors.rejectValue("price","range",new Object...,null);
    				}
    		}
    }
    
    public String addItemV5(@ModelAttribute Item item, 
    		BindingResult bindingResult,
    		RedirectAttributes redirectAttributes) {
    		
    		//검증
    		itemValidator.validate(item, bindingResult);
    		//에러가 있으면
    		if (bindingResult.hasErrors()) {
              log.info("errors={}", bindingResult);
              return "validation/v2/addForm";
    		}
    		//성공 로직
    		...
    }
    ```
    
- WebDataBinder
    
    ```java
    @InitBinder
    pulbic void init(WebDataBinder dataBinder) {
    		dataBinder.addValidators(itemValidator);
    }
    ```
    
    - 위와같이 검증기를 추가하면 해당 컨트롤러에서는 검증기를 자동으로 등록 할수 있다.
    - @InitBinder → 해당 컨트롤러에만 영향을 준다.
- @Validated
    
    ```java
    public String addItem(@Validated @ModelAttribute Item item, BindingResult
      bindingResult, RedirectAttributes redirectAttributes) {}
    ```
    
    - @Validated는 검증기를 실행하라는 애노테이션이다.
    - @Validated, @Valid 둘다 사용가능하다.
    - @Validated는 스프링 전용
    - @Valid는 자바 표준
    
- BeanValidation
    - 사용예시
    
    ```java
    public class Item {
    		private Long id;
    		@NotBlank
    		private String itemName;
    		@NotNull
    		@Range(min=1000, max=100000)
    		private Integer price;
    		@Max(9999)
    		private Integer quantity;
    }
    ```
    
    - BeanValidation은 특정 구현체가 아니라 Bean Validation 2.0(JSR-380)이라는 기술 표준이다.
        - 🤫 JSR
            - Java Specification Requests 자바 스펙 요구사항
            - JSR 전체 리스트 확인: [https://jcp.org/en/jsr/all](https://jcp.org/en/jsr/all)
    - BeanValidation을 구현한 기술중 일반적으로 하이버네이트 벨리데이터 구현체를 사용한다.
        - 공식 사이트: [https://hibernate.org/validator/](https://hibernate.org/validator/)
        - 공식 메뉴얼: [https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/)
        - 검증애노테이션 모음: [https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#validator-defineconstraints-spec](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#validator-defineconstraints-spec)
    - 의존관계 추가
    - build.gradle
    
    ```java
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    ```
    
    - 스프링 MVC가 BeanValidator를 사용하는 방법
        - 스프링부트가 spring-boot-starter-validation 라이브러리를 넣으면 자동으로
        - BeanValidator 를 인지하고 스프링에 통합한다.
        - 스프링부트는 자동으로 글로벌 Validator LocalValidatorFactoryBean을 등록한다.
        - 해당 Validator는 애노테이션을 보고 검증을 수행한다.
        - 글로벌 Validator가 적용되었기 때문에, @Valid, @Validated만 적용하면 된다.
        - 검증 오류가 발생하면 FieldError, ObjectError를 생성하해서 BindingResult에 담아준다.
    - BeanValidation 에러코드
    
    ```java
    NotBlank.item.itemName
    NotBlank.itemName
    NotBlank.java.lang.String
    NotBlank
    ...
    ```
    
    - BeanValidation 오브젝트 오류
    
    ```java
    @Data
    @ScriptAssert(lang = "javascript", script = "_this.price * _this.quantity >= 10000")
    public class Item {
    		//...
    }
    ```
    
    - 위와 같이 사용하지만 실무에서는 활용가능성이 적어보인다.
    - 오브젝트 오류의 경우 자바에서 직접작성
    - 한계
        - 데이터를 등록할때와 수정할때 요구사항이 다를 수 있다.
        - 해결 방법 2가지
            - groups기능 활용
                
                ```java
                @NotBlank(groups = {SaveCheck.class, UpdateCheck.class})
                private String itemName;
                
                public String addItemV2(@Validated(SaveCheck.class) @ModelAttribute Item item ...){}
                ```
                
                - 위와 같이 groups 속성을 이용하여 사용할 check인터페이스 활용
                - 코드의 복잡도가 올라간다. 좋은 방법은 아니다
            - 폼 전송을 위한 별도의 모델 객체를 만들어 사용
                - ItemSaveForm, ItemUpdateForm 등과 같이 데이터 전달을 위한 별도의 객체를 사용
                    - 장점:
                        - 별도의 폼객체를 만들기 때문에 검증이 중복되지않음 한계 극복
                        - 등록 수정 화면도 다르고 입력되는 정보도 다르기때문에 나누는게 좋음
                    - 단점: 별도의 폼객체에서 실제 데이터 객체를 생성하는 과정이 추가됨
                        - ex) Item 객체를 생성해서 서비스 로직에서 사용
                - 😎    이러한 이유로 DTO 등을 만들어사용하는 구나.....!!!!!!!!
    - @Valid , @Validated 는 HttpMessageConverter ( @RequestBody )에도 적용할 수 있다.
    - @ModelAttribute
      - 필드 단위로 정교하게 바인딩이 적용된다. 
      - 특정 필드가 바인딩 되지 않아도 나머지 필드는 정상 바인딩 되고, Validator를 사용한 검증도 적용할 수 있다.
    - @RequestBody
      - HttpMessageConverter 단계에서 JSON 데이터를 객체로 변경하지 못하면 이후 단계 자체가 진행되지 않고 예외가 발생한다. 
      - 컨트롤러도 호출되지 않고, Validator도 적용할 수 없다.
