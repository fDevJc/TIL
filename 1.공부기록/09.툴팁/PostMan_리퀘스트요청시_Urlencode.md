## PostMan을 사용하여 API 테스트 진행시 파라미터 UrlEncoding 하는 방법

- Request의 Pre-request Script
```javascript
var querycount = pm.request.url.query.count();
for(let i = 0; i < querycount; i++) {
  pm.request.url.query.idx(i).value = encodeURIComponent(pm.request.url.query.idx(i).value);
}
```