- 스프링 배치는 잡을 관리하는 기능, 실행시키는 주체는 아니다
- 스케줄러와 다른 개념

- 스텝
    - 배치 처리를 정의하고 제어하는 독립된 작업의 단위
    - Tasklet Step 방식
        - tasklet
        - 간단히 정의한 하나의 작업 처리
    - Chunk-oriented Step
        - ItemReader
        - ItemProcessor
        - ItemWriter
        - 한번에 하나씩 데이터를 읽고 chunk를 만든 후 chunk 단위로 트랜잭션을 처리

- Job
    - 처음부터 끝까지 독립적으로 실행할 수 있으며 고유하고 순서가 지정된 여러 스텝들의 모음

~P5.ch03.03