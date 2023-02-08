## 기본문법
```java
@Query(nativeQuery = true, value = "SELECT * FROM feed as f WHERE f.feed_id in :ids")
List<Feed> findByIds(Pageable page, @Param("ids") List<Long> ids);
```