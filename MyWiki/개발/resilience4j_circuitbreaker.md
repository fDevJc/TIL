# CircuitBreaker
## CircuitBreaker 패턴
애플리케이션이 실패할 가능성이 있는 작업을 반복적으로 수행하지 않도록 방지하여 시스템의 다른 서비스까지 장애가 전파되는 것을 막는데 도움을 주는 패턴
## CircuitBreaker의 3가지 형태
### Closed
### Open
### Half-Open
## Resilience4j
함수형 프로그래밍을 위해 설계된 가벼운 라이브러리

### Comparison to Netflix Hystrix
- Hystrix에서 외부 시스템 호출은 HystrixCommand로 래핑되어야 한다
- 실패한 호출을 재시도하거나 호출결과를 캐시하는 데코레이터를 제공
- 필요한 데코레이터만 선택할 수 있다
- CompletableFuture 또는 RxJava를 사용하여 동기식 또는 비동기식으로 실행할 수 있다.
- 많은 호출이 특정 응답 시간 임계값을 초과하면 원격 시스템이 응답하지 않고 예외가 발생하기 전에도 CircuitBreaker가 열릴 수 있다.
- Half-Open상태일때
    - Hystrix는 단일 수행만 수행하고 CircuitBreaker를 닫을지 여부를 결정
    - Resilience4j는 설정한 수의 실행을 수행하고 결과를 설정한 임계값과 비교하여 CircuitBreaker를 닫을지 여부를 결정


## Core Modules
- resilience4j-circuitbreaker: Circuit breaking
- resilience4j-ratelimiter: Rate limiting
- resilience4j-bulkhead: Bulkheading
- resilience4j-retry: Automatic retrying (sync and async)
- resilience4j-cache: Result caching
- resilience4j-timelimiter: Timeout handling

## resilience4j-circuitbreaker
- CLOSED, OPEN 및 HALF_OPEN의 세 가지 정상 상태와 DISABLED 및 FORCED_OPEN 두 가지 특수 상태
    ![Alt text](https://files.readme.io/39cdd54-state_machine.jpg)
- 슬라이딩 윈도우를 사용하여 콜의 결과를 저장, 집계
    - 개수기반, 시간기반 슬라이딩 윈도우를 결정할 수 있다
    - 개수기반 슬라이딩 윈도우 : 마지막 N개를 집계
    - 시간기반 슬라이딩 윈도우 : 마지막 N초를 집계
### 개수기반 슬라이딩 윈도우
- N개의 선형 배열로 구현
### 시간기반 슬라이딩 윈도우
- N개의 선형 배열로 구현
- 호출 결과를 개별적으로 저장하지 않지만 부분 집계 및 전체 집계를 점진적으로 업데이트
> 🤔 공식문서만 봤을때는 개수기반 슬라이딩 윈도우와 시간기반 슬라이딩 윈도우가 잘 이해가 되지않네, 사용하면서 필요하면 더 공부

### Failure rate and slow call rate thresholds
- CLOSED -> OPEN
    - 실패율이 설정한 임계값보다 크거나 같으면
    - 느린호출 비율이 설정한 임계값보다 크거나 같으면
- 기본적으로 모든 예외는 실패로 간주
    - 실패 예외 목록과 무시가능한 예외 목록을 설정할 수 있다
- 실패율과 느린호출 비율은 최소한의 호출이 기록된 경우에 계산 가능
- CircuitBreaker는 OPEN일 때 `CallNotPermittedException`이 있는 호출을 거부합니다.
- 대기 시간이 경과한 후 OPEN -> HALF_OPEN으로 변경되고 설정한 수의 호출을 허용하여 상태 확인
- DISABLED(항상 액세스 허용) 및 FORCED_OPEN(항상 액세스 거부)의 두 가지 특수 상태를 더 지원
- 이 두 상태에서는 서킷 브레이크 이벤트(상태 전환 제외)가 생성되지 않으며 메트릭이 기록되지 않음
- 서킷브레이커 쓰레드 세이프
    - 서킷브레이커는 AtomicReference에 저장
    - 서킷브레이커는 atomic 연산을 사용하여 사이드이펙트가 없는 함수로 상태를 업데이트 한다
    - 슬라이딩 윈도우에서 호출을 기록하고 스냅샷을 읽는 작업은 동기화 된다
- 동시스레드 수를 제한하려면 Bulkhead를 사용(Bulkhead와 Circuitbreaker는 결합가능)
![Alt text](https://files.readme.io/45dc011-Thread1.PNG)
![Alt text](https://files.readme.io/8d10418-Multiplethreads.PNG)

### CircuitBreakerRegistry
- 스레드 안전 및 원자성 보장을 제공하는 ConcurrentHashMap을 기반으로 하는 인메모리 CircuitBreakerRegistry 제공
- CircuitBreakerRegistry를 활용하여 CircuitBreaker 인스턴스 관리(생성 및 검색)

### CircuitBreaker 설정정보 ([링크](https://resilience4j.readme.io/docs/circuitbreaker#create-and-configure-a-circuitbreaker))
- failureRateThreshold
    - 실패율 임계점(백분율)
- slowCallRateThreshold
    - 슬로우콜 비율 임계점(백분율)
- slowCallDurationThreshold
    - 슬로우콜 기준
- permittedNumberOfCallsInHalfOpenState
- maxWaitDurationInHalfOpenState
- slidingWindowType
- slidingWindowSize
- minimumNumberOfCalls
    - 최소호출수
- waitDurationInOpenState
- automaticTransitionFromOpenToHalfOpenEnabled
- recordExceptions
- ignoreExceptions
- recordFailurePredicate
- ignoreExceptionPredicate	