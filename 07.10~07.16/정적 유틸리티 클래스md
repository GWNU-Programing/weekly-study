## 인스턴스화를 막으려거든 private 생성자를 사용하라
### Enforce noninstantiability with a private constructor
---
단순히 정적 메서드와 정적 필드만을 담은 클래스를 만들 때 private 생성자를 사용한다.  
객체 지향적이지는 않은 방식이지만, 나름의 쓰임새가 있다.
- `java.lang.Math`와 `java.util.Arrays`처럼 기본 타입 값이나 배열 관련 메서드들을 모아놓은 경우
- `java.util.Colletions`처럼 특정 인터페이스를 구현하는 객체를 생성하는 정적 메서드(혹은 팩터리)를 모아놓는 경우
- final 클래스와 관련한 메서드들을 모아놓은 경우

<br>

이러한 정적 멤버만을 담은 유틸리티 클래스는 인스턴스로 만들어 쓰려고 설계한 게 아니기에 private 생성자를 사용하여 인스턴스화를 막는다.

<br>

private 생성자를 사용하는 이유
- 추상 클래스로 만드는 것만으로는 인스턴스화를 막을 수 없다.
	- 하위 클래스를 만들어 인스턴스화 가능하다. **또한 상속해서 사용하라는 뜻으로 오해할 수 있다.**
- 생성자를 명시하지 않으면 컴파일러가 자동으로 기본 생성자를 만든다.


##### 인스턴스를 만들 수 없는 유틸리티 클래스
```Java 
public class UtilityClass {
    // 기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용).
    private UtilityClass() {
        throw new AssertionError();
    }

    // 나머지 코드는 생략
}
```

꼭 `AssertionError`를 던질 필요는 없지만, 클래스 안에서 실수로 생성자를 호출하는 일을 방지