# 비동기 프로그래밍

### 비동기 프로그래밍 이란?
- Async 한 통신
- 실시간성 응답을 필요로 하지 않는 상황에서 사용 (ex: Notification, Email, Push)
- Main Thread가 Task를 처리하는 것이 아니라 Sub Thread가 Task를 위임하는 행위

### 스프링에서 비동기 프레임워크
- Spring에서는 비동기 프로그래밍을 위해 ThreadPool을 정의할 필요가 있다.
- 비동기는 Main Thread가 아닌 Sub Thread에서 작업이 진행
- Java에서는 ThreadPool을 생성하여 Async 작업을 처리

### ThreadPool 생성 옵션
- ThreadPoolExcutor
- CorePoolSize
    - 최소한의 쓰레드 갯수
- MaxPoolSize
    - 최대의 쓰레드 갯수
- WorkQueue
    - 큐에 task를 저장하고 쓰레드가 하나씩 처리
- KeepAliveTime
    - 최소한의 쓰레드 갯수보다 현재 쓰레드갯수가 많을때 언제 반환할것인지

- CorePoolSize만큼 쓰레드를 생성하고 Queue에 Task를 담고 Queue가 다차면 MaxPoolsize만큼 쓰레드 생성

### ThreadPool 주의사항
- CorePoolSize가 너무 클경우
- exception
    - IllegalArgumentException : 생성 옵션의 validation이 안맞을때
    - NullPointerException : workQueue가 null
