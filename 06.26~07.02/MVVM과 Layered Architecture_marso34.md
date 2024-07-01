# MVVM과 Layered Architecture
프론트엔드 개발을 위한 공부

<br>

### MVVM (Model-View-ViewModel)이란?
---
애플리케이션(응요 프로그램)을 데이터를 처리하는 모델(Model), 사용자에게 보여지는 뷰(View), 모델과 뷰 사이를 이어주는 뷰모델(ViewModel)로 분리하는 소프트웨어 아키텍처 패턴

<br>

MVC 패턴 -> MVP 패턴 -> **MVVM 패턴**으로 등장
- *mvp는 mvvm과 유사한 패턴으로 Model-View-Presenter로 구성. 데이터 바인딩, 상태 관리 등으로 mvp보다는 mvvm을 더 보편적으로 사용*

<br>

#### MVVM 구조
- Model: 데이터, 비즈니스 로직, 비즈니스 규칙 등
- View: 사용자 인터페이스, 사용자와의 상호작용
- ViewModel: Model 데이터를 View에 제공, VIew의 이벤트를 받아서 Model을 갱신. Data Binding을 사용하여 View와 ViewModel을 직접 연결

<br>

**장점**
- View와 Modle간 독립성 유지
	- 뷰와 비즈니스 로직의 명확한 분리 가능
- 유닛 테스트가 용이
- Data Binding으로 간결한 코드 작성
<br>

**단점**
- 디버깅이 어려울 수 있음
- ViewModel의 비대화 가능
- 표준화된 틀이 없음

<br>

### Layered Architecture (계층화 아키텍처)란?
---
계층 구조를 사용하여 애플리케이션(응용 프로그램)을 구조화하는 소프트웨어 아키텍처 패턴

<br>

#### 구조
- **Presentation Layer (UI Layer)**: 사용자 인터페이스와 사용자 상호작용을 처리
- **Application Layer (Service Layer)**: 비즈니스 로직을 담당. 사용자 요청을 처리하고, 데이터를 변환하며, 응용 프로그램의 흐름을 관리 (상태 관리)
- **Business Layer (Domain Layer)**: 도메인 로직과 비즈니스 규칙을 처리. 복잡한 비즈니스 규칙을 캡슐화하여 다른 계층에서 사용할 수 있도록 구성
- **Data Access Layer (Persistence Layer)**: ~~데이터베이스와의 상호작용~~ 서버와의 데이터 통신, API 요청 및 응답 처리, 로컬 저장소 상호작용 등


**장점**
- 코드 재사용
- 코드 중복 방지
- 책임 분리
- 유지보수 용이

<br>

**단점**
- 복잡성 증가
- 성능 오버헤드

<br>

### 프론트엔드 아키텍처 구조
---
MVVM과 Layered Architecture를 결합한 아키텍처

<br>

![image](https://github.com/GWNU-Programing/weekly-study/assets/96871583/bae8338d-dc4d-4894-a34f-0050818cbd46)

<br>

- 각 계층은 개발하면서 디테일 추가

<br>
<br>
