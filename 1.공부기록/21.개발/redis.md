# redis
### redis란?
- 오픈소스
- In-Memory 데이터베이스로써 다양한 자료구조를 제공
    - String, List, Set, Hash, Stream, Sorted set, Geospatial 등
- 메모리 접근이 디스크 접근보다 빠르다
- 데이터가 많은 경우, DB 조회하는 부분을 메모리에 캐싱 하여 성능 향상 가능

### 주의사항
- 캐싱 데이터는 update가 자주 일어나지 않는 데이터가 효과적
- 너무 많은 update가 일어나는 데이터일 경우, DB와의 Sync 비용 발생
- Redis 사용시 반드시 failover에 대한 고려
    - 레디스 장애시 데이터베이스에서 조회, 레디스 이중화 및 백업

### 간단 실습
```shell
# docker
docker run -d -p 6379:6379 --name redis redis
# docker cli 접속
docker exec -it redis redis-cli

127.0.0.1:6379> set id 10
OK
127.0.0.1:6379> get id
"10"
127.0.0.1:6379> del id
(integer) 1
```