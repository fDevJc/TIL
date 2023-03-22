## 기본 문법
    
```java
    public List<SampleDto> findSampleDtoCount() {
        return queryFactory
                .select(
                        new SampleDto(sample.name,
                                sample.id.count().as("count"))
                )
                .from(sample)
                .groupBy(sample.name)
                .fetch();
    }                    
```
    
- 최적화관련
    - [Querydsl 에서 Group by 최적화하기 (feat. MySQL)](https://jojoldu.tistory.com/477)