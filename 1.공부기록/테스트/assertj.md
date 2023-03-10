## assertThatThrownBy() vs assertThatCode()
- assertThatThrownBy
    >if the the provided ThrowableAssert.ThrowingCallable does not raise an exception, an error is immediately thrown, in that case the test description provided with as(String, Object…) is not honored.
    - 예외가 발생하지 않으면 바로 실패

- assertThatCode
    >The main difference with assertThatThrownBy(ThrowingCallable) is that this method does not fail if no exception was thrown.
    - 예외가 발생하지 않아도 실패하지 않는다
