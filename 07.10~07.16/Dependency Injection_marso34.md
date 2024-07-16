## 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
### Prefer dependency injection to hardwiring resources
---
많은 클래스가 하나 이상의 자원에 의존한다. 가령 <u>맞춤범 검사기</u>는 사전(dictionary)에 의존하는데, 이런 클래스를 [정적 유틸리티 클래스](../07.10~07.16/정적%20유틸리티%20클래스.md)와 **싱글턴**으로 구현한 모습을 드물지 않게 볼 수 있다. (싱글턴 보다는 <u>정적 유틸리티 클래스</u>로 만들 가능성이 높다)

##### 정적 유틸리티(Static Utility Class) 사용 예시
```Java
// 유연하지 않고 테스트하기 어렵다
public class SpellChecker {
	private static final Lexion Dictionary = ...;
	
	private SpellChecker() {} // 객체 생성자
	
	public static boolean isValid(String word) { ... }
	public static List<String> suggestions(String typo) { ... } 
}
```

##### 싱글턴(Singlton) 사용 예시
```Java
// 유연하지 않고 테스트하기 어렵다
public class SpellChecker {
	private final Lexion Dictionary = ...;
	
	private SpellChecker() {} // 객체 생성자
	private static SpellChecker INSTANCE = new SpellChecker(...);
	
	public boolean isValid(String word) { ... }
	public List<String> suggestions(String typo) { ... } 
}
```

두 방식 모두 사전을 하나만 사용한다고 가장한다는 점에서 좋지 않다. 실전에서는 사전이 언어별로 따로 있고 특수 어휘용 사전을 별도로 두기도 한다. 테스트용 사전이 필요할 수도 있다. 즉, 사전 하나로 대응하기는 어렵다. 또한 **사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.**

<br>

이러한 문제를 해결하는 간단한 패턴이 <u>인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식</u>인 **의존 객체 주입(dependency injection)**이다.

##### 의존 객체 주입(Dependency Injection) 예시
```Java
// 유연성과 테스트 용이성을 높인다
public class SpellChecker {
	private final Lexion dictionary;
	
	public SpeelChecker(Lexion dictionary) {
		this.dictionary = Objects.requireNonNull(dictionary);
	}
	
	public boolean isValid(String word) { ... }
	public List<String> suggestions(String typo) { ... } 
}
```
- [Objects.requireNonNull](https://docs.oracle.com/javase/8/docs/api/java/util/Objects.html#requireNonNull-T-)[^1]

[^1]: `Objects` 클래스에서 제공하는 `Null` 체크 메서드


의존 객체 주입 패턴은 단순하면서도 자원이 몇 개든 의존 관계가 어떻든 상관없이 작동한다. 

<br>

아래 모두 동일하게 적용 가능
- 불변 클래스
- [정적 팩터리 메서드](../07.03~07.09/java%20factory%20methods_marso34.md)
- 빌더 패턴
  
<br>

>의존성 객체 주입이 유연성과 테스트 용이성을 개선해주지만 의존성이 많아지면 코드가 어지러워진다. 이럴 때 대거(Dagger), 주스(Guice), **스프링(Spring)** 같은 의존 객체 주입 프레임워크를 사용해 해결할 수 있다. 

<br>

### One More Thing

개발할 때 알아두면 좋은 [개발 용어 발음](https://youtu.be/QhmWV48JuWM).
