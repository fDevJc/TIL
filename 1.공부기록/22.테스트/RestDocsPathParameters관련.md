## 상황
- RestDocs 사용중 PathParameters를 추가하니 에러 발생
- `urlTemplate not found. If you are using MockMvc did you use RestDocumentationRequestBuilders to build the request?`

## 해결
- [공식문서](https://docs.spring.io/spring-restdocs/docs/1.0.0.BUILD-SNAPSHOT/reference/html5/#documenting-your-api-path-parameters)
>To make the path parameters available for documentation, the request must be built using one of the methods on RestDocumentationRequestBuilders rather than MockMvcRequestBuilders.

- 의존성 변경
    - `import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;`
    - `import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.*;`
