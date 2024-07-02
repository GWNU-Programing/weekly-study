
# Python List VS Array

파이썬을 사용할 때 배열을 리스트라고 불러서 배열과 리스트가 같은 의미로 사용되는것으로 알고 있었다. 

> 하지만 다른 언어에서는 배열로 부르는것을 왜 파이썬에서는 리스트라고 불리는거야?

분명 의미는 같을지언정 내부적으로 뭔가 다를것이라고 생각이 들었다. 
간단한 코드를 통해 검증해 보면서 배열과 리스트의 차이점에 대해 알 수 있었다. 
_(리스트는 내장된 데이터 구조로 바로 사용가능하지만 배열은 array 모듈이나 numpy 모듈을 사용해야한다고 한다.) _

## 1. 데이터 타입 
``` python
import array

t_list = [1, 2, 'test', [1,2,3], {"key":1}]
t_arr = array.array("i",[1,2,3])
```
>파이썬의 리스트에는 여러가지 데이터 타입을 담아서 사용 할 수 있다. 
하지만 배열은 단일 데이터 타입만을 허용한다. 

단일 타입만을 허용하는데 t_arr에서 **" i "** 는 뭐지??
-> "i"는  signed integer (정수형 4바이트 데이터)를 의미하는 TypeCode코드이다.
array 모듈에서 배열을 사용할때 2개의 매개변수를 받는데 첫 위치가 TypeCode이다. 
TypeCode는 저장할 데이터 타입을 정하는건데 아래와 같이 array 모듈에 작성되어있다. 

![](https://velog.velcdn.com/images/hwstar-1204/post/67e486c8-e567-40bf-821a-bfb8a28529e7/image.png) 

공식문서를 찾아본 결과 이렇게 정의되어 있는것을 볼 수 있었다. 
https://docs.python.org/ko/3/library/array.html 
![](https://velog.velcdn.com/images/hwstar-1204/post/3f54fa00-d792-4a79-a6aa-a2af9bb54dfb/image.png) 


## 2. 메모리 사용 
```python
import array, sys

arr1 = []  # 리스트
arr2 = array.array("i",arr1)  # 배열

# 결과 1
print(f"arr1 : {sys.getsizeof(arr1)} bytes")  # arr1: 56 bytes
print(f"arr2 : {sys.getsizeof(arr2)} bytes")  # arr2: 80 bytes

arr1 = [i for i in range(10)]  # 정수형 데이터 10개 추가 
arr2 = array.array("i", arr1)  

# 결과 2
print(f"arr1: {sys.getsizeof(arr1)} bytes")  # arr1: 184 bytes
print(f"arr2: {sys.getsizeof(arr2)} bytes")  # arr2: 120 bytes
```
같은 개수의 정수형 데이터를 각 변수에 리스트와 배열로 저장한 후 메모리 차지 공간을 출력해보았다. 
출력 결과는 _내부 정수형 데이터의 크기를 포함하지 않은 객체 자체_ 의 크기를 출력한 것이다.

>
#### 결과 1
기본 리스트와 배열의 객체 크기 비교 
: 리스트 (56 bytes) < 배열 (80 bytes)
#### 결과 2
같은 정수 데이터를 저장한 리스트와 배열의 객체 크기 비교 
: 리스트 (180 bytes) < 배열 (120 bytes)
>
위 결과의 원인은 리스트와 배열의 **메모리 관리 방식 차이**에 있다. 
- 리스트는 각 요소에 대한 포인터를 저장한다.
- 배열은 동일 타입의 데이터를 연속된 블록에 저장한다. 
>
결과 2의 리스트와 배열 크기 계산
- 리스트 184 bytes = 리스트 객체(56) + 포인터 배열 (8*10) + 미리 할당된 포인터(8*6)
- 배열  120 bytes = 배열 객체(80) + 정수형 데이터 (4*10)

64bit 시스템에서 각 포인터는 8 bytes를 차지한다. 
만약 전체 (내부 정수형 데이터를 포함한 객체)의 크기를 출력해보면 리스트의 크기가 훨씬 크게 출력되는데 포인터 배열에 저장된 데이터는 주소는 정수형 데이터를 포함한 정수형 객체를 가리키고 있기 때문이다.  
자세한 사항은 [여기를](https://velog.io/@l_cloud/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EA%B3%BC-%EB%A9%94%EB%AA%A8%EB%A6%AC#%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%97%90%EC%84%9C-%EA%B0%9D%EC%B2%B4%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%96%B4%EB%8A%90-%EC%98%81%EC%97%AD%EC%97%90-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A0%80%EC%9E%A5%EB%90%98%EB%8A%94%EA%B0%80) 참고하는게 좋을것 같다. 

## 3. 크기 조절
위의 결과2에서 리스트의 크기에 '미리 할당된 포인터'가 포함되어있었다. 
이러한 이유는 리스트가 값이 추가될때 마다 크기를 조금씩 증가 시키지 않고 여유롭게 미리 증가시켜 놓는다. 
이는 리스트의 객체가 크기를 유연하게 변경할 수 있도록 메모리를 할당하고 관리하기 위함이다. 
아래의 코드를 통해 확인해 보자 
```python
import sys

arr1 = []
print(f"리스트 객체 : {sys.getsizeof(arr1)} bytes")

for i in range(20):
    arr1.append(i)
    print(f"{i+1} 개 원소: {sys.getsizeof(arr1)} bytes")
```

**결과**

<img src="https://velog.velcdn.com/images/hwstar-1204/post/825c5f96-273c-40cc-af4e-de3ad2df695f/image.png" width="250" height="400">

## 4. 성능 
리스트를 사용하면 다양한 데이터 타입을 다룰 수 있고 파이썬의 내장 함수와 매소드 지원이 풍부하기 때문에 유연성이나 사용성 측면에서 유리하다. 
하지만 각 요소에 대한 포인터를 저장하므로 메모리 효율성이나 데이터 접근 속도 측면에서 배열보다 성능이 떨어질 수 있다. 

배열은 고정된 크기에서 동일 타입의 데이터를 연속하여 저장하므로 메모리 효율성이나 데이터 접근속도 측면에서 성능이 높다. 

## 리스트 (`list`)와 배열 (`array.array`)의 차이점

| 항목         | 리스트 (`list`)                         | 배열 (`array.array`)                           |
|-------------|---------------------------------------|---------------------------------------------|
| **데이터 타입**  | 다양한 데이터 타입 저장 가능            | 단일 데이터 타입 저장 (예: `i`는 정수형)         |
| **메모리 사용**  | 상대적으로 더 많은 메모리 사용            | 메모리 사용이 더 효율적 (같은 타입의 데이터만 저장) |
| **크기 조절**    | 동적으로 크기 조절 가능                   | 크기 조절은 가능하지만 동일 타입의 데이터만 가능  |
| **성능**         | 다양한 작업을 지원하지만 배열보다 느림    | 특정 데이터 타입 작업에 최적화되어 빠름        |


## 결론
리스트와 배열은 각각의 장단점이 있으며, 용도와 상황에 따라 선택적으로 사용해야 한다
리스트는 유연성과 사용성이 뛰어나지만, 메모리 효율성과 성능 면에서는 배열이 더 유리하다.

따라서, 데이터의 특성과 작업의 성격에 따라 적절한 자료구조를 선택하는 것이 중요하다.
ex) 다양한 데이터 타입을 저장하고 관리하는 작업에는 리스트를 사용하고, 대규모 수치 데이터를 빠르게 처리해야 하는 경우에는 배열을 사용하는 것이 좋다.

#### 참고 자료 
https://learnpython.com/blog/python-array-vs-list/
[[파이썬 자료구조] 파이썬에서 배열과 리스트의 차이](https://siloam72761.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%97%90%EC%84%9C-%EB%B0%B0%EC%97%B4%EA%B3%BC-%EB%A6%AC%EC%8A%A4%ED%8A%B8%EC%9D%98-%EC%B0%A8%EC%9D%B4)
[Python 객체의 메모리 크기 구하는 방법
](https://velog.io/@mhcoma/Python-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%81%AC%EA%B8%B0-%EA%B5%AC%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)


블로그 
https://velog.io/@hwstar-1204/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%98-%EB%B0%B0%EC%97%B4%EA%B3%BC-%EB%A6%AC%EC%8A%A4%ED%8A%B8%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90