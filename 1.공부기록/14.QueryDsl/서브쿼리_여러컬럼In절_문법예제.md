## 예제
```java
queryFactory
			.select(room)
			.from(room)
			.join(deal).on(deal.room.eq(room))
			.where(Expressions.list(deal.room.id, deal.orderNumber).in(
				JPAExpressions
					.select(deal.room.id, deal.orderNumber.min())
					.from(deal)
					.where()
					.groupBy(deal.room.id)))
			.fetch();
```
- 두개의 조건으로 비교시 : Expressions.list().in() 활용
- 서브쿼리 : JPAExpressions 활용
