# 분산을 고려한 MySQL 운용
- OS 캐시 활용
    - 전체 데이터 크기에 주의
        - 데이터량 < 물리 메모리를 유지
        - 메모리가 부족할 경우에는 증설
    - 스키마 설계가 데이터 크기에 미치는 영향을 고려

- 인덱스
    - 인덱스 = 색인
    - 알고리즘, 데이터 구조에서 탐색을 할 때는 기본적으로 트리가 널리 사용
    - MySQL의 인덱스는 기본적으로 B+트리
    - 인덱스의 계산량 O(n) -> O(log n)
    - 계산량 측면에서 개선될 뿐만 아니라 디스크 구조에 최적화된 인덱스를 사용해서 탐색함으로써 디스크 seek 횟수면에서도 개선
    - MySQL에서는 한 번의 쿼리에서 하나의 인덱스만 사용

- MySQL 분산
    - 레플리케이션 기능
        - 마스터 슬레이브

- MySQL 스케일아웃과 파티셔닝
    - 파티셔닝 (테이블 분할)
    - 조인 배제
    - 단점
        - 운용이 복잡
        - 고장률
        - 퀴즈 서버대수가 4대가 좋은이유?
            - 1대 마스터 슬레이브 2 일경우
            - 서버1대가 고장 -> 1대 마스터 1대 슬레이브
            - 새로운 서버에 복사를 하기 위해서는 1대가 중지 되어야함