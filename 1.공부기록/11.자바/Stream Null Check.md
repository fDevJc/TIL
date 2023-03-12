1. Null 자체가 stream에 시작되면 NPE
```java
Optional.ofNullable(someList) .orElseGet(Collections::emptyList).stream() 로 시작
```

2. NULL 제거 리스트 얻고 싶을때
```java
.filter(Objects::nonNull)
```
---
### REF
https://sigmasabjil.tistory.com/m/43