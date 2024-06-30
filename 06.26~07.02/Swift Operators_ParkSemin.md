# [Swift] 연산자(대입, 산술, 복합 대입, 비교, 삼항, Nil 결합, 범위, 논리)
***  
<br>  

**\[머릿말\]**

  새로운 언어를 공부할 때마다 느끼는 것이지만 프로그래밍 언어는 대부분 비슷한 것 같다. 구조, 연산자 등 공부를 하면 할수록 비슷한 부분을 많이 느낀다. 이러한 이유 때문인지 이제 새로운 언어를 공부해야 할 일이 생겨도 대학교 1학년 때처럼 두렵지는 않은 것 같다..ㅎ

---

## **1\. 산술 연산자** (Arithmetic Operators)

> Swift는 **type-safe 언어**이기 때문에 **같은 자료형에 대해서만 연산이 가능**하다. 즉, Int형과 Double형의 변수는 서로 연산할 수 없다는 것이다. 그러나 변수로 저장되지 않은 수 자체에 대해서는 연산이 가능하다.  
>   
> <가능한 경우>  
> print(3 + 0.5)  
>   
> <불가능한 경우>  
> let v1: Int = 3  
> let v2: Double = 0.5  
> print(v1 + v2) // binary operator '+' cannot be applied to operands of type 'Int' and 'Double'

-   덧셈 : +
-   뺄셈 : -
-   곱셈 : \*
-   나눗셈 : /
-   나머지 : %

```swift
let v1 = 3
let v2 = 2

print("덧셈 : ", v1 + v2)
print("뺄셈 : ", v1 - v2)
print("곱셈 : ", v1 * v2)
print("나눗셈 : ", v1 / v2)
print("나머지 : ", v1 % v2)

/* [결과]
덧셈 :  5
뺄셈 :  1
곱셈 :  6
나눗셈 :  1
나머지 :  1
*/
```

위 연산자들은 이미 다른 프로그래밍 언어에서 원없이 보았던 것들이다. 위 예제에서 두 변수는 정수형(Int)이므로 나눗셈을 하면 1.5가 나오는 Python과는 달리 1의 결과가 나오게 된다. 나머지 연산자도 Python과 다른 부분이 존재한다. A를 B로 나눈 나머지와 -B로 나눈 나머지는 동일하다는 것이다.

```swift
let v1 = 3
let v2_plus = 2
let v2_minus = -2

print("3 % 2 == ", v1 % v2_plus)
print("3 % -2 == ", v1 % v2_minus)

/* [결과]
3 % 2 ==  1
3 % -2 ==  1
*/
```

위 결과는 다음 식을 통해 이해 가능하다.

> 3 = (2 \* 1) + 1  
> 3 = (-2 \* -1) + 1

그러나 파이썬에서는 3을 -2로 나누면 정수 몫을 기준으로 -2가 나오기 때문에 나머지는 -1이 된다(3 = (-2 \* -2) -1).

## **2\. 대입 연산자** (Assignment Operator)

대입 연산자는 너무나도 잘 알고 있듯 우항의 값을 좌항으로 대입하는 연산자이다.

```swift
let a = 10
let b: Int

b = a

print("a : \(a), b : \(b)")

// a : 10, b : 10
```

## **3\. 복합 대입 연산자** (Compound Assignment Operators)

  Swift에도 복합 대입 연산자가 존재한다. 산술 연산자에 등호를 붙인 형태로 연산을 수행함과 동시에 결과를 저장할 수 있는 연산자이다.

-   a += b : a+b를 수행하고 이를 a에 저장
-   a -= b : a-b를 수행하고 이를 a에 저장
-   a \*= b : a\*b를 수행하고 이를 a에 저장
-   a /= b : a/b를 수행하고 이를 a에 저장
-   a %= b : a%b를 수행하고 이를 a에 저장

```swift
var a = 10
var b = 2

a += b
print("a += b")
print("a : \(a), b : \(b) \n")

a -= b
print("a -= b")
print("a : \(a), b : \(b) \n")

a *= b
print("a *= b")
print("a : \(a), b : \(b) \n")

a /= b
print("a /= b")
print("a : \(a), b : \(b) \n")

a %= b
print("a %= b")
print("a : \(a), b : \(b) \n")

/* [결과]
a += b
a : 12, b : 2 

a -= b
a : 10, b : 2 

a *= b
a : 20, b : 2 

a /= b
a : 10, b : 2 

a %= b
a : 0, b : 2 
*/
```

## **4\. 비교 연산자** (Comparison Operators)

-   a == b : a와 b의 값이 같으면 true, 다르면 false
-   a != b : a와 b의 값이 같으면 false, 다르면 true
-   a > b : a가 b보다 크면 true, 작거나 같으면 false
-   a < b : a가 b보다 작으면 true, 크거나 같으면 false
-   a >= b : a가 b보다 크거나 같으면 true, 작으면 false
-   a <= b : a가 b보다 작거나 같으면 true, 크면 false

비교 연산자는 다른 언어들과 마찬가지로 if 조건문에서도 사용된다. 추가로 Swift에서의 비교 연산자는 하나의 특징을 가지고 있다. 바로 **같은 타입과 같은 갯수의 값을 가지고 있는 2개의 튜플은 비교할 수 있다**는 점이다. 튜플의 값을 앞에서부터 비교하여 앞의 요소를 비교한 결과가 조건문의 결과로 사용할 수 있다면 뒤의 요소들은 비교되지 않는다. 이러한 경우를 **연산 생략(short-circuit evaluation)**이라고 한다.

```swift
print( (1, "A") < (2, "B")) // 1 < 2이므로 "A"와 "B"는 비교되지 않는다.
print( (1, "A") < (1, "B")) // 1 == 1이므로 "A"와 "B"가 비교된다.
print( (1, "A") < (1, "A")) // 1 == 1, "A" == "A"이므로 비교 결과는 false가 된다.

/* [결과]
true
true
false
*/
```

## **5\. 삼항 조건 연산자** (Ternary Conditional Operator)

-   (조건식) ? (true일 때 실행될 부분) : (false일 때 실행될 부분)

  삼항 조건 연산자는 조건식의 결과에 따라 수행되는 if 조건문을 더 간단한 형태로 사용할 수 있는 연산자다. 너무 많이 사용하면 코드의 복잡도가 증가하여 좋지 않은 코드가 되므로 주의해야 한다. if 조건문을 한 줄로 나타낼 수 있다는 점에서 잘 사용하면 코드가 깔끔해진다. ~_이 편리한 연산자가 왜 Python에는 존재하지 않는 것인가..._~

```swift
var A: Int
var B: Int

A = 3; B = 2
var result = A > B ? "A는 B보다 큽니다" : "A는 B보다 작거나 같습니다"
print(result)

A = 3; B = 3
result = A > B ? "A는 B보다 큽니다" : "A는 B보다 작거나 같습니다"
print(result)

/* [결과]
A는 B보다 큽니다
A는 B보다 작거나 같습니다
*/

/* [이를 if 조건문으로 나타낸다면]
if A > B {
    result = "A는 B보다 큽니다"
} else {
    result = "A는 B보다 작거나 같습니다"
}
*/
```

## **6\. Nil-결합 연산자** (Nil-Coalescing Operator)

-   a ?? b : a가 nil이 아니라면 a에 저장된 값을 사용하고, 그렇지 않다면 b를 사용한다.  
    이를 삼항 조건 연산자로 나타내면 다음과 같다.  
    \=>   (a == nil) ? b : a

  Swift에서는 값이 없음을 뜻하는 null을 **nil**이라고 사용한다. 그리고 nil 값을 가질 수 있는 변수를 **옵셔널 변수(Optional Variable)**라고 한다. 옵셔널 변수를 사용할 때는 해당 변수가 **nil일 경우와 그렇지 않을 경우에 대한 처리를 모두** 해야 한다. 그렇지 않으면 **컴파일 오류**가 발생한다.

```swift
var nilVariable: Int?

print(nilVariable ?? "값이 존재하지 않습니다")

nilVariable = 10
print(nilVariable ?? "값이 존재하지 않습니다")

/* [결과]
값이 존재하지 않습니다
10
*/
```

## **7\. 범위 연산자** (Range Operators)

### 7-1. 닫힌 범위 연산자 (Closed Range Operator)

-   a...b : a 이상 b 이하의 값을 모두 포함하는 범위다. 이때 **a가 b보다 크면 컴파일 오류가 발생**한다.

```swift
for i in 1...5 {
    print(i)
}

/* [결과]
1
2
3
4
5
*/
```

### 7-2. 반-열림 범위 연산자 (Half-Open Range Operator)

-   a ..< b : a 이상 b 미만의 값을 모두 포함하는 범위다. 마찬가지로 a는 b보다 클 수 없다.

```swift
for i in 1..<5 {
    print(i)
}

/* [결과]
1
2
3
4
*/
```

### 7-3. 단-방향 범위(One-Sided Ranges)

-   a... : a 이상의 모든 값

```swift
for i in 10... {
    print(i)
}

/* [결과]
10
11
12
13
14
15
16
...
*/
```

-   ...a : a 이하의 모든 값. contains() 메서드를 사용하면 특정 값이 범위에 포함되는지를 확인할 수 있다.

```swift
let ran = ...5

print(ran.contains(-100))
print(ran.contains(1))
print(ran.contains(5))
print(ran.contains(6))

/* [결과]
true
true
true
false
*/
```

-   ..<a : a 미만의 모든 값. a가 포함되지 않는다는 점을 제외하고는 ...a 와 동일하다.

## **8\. 논리 연산자** (Logical Operators)

### 8-1. AND 연산자 :: &&

  AND 연산자는 좌항의 조건과 우항의 조건이 **모두 true여야만 true**를 반환한다. 만약 좌항의 조건이 false라면 우항의 조건을 보지 않아도 이미 결과는 false이므로 우항의 조건은 확인되지 않는다. 이를 역시 연산 생략이라고 한다.

```swift
print(true && true) // true
print(true && false) // false
print(false && false) // false

print(10 > 20 && "A" < "B") // 이때 10>20은 false이므로 "A"와 "B"는 비교되지 않는다.

/* [결과]
true
false
false
false
*/
```

### 8-2. OR 연산자 :: ||

  OR 연산자는 좌항과 우항 중 **하나만 true면 true**를 반환한다. 만약 좌항의 조건이 true라면 우항의 조건을 보지 않아도 이미 결과는 true이므로 연산 생략이 일어난다.

```swift
print(true || true) // true
print(true || false) // true
print(false || false) // false

print(10 < 20 || "A" < "B") // 이때 10<20은 true이므로 "A"와 "B"는 비교되지 않는다.

/* [결과]
true
true
false
true
*/
```

### 8-3. NOT 연산자 :: !

NOT 연산자는 **Bool 값을 반대로 반전**시킨다. 즉, true라면 false로 반전시키고 false라면 true로 반전시킨다.

```swift
print(!true)    // false
print(!false)   // true

/* [결과]
false
true
*/
```

### 8-4. 논리 연산자 우선순위

  논리 연산자의 우선 순위는 **NOT 연산자, AND 연산자, OR 연산자** 순이다. 그러나 다른 프로그래밍 언어들과 동일하게 연산자의 우선순위에 의존하여 많은 논리 연산자를 사용한다면 코드의 가독성이 떨어지므로 만약 여러 논리 연산자를 사용해야 한다면 **소괄호()**를 사용하는 것이 좋다.

```swift
print(true && false || true && true) // true
// true && false가 거짓이므로 true && true가 실행되고 이는 참이므로 결과는 true

print(true && false || true && !true) // false
// true && false가 거짓이므로 true && !true가 실행되는데 이때 !true는 false이므로 결과는 false


/* 위 문장을 괄호를 사용하여 표현하면 다음과 같다.
print( (true && false) || (true && !true) )
print( (true && false) || (true && (!true) ) )
*/
```

---

어느 프로그래밍 언어더라도 공통적으로 사용되는 연산자이므로 잘 공부하도록 하고 그중 Swift의 특징은 특히 신경써서 기억해야겠다.

끝!