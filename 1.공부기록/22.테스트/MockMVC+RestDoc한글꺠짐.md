## 문제
- RestDoc, MockMvc를 사용하여 Controller의 unit 테스트를 작성하던 중 응답값의 한글이 깨지는 현상 발견
## 해결
```java
@AutoConfigureRestDocs
@WebMvcTest
public abstract class ControllerTest {
	@Autowired
	private WebApplicationContext ctx;
	@Autowired
	private RestDocumentationContextProvider contextProvider;

	@Autowired
	protected MockMvc mockMvc;

	@BeforeEach
	void init() {
		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx)
			.apply(MockMvcRestDocumentation.documentationConfiguration(contextProvider))
			.addFilters(new CharacterEncodingFilter("UTF-8", true))
			.build();
	}
}

```