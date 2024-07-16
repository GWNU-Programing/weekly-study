## **1\. 반복문**

Swift에서 반복문은 for ~ in, while, repeat ~ while이 있다.

### 1-1. for ~ in

배열, 집합, 딕셔너리과 같은 콜렉션의 모든 요소에 접근하거나 문자열의 각 문자에 대한 접근, 범위 내 모든 값의 접근이 필요할 때 사용하는 반복문이다.

```swift
// 1. 배열
for i in [1, 2, 3, 4] {
    print(i, terminator: " ") // 1 2 3 4
}
print()

// 2. 집합
let mySet: Set = ["A", "B", "C"]
for i in mySet {
    print(i, terminator: " ") // A B C
}
print()

// 3. 딕셔너리
let myDict = ["name": "Semin", "location": "Won-Ju"]
for (key, value) in myDict {
    print(key, value)
}
/*
 name Semin
 location Won-Ju
 */

// 4. 문자열
for c in "Hello!" {
    print(c)
}
/*
 H
 e
 l
 l
 o
 !
 */

// 5. 범위
for i in 1...5 {
    print(i, terminator: " ") // 1 2 3 4 5
}
```

#### 1-1-1. 언더바(\_)

만약 특정 횟수만큼 반복하는 것만 필요하고 반복 변수는 필요하지 않다면 **언더바(\_)**를 사용하면 **값을 무시**할 수 있다.

```swift
for _ in 1...5 {
    print("실행됨!")
}

/*
 실행됨!
 실행됨!
 실행됨!
 실행됨!
 실행됨!
 */
```

#### 1-1-2. stride(from: to: by:), stride(from: through: by:)

stride는 **from**부터 **to**까지 **by** 만큼의 간격으로 실행하기 위해 사용한다. 이때 **to를 사용하면 마지막 범위를 포함****하지 않고**, **through를 사용하면 마지막 범위를 포함**한다.

```swift
for i in stride(from: 0, to: 6, by: 2) {
    print(i, terminator: " ") // 0 2 4
}
print()

for i in stride(from: 0, through: 6, by: 2) {
    print(i, terminator: " ") // 0 2 4 6
}
```

### 1-2. while

while 반복문은 **조건문이 참인 경우에만 실행**하고 거짓이라면 실행하지 않는 반복문이다. 조건문이 거짓이라면 단 한 번도 실행되지 않는다.

```swift
var count = 0

while count <= 3 {
    print(count)
    count += 1
}
/*
 0
 1
 2
 3
 */

// 조건이 거짓이라면 한 번도 실행되지 않는다
while count < 0 {
    print("실행됨!") // 절대 출력되지 않는 부분
}
```

### 1-3. repeat ~ while

이 반복문은 다른 프로그래밍 언어의 **do ~ while** 문과 같은 역할을 한다. **최초 1번은 조건문에 관계없이 실행**되고 그 이후부터는 조건문에 따라 실행 여부가 정해진다. 아래 코드를 보면 count는 0이므로 count < 0은 false이지만 최초 1번은 실행되는 것을 확인할 수 있다.

```swift
var count = 0

repeat {
    print("일단 실행됨!") // 일단 실행됨!
} while count < 0
```

## **2\. 조건문**

### 2-1. if, if ~ else, if ~ else if ~ else

if 조건문은 거의 모든 프로그래밍 언어에서 볼 수 있는 조건문이다. 조건이 **참이면 if 부분**이, **거짓이라면 else 부분**이 실행되고 if 부분이 거짓일 때 else if 부분이 있다면 다음 조건으로 이를 확인한다. 조건문에는 **Bool** 타입의 값만 올 수 있고 **숫자, 문자 등을 사용하면 오류가 발생**한다.

```swift
if 3 < 5 {
    print("3은 5보다 작습니다.") // 실행되는 부분
} else {
    print("3은 5보다 큽니다.")
}


if "AA" < "BB" {
    print("문자는 ASCII 코드르 기준으로 비교됩니다.") // 실행되는 부분
} else {
    print("AA >= BB")
}


if 10 < 10 {
    print("10 < 10")
} else if 10 > 10 {
    print("10 > 10")
} else {
    print("10 == 10") // 실행되는 부분
}


//if 1 {
//    // 조건문에는 Bool 타입만 사용할 수 있습니다.
//}
```

**Optional** 타입의 변수에 **값이 존재하면 true**, **그렇지 않다면 false**가 반환되는 것도 조건문에 사용할 수 있다.

```swift
var optionalIntValue: Int?

if let v = optionalIntValue {
    print("값 O, \(optionalIntValue)")
} else {
    print("값 X, \(optionalIntValue)") // 이 부분 실행
}


optionalIntValue = 10
if let v = optionalIntValue {
    print("값 O, \(optionalIntValue)") // 이 부분 실행
} else {
    print("값 X, \(optionalIntValue)")
}
```

#### 2-1-1. defer

defer 안에 작성한 문장은 **현재 범위의 마지막 문장이 실행된 다음에 실행**된다. 따라서 defer은 **범위의 최상단에 작성**해야 한다. defer을 2개 사용하면 **가장 아랫 부분에 사용한 것부터 가장 윗 부분에 사용한 순으로 실행**된다.

```swift
var age: Int = 25

if 20 <= age && age < 30 {
    defer {
        print("반복문을 종료합니다.")
    }
    
    defer {
        age -= age
        print("변경 후 나이는 \(age)입니다.") // 0
    }
    
    print("최초 나이는 \(age)입니다.")
}

/*
 최초 나이는 25입니다.
 변경 후 나이는 0입니다.
 반복문을 종료합니다.
 */
```

### 2-2. Switch

-   switch 변수 ~ case: ~ default

  switch 조건문은 명시한 **변수의 값에 매치되는 case를 실행**하는 조건문이다. **Swift에서 모든 switch 구문은 완벽**해야 하므로 사용한 변수는 **반드시 하나의 경우에는 매치**되어야 한다. **어느 하나의 경우에도 매치되지 않으면 오류가 발생**한다. **어느 case에도 매치되지 않을 때 실행되는 부분을 작성**하려면 **default:** 키워드에 작성하면 된다.

  다른 프로그래밍 언어에서 switch는 각각의 case에서 break를 사용해야 다음 case로 넘어가지 않지만 Swift에서는 **break를 사용하지 않더라도 다음 case로 넘어가지 않고 바로 switch 부분의 실행이 종료**된다.

```swift
var grade = 3.7

switch grade {
case 4.5:
    print("A+")
case 4.0..<4.5:
    print("A")
case 3.5..<4.0:
    print("B+") // B+
case 3.0..<3.5:
    print("B")
case 2.5..<3.0:
    print("C+")
default: // 이 부분을 생략하면 "Switch must be exhaustive" 오류 발생
    print("재수강")
}
```

#### 2-2-1. fallthrough

  앞서 Swift에서의 Switch 문은 하나의 case가 실행되고 나면 다음 case로 넘어가지 않는다고 하였다. 이때 **fallthrough**를 사용하면 **다음 case로 실행을 이어나갈 수 있다**.

```swift
let v = "a"

switch v {
case "a":
    print("소문자 a")
    fallthrough // fallthrough에 의해 case "A": 부분도 실행됨
case "A":
    print("대문자 A")
default:
    print("A 또는 a 아님")
}

// 소문자 a
// 대문자 A
```

#### 2-2-2. 하나의 case에서 여러 값을 확인

Swift에서는 case: 키워드에 **하나 이상의 값을 사용**할 수 있다. 이렇게 되면 하나의 case에 **매치되도록 할 값을 여러 개 설정**할 수 있다.

```swift
let v = "A"

switch v {
case "a", "A":
    print("A 또는 a")
default:
    print("A 또는 a 아님")
}
```

#### 2-2-3. 튜플

2개의 값을 갖는 **튜플을 case 키워드에 사용하여 한 번에 2개의 값을 비교**할 수 있다.

```swift
let hourMinute = (6, 15)

switch hourMinute {
case (6, 0):
    print("6시 00분")
case (6, 5):
    print("6시 05분")
case (6, 10):
    print("6시 10분")
case (6, 15):
    print("6시 15분") // 6시 15분
default:
    print("NONE")
}
```

#### 2-2-4. 바인딩 (Binding)

바인딩은 **특정 값을 변수 또는 상수에 매칭시키는 것**으로, 이것을 사용하면 **(1, 0), (2, 0), (3, 0)**처럼 **(x, 0)**과 같은 타입의 값이면 다 매칭되도록 설정할 수 있다.

```swift
let hourMinute = (6, 15)

switch hourMinute {
case (6, let minute): // let minute = 15
    print("6시 \(minute)분") // 6시 15분
default:
    print("NONE")
}
```

#### 2-2-5. 조건 (Where)

**where**을 사용하면 **바인딩을 사용했을 때의 추가 확인 조건**을 설정할 수 있다.

```swift
let hourMinute = (6, 30)

switch hourMinute {
case (6, let minute) where minute == 0:
    print("정각 6시")
case (6, let minute) where minute == 30:
    print("6시 30분")
default:
    print("NONE")
}
```

### 2-3. guard

guard 문을 사용하면 **조건식이 true일 경우에는 실행을 이어가고**, **false일 경우에는 else 부분이 실행**되도록 설정할 수 있다. 그렇다보니 guard 구문을 사용할 때 **else 부분은 필수로 사용**해야 한다. guard 문은 **함수, 반복문 등의 범위 내**에서 실행되며 else 부분에는 반드시 **return**,  **throw**, **continue**, **break** 등의 실행을 제어하는 키워드를 사용하여 예외 처리를 해주어야 한다. **이를 생략하면 오류가 발생**한다.

```swift
for i in 1...5 {
    guard i < 5 else {
        print("i는 5보다 큽니다")
        break
    }
    print(i)
}

/*
 1
 2
 3
 4
 i는 5보다 큽니다
 */
```

```swift
func printString(text: Any) {
    guard text is String else {
        print("String 타입의 값만 출력할 수 있습니다")
        return
    }
    
    print(text)
}

printString(text: "Hello") // Hello
printString(text: 123)     // String 타입의 값만 출력할 수 있습니다
```

## **3\. 제어 변경 구문**

### 3-1. continue

continue는 반복문에서 **현재 실행을 중지하고 다음 반복 값으로 넘어가기** 위해 사용한다.

```swift
for i in 1...5 {
    if i == 3 { continue } // i == 3이라면 다음 값인 4로 넘어감
    
    print(i)
}

/*
 1
 2
 4
 5
 */
```

### 3-2. break

break는 **현재 실행을 중지하고 반복문을 종료**하기 위해 사용한다.

```swift
for i in 1...5 {
    if i == 3 { break } // i == 3이라면 반복문이 종료됨
    
    print(i)
}

/*
 1
 2
 */
```

---
<br>  
<center>
모든 프로그램에 필수로 사용되는 반복문, 조건문은 정말정말 중요하다...

fallthrough, defer, 바인딩 등 Swift에서 새롭게 보는 개념이 많으니 잘 익혀두도록 하자!

끝!
</center>