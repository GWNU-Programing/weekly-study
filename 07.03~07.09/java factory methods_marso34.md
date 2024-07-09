# 생성자 대신 정적 팩터리 메서드를 고려하라
## Consider static factory methods intead of constructors 

클래스의 인스턴스를 얻는 전통정인 방법은 public 생성자다. 클래스는 생성자와 별도로 클래스의 인스턴스를 반환하는 정적 팩터리 메서드(static factory method)를 제공할 수 있다. 

> *디자인 패턴 중 팩터리 메서드(Factory Mehod)와는 다름*

<br>

###### Boolean (boolean boxed class) 예시
```
public static Boolean valueOf(boolean b) {
	return b ? Boolen.TURE : Boolean.FALSE;
}
```

<br>

#### 정적 팩터리 메서드의 장점
1. 이름을 가질 수 있다
   
   **정적 팩터리 메서드는 이름을 가질 수 있기에 아래와 같은 생성자의 단점을 해결할 수 있다.**
   - 생성자의 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 알 수 없다.
   - 생성자는 하나의 시그니처만 사용이 가능하다.
     ```
     public class Test {
	     private String title;
	     private String number;
	     
	     /**
	      * 하나의 시그니처만 사용이 가능하므로 아래와 같은 생성자는 같이 사용할 수 없다.
	      * 
	      */
	     public Test(String title) {
		     this.title = title;
	     }
	     
	     public Test(String number) {
		     this.number = number;
	     }
     }
     ```
    
2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다
	  -  **불변 클래스(immutable class)** 는 미리 만들어놓은 인스턴스를 재활용(캐싱)하여 불필요한 객체 생성을 피할 수 있다.
		  - [Boolean 예시 참고](#boolean-boolean-boxed-class-예시)
		  - `Boolean`에서 `Boolen.TRUE` 와 `Boolen.FALSE` 는 상수이기에 객체를 아예 생성하지 않음 
	  - **플라웨이트 패턴(Flyweight pattern)** 도 유사한 기법  *나중에 플라웨이트 패턴에 대한 문서 작성*
	  - **인스턴스 통제 클래스(Instance-Controlled Class)**: 정적 팩토리 방식의 클래스는 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제 가능
		  - 싱글턴(Singleton) / 인스턴스화 불가(noninstantible)로 만들 수 있음
		  - **불변 클래스(immutable class)** 에서 동치 인스턴스 하나임을 보장 (a == b 일 때만 a.equals(b)가 성립)
		  - 열거타입은 인스턴스가 하나만 만들어짐을 보장
		
3. 반환(리턴) 타입의 하위 타입을 반환할 수 있는 능력이 있다.
    - 반환할 객체의 클래스를 자유롭게 선택할 수 있다. **(유연성)**
		```
		List<String> list = new ArrayList<>();
		```
    - **인터페이스 기반 프레임워크**: 인터페이스를 정적 펙토리 메서드의 반환 타입으로 사용
	    - 인터페이스를 구현한 모든 클래스를 공개하는 것이 아닌 인터페이스만을 공개할 수 있다.
		```
		// java9 List of()
		static <E> List<E> of() {
		    return (List<E>) ImmutableCollections.ListN.EMPTY_LIST;
		}
		```
	
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
   - 하위 타입 클래스이기만 하면 어떠한 클래스의 객체를 반환하든 상관 없다.
   -  `EnumSet` 은 public 생성자 없이 오직 정적 팩터리만으로 제공. (원소 수가 64개 이하면 `RegularEnumSet`의 인스턴스를 65개 이상이면 `JumboEnumSet`의 인스턴스를 반환)
        ```
        public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
            implements Cloneable, java.io.Serializable
        {
            //...
            /**
            * Creates an empty enum set with the specified element type.
            *
            * @param <E> The class of the elements in the set
            * @param elementType the class object of the element type for this enum
            *     set
            * @return An empty enum set of the specified type.
            * @throws NullPointerException if <tt>elementType</tt> is null
            */
            public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
                Enum<?>[] universe = getUniverse(elementType);
                if (universe == null)
                    throw new ClassCastException(elementType + " not an enum");
        
                if (universe.length <= 64)
                    return new RegularEnumSet<>(elementType, universe);
                else
                    return new JumboEnumSet<>(elementType, universe);
            }
        
        //...
        }
        ```
    
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
    - **서비스 제공자 프레임워크(Service Provider Framework)** 의 근간이 되는 유연성을 가진다. 
      대표적인 서비스 제공자 프레임워크는 JDBC(Java Database  Connectivity)
    - **서비스 제공자 프레임워크** 의 3개 핵심 컴포넌트 
	    - **서비스 인터페이스**(service interface) : 구현체의 동작 정의
	    - **제공자 등록 API**(provider registration API) : provider가 구현체를 등록할 때 사용
	    - **서비스 접근 API**(service access API) : 클라이언트는 서비스 접근 API 사용시 원하는 구현체의 조건을 명시할 수 있음 <br>
	    **+  서비스 제공자 인터페이스**(service prvider interface) : 서비스 인터페이스의 인스턴스를 생성하는 펙토리 객체를 설명.
    - 클라이언트는 서비스 접근 API 사용시 원하는 구현체의 조건을 명시할 수 있는 점은 Service Provider Framework가 유연한 정적 팩토리라고 할 수 있는 실체이다.   
    
<br>

#### 정적 팩터리 메서드의 단점

1. 정적 팩터리 메서드만 제공하면 상속을 할 수 없다.
	- 상속을 위해서는 `public` or `protected` 생성자가 필요하다 (따라서 컬렉션 프레임워크의 유틸리티 구현체 클래스들은 상속할 수 없다.)
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
	- 생성자처럼 API 설명에 명확히 드러나지 않기에 클래스를 인스턴스화할 방법을 알아내야 한다. (API 문서화를 잘 해야함)
	
<br>

#### 주로 사용하는 명명 방식

|                메서드                | 설명                                                                                           | 예제                                                          |
| :-------------------------------: | :------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
|              `from`              | 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드                                                       | `Date d = Date.from(instant);`                              |
|               `of`                | 여러 매개변수를 받아 적합한 타임의 인스턴스를 반환하는 집계 메서드                                                        | `Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);`      |
|             `valueOf`             | `from`과 `of`의 더 자세한 버전                                                                       | `BigInteger.valueOf(Integer.MAX_VALUE);`                    |
| `instance`<br>or<br>`getInstance` | (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.                                     | `StackWalker luke = StackWalker.getInstance(options);`      |
|  `create`<br>or<br>`newInstance`  | `instance` 혹은 `getInstance`와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.                                | `Object newArr = Array.newInstance(classObj.arrayLen);`     |
|             `getType`             | `getInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. *"Type"* 은 팩터리 메서드가 반환할 객체의 타입이다. | `FileStore fs = Files.getFileStore(path)`                   |
|             `newType`             | `newInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스의 팩터리 메서드를 정의할 때 쓴다. *"Type"* 은 팩터리 메서드가 반환할 객체의 타입이다. | `BufferedReader br = Files.newBufferedReader(path);`        |
|              `type`               | `getType`과 `newType`의 간결한 버전                                                                 | `List<Complaint> litany = Collections.list(legachLitancy);` |
