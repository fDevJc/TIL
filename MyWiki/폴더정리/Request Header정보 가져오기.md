1. 특정 헤더 정보만 가져오기
```java
public ResponseEntity<ApiResponse> getGoals(@RequestHeader("Authorization") String authorization) {}
```

2. 모든 헤어 정보 가져오기
```java
public ResponseEntity<ApiResponse> getGoals(@RequestHeader HttpHeaders header) {
    header.get("Authorization");
}
```
