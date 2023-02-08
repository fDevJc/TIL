## What

실제로 필요한 경우 연산을 시작

---

## Why

필요하지 않는 연산을 하지 않기때문에 성능적으로 이점을 얻을 수 있다.

---

## How

```java
import java.util.function.Supplier;

public class LazyEval {

	public String run1(String s1, String s2) {
		//eager evaluation
		boolean result1 = compute(s1);
		boolean result2 = compute(s2);

		return result1 && result2 ? "true!!" : "false!!";
	}

	public String run2(String s1, String s2) {
		//lazy evaluation
		return compute(s1) && compute(s2) ? "true!!" : "false!!";
	}

	public String run3(String s1, String s2) {
		//after java8 use Supplier
		Supplier<Boolean> result1 = () -> compute(s1);
		Supplier<Boolean> result2 = () -> compute(s2);

		return result1.get() && result2.get() ? "true!!" : "false!!";
	}

	private boolean compute(String str) {
		System.out.println("compute exec : str = " + str);
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		return str.contains("a");
	}
}

```

---

## Conclusion

Lazy Evaluation 방식은 Java8의 Stream에서 기본적으로 사용하고있는 방식. Stream의 경우 각 단계에서 모든 원소에 대해 연산을 진행하지 않는다. 

Lazy Evaluation을 활용하여 좀더 성능적으로 좋은 코드를 만드는 방법을 고민해보자!

---

## Reference

https://sabarada.tistory.com/153