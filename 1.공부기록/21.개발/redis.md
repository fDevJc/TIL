# redis
### redis란?
- 오픈소스
- In-Memory 데이터베이스로써 다양한 자료구조를 제공
    - String, List, Set, Hash, Stream, Sorted set, Geospatial 등
- 메모리 접근이 디스크 접근보다 빠르다
- 데이터가 많은 경우, DB 조회하는 부분을 메모리에 캐싱 하여 성능 향상 가능

### Cache란?
- 나중에 요청올 결과를 미리 저장해두었다가 빠르게 서비스를 해주는 것을 의미

### 어디서 사용하나
- 추상적인 웹서비스 구조
    - 클라이언트 -> 웹서버 -> 디비
    - 디비 안에는 많은 데이터들이 있고, 이 때 주로 디스크에 내용이 저장
- 구조 1 - Look aside Cache
    - 클라이언트 -> 웹서버 -> 캐시 -> 디비
    - 1. 웹서버는 캐시에 데이터가 존재하는지 먼저  확인
    - 2. 캐시에 데이터가 존재하면 캐시에서 바로 조회
    - 3. 캐시에 대ㅔ이터가 없다면 디비에서 조회
    - 4. 디비에서 얻어온 데이터를 캐시에 다시 저장
    - 메모리 캐시를 이용하게 되면 훨씬 더 빠른 속도로 서비스가 가능하다.
- 구조 2 - Write Back
    - 클라이언트 -> 웹서버 -> 캐시 -> 디비
    - 1. 웹 서버는 모든 데이터를 캐시에만 저장
    - 2. 캐시에 특정 시간동안 데이터 저장
    - 3. 캐시에 있는 데이터를 디비에 저장
    - 4. 디비에 저장된 데이터를 삭제
    - 주의) 캐시에 먼저 저장되기 때문에 장애 발생시 데이터 유실 가능성 존재

### 왜 컬렉션이 중요한가?
- 개발의 편의성
- 개발의 난이도
- 외부의 Collection을 잘 이용하는 것으로, 여러가지 개발 시간을 단축시키고, 문제를 줄여줄 수 있기 때문에 Collection이 중요.

### 랭킹서버를 구현 한다면?
개발의 편의성
- 가장 간단한 방법
    - 디비에 유저의 스코어를 저장하고 스코어로 오더 바이로 정렬 후 읽어오기
    - 개수가 많아지면 속도에 문제가 발생할 수 있음
        - 결국은 디스크를 사용하므로
- 인메모리 기준으로 랭킹 서버의 구현이 필요
    - 레디스의 소티드 셋을 이용하면, 랭킹을 구현 할 수 있음.
        - 덤으로 레플리케이션도 가능
        - 다만 가져다 쓰면 거기의 한계에 종속적이 됨. 
### 친구 리스트를 구현 한다면?
개발의 난이도
    - 친구 리스트를 키/밸류 형태로 저장해야 한다면?
        - 현재 유저 123의 친구 key는 friends:123, 현재 친구 A가 있는 중
        - 경쟁 상태 발생 가능성
    - 레디스의 경우 자료구조가 Atomic 하기 때문에 경쟁상태를 피할 수 있다.
        - 잘못작성하면 발생
    
### Redis 사용 처
- Remote Data Store
    - A서버, B서버, C서버에서 데이터를 공유하고 싶을때
- 한대에서만 필요하다면, 전역 변수를 쓰면 되지 않을까?
    - Redis자체가 Atomic을 보장해준다.(싱글 스레드)
- 주로 많이 쓰는 곳들
    - 인증 토큰 등을 저장(Strings 또는 hash)
    - Ranking 보드로 사용(Sorted set)
    - 유저 API Limit
    - 잡 큐

### 주의사항
- 캐싱 데이터는 update가 자주 일어나지 않는 데이터가 효과적
- 너무 많은 update가 일어나는 데이터일 경우, DB와의 Sync 비용 발생
- Redis 사용시 반드시 failover에 대한 고려
    - 레디스 장애시 데이터베이스에서 조회, 레디스 이중화 및 백업

### Redis Collections
- String
    - 키 밸류
    - 키를 어떻게 잡을 것인가?
        - 키에 따라 분산이 어떻게 되는지 추후 결정될 수 있다. 고민해볼 문제
    - Insert into users(name, email) values('yang','yang@mail.com');
    - Set name:yang yang
    - Set email:yang yang@email.com
- List
    - 리스트
    - 잡 큐
- Set
    - 중복된 데이터 X
- Sorted Set
    - 순서있는 set
    - 키 밸류 외에 score 가 존재 
    - score는 double타입이기 때문에, 값이 정확하지 않을 수 있다.
- Hash

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

### Collection 주의 사항
- 하나의 컬렉션에 너무 많은 아이템을 담으면 좋지 않음
    - 만개 이하 몇천개 수준으로 유지하는게 좋음
- Expire는 Collection의 item 개별로 걸리지 않고 전체 Collection에 대해서만 걸림
    - 즉 해당 만개의 아이템을 가진 Collection에 expire가 걸려있다면 그 시간 후에 만개의 아이템이 모두 삭제

### Redis 운영
- 메모리 관리를 잘하자.
- O(N) 관련 명령어는 주의하자
- Replication
- 권장 설정 Tip

### 메모리 관리를 잘하자
- Redis는 In-Memory Data Store.
- Phusical Memory 이상을 사용하면 문제가 발생
    - Swap이 있다면 Swap 사용으로 해당 메모리 Page 접근시 마다 늦어짐
    - Swap이 없다면? OOM 발생할 수 있다
- Maxmemory를 설정하더라도 이보다 더 사용할 가능성이 큼
- RSS 값을 모니터링 해야함
- 큰 메모리를 사용하는 instance 하나보다는 적은 메모리를 사용하는 instance 여러개가 안전
- 레디스는 메모리 파편화가 발생할 수 있음. 4.x 대 부터 메모리 파편화를 줄이도록 jemalloc에 힌트를 주는 기능이 들어갔으나, jemalloc 버전에 따라서 다르게 동작할 수 있음.
- 3.x대 버전의 경우
    - 실제 used memory는 2GB로 보고가 되지만 11GB의 RSS를 사용하는 경우가 자주 발생
- 다양한 사이즈를 가지는 데이터 보다는 유사한 크기의 데이털르 가지는 경우가 유리

### 메모리가 부족할 떄는?
- 좀 더 메모리 많은 장비로 Migration
- 있는 데이터 줄이기

### 메모리를 줄이기 위한 설정
- 기본적으로 Collection 들은 다음과 같은 자료구조를 사용
    - Hash -> HashTable을 하나 더 사용
    - Sorted Set -> Skiplist와 HashTable을 이용.
    - Set -> HashTable 사용
    - 해당 자료 구조들은 메모리를 많이 사용함.
- Ziplist를 이용하자

### Ziplist 구조
- In-Memory 특성 상, 적은 개수라면 선형 탐색을 하더라도 빠르다.

### O(N) 관련 명령어는 주의하자
- Redis는 Single Thread
    - 단순한 get/set의 경우 초당 10만 TPS 이상 가능
- Packet으로 하나의 Command가 완성되면 processCommand에서 실제로 실행됨
    - 실행되는 동안 다른 packet은 그대로 쌓임
- 대표적인 O(N) 명령들
    - KEYS
    - FLUSHALL, FLUSHDB
    - Delete Collections
    - Get ALL Collections

### KEYS 는 어떻게 대체할 것인가?
- scan 명령을 사용하는 것으로 하나의 긴 명령을 짧은 여러번의 명령으로 바꿀 수 있다.

### Collection의 모든 item을 가져와야 할 때?
- Collection의 일부만 가져오기
- 큰 Collection을 작은 여러개의 Collection으로 나눠서 저장
    - 하나당 몇천개 안쪽으로 저장하는게 좋음

## Redis Replication

### Redis Replication
- Async Replication
    - Replication Lag 이 발생할 수 있다.
- 'Replicaof'(>=5.0.0) or 'slaveof' 명령으로 설정 가능
    - Replicaof hostname port
- DBMS로 보면 statement replication 유사
    - 쿼리가 간다(now 명령어 등을 사용하면 값이 다를 수 있다)
- 과정
    - 세컨더리에서 명령을 전달
    - 세컨더리는 프라이머리에 신크 명령 전달
    - 프라이머리는 현재 메모리 상태를 저장하기 위해 Fork
    - Fork한 프로세서는 현재 메모리 정보를 disk에 dump
    - 해당 정볼르 세컨더리에 전달
    - Fork 이후의 데이터를 세컨더리에 계속 전달
    - diskless stream으로 디스크에 IO 작업없이도 가능
- 주의 할점
    - fork가 발생하므로 메모리 부족이 발생할 수 있다.
    - Redis-cli--rdb 명령은 현재 상태의 메모리 스냅샷을 가져오므로 같은 문제를 발생
    - AWS나 클라우드의 Redis는 좀 다르게 구현되어서 좀더 해당 부분이 안정적
    - 많은 대수의 Redis 서버가 Replica를 두고 있다면
        - 네트워크 이슈나, 사람의 작업으로 동시에 replication이 재시도 되도록하면 문제가 발생할 수 있음

### Redis 데이터 분산
- 데이터의 특성에 따라서 선택할 수 있는 방법이 달라진다.
    - Cache 일때는 좋지만
    - Persistent 해야하면 ...

### Consistent Hashing
- 음 ??

### Sharding
- 데이터를 어떻게 나눌것인가?
- 데이터를 어떻게 찾을것인가?
- 상황마다 샤딩방법은 달라짐
    - Range
        - 놀고있는 서버가 있을 수 있다
    - Modular
        - 2의 배수로 늘리게 되면 데이터의 변화가 크게 없지만 서버수가 기하급수적으로 늘어날수 있음
    - Indexed
        - 인덱스 서버를 따로 둬서 관리
    
