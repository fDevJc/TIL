## Pessimistic Lock(exclusive lock)
- 다른 트랜잭션이 특정 row의 lock을 얻는 것을 방지
    - A트랜잭션이 끝날때까지 기다렷다가 B트랜잭션이 lock을 획득
- 일반 select 문의 경우 lock이 없기 때문에 조회 가능

- JPA에서 사용법 @Lock(LockModeType.PESSIMISTIC_WRITE)

## Optimistic Lock
- lock을 걸지 않고 문제가 발생할 때 처리
- 대표적으로 version cloumn을 만들어서 해결하는 방법

- sever1
    - ```select * from stock where id = 1```
    - version 1의 데이터를 셀렉트
    - ```update set version=version+1, quantity=1 from stock where id=1 and version=1```
- server2
    - ```select * from stock where id = 1```
    - version 1의 데이터를 셀렉트
    - ```update set version=version+1, quantity=1 from stock where id=1 and version=1```
- 위의 server2에서 실행되는 업데이트문의 경우 version이 맞지 않기때문에 업데이트가 실행되지 않는다
- 버전이 맞지 않는 경우 데이터를 다시 읽어오는 로직이 필요하다

- JPA에서 사용법 @Lock(LockModeType.OPTIMISTIC)
    - 테이블에 락을 걸지 않고 개발자가 직접 재시도 로직을 작성해야한다.
    - 충돌이 많을 경우 Pessimistic Lock이 성능이 뛰어날 수 있고 충돌이 적은 경우 Optimistic Lock이 좋은 성능을 발휘 할 수 있다.

## Named Lock
- 이름과 함께 lock을 획득
- 해당 lock은 다른 세션에서 획득 및 해제가 불가능 