### Collectors.joining() 활용
```java
private static String userIdsToString(List<Feed> feeds) {
        return feeds.stream()
                .map(feed -> feed.getUserId().toString())
                .collect(Collectors.joining(","));
    }
```