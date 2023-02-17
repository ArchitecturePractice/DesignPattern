# Facade

- 도구의 단순화
- (c.f. 클린코드 향하는 방법)
- (e.g. 인터페이스, 핸들러, ... etc)

## 단어의 뜻

- 건물의 정면 ~- 입구 ?!

## 설계 구조

![설계구조](https://www.plantuml.com/plantuml/png/ZP91QWCn34NtEeMM2KCMzGaoDdJPJGzW75jjXyRZC5OMfUJkBMC4CIZJUlc_zFv7-5WvB7pDPm0k5-I2fy9SCgPfeSXJcG7YETIApo66HFWDWBzlc6QcvIr4sktX1d09yJEy4xvUJ_MhCcK_okZzkgZj3PdueoG_xlQhTjD9LYLPZMI5r2nXlL1bIluEsMtttCxgu49l2HkDdThOQOtTFnixvvyUTkxDBc3rbDv_ffcjDBEHPlGhjMKS3WVXZ8fc5Ss1cDsa1V55bbCyCWp0aUQq_lOV)

## 단순화

- 단순화는 일정 목표를 갖는 처리과정을 수행할 때, 실제 로직에 대한 접근을 제한하거나 대신 수행하는 것을 말한다.
- 사용자가 이에 대해 종단의 I/O 에 대한 제한사항의 이해로 원하는 결과를 갖도록 한다.

### Syntactic Sugar

- 문법적 설탕은 문법의 시각적, 구조적 단순화를 위해 생긴 수단이다.
- 문법적으로 기재해야할 항목을 암묵적(`Implcit`)으로 단순화시킨다.
- 이에 대한 잘못된 사용방식은 `RuntimeException` 으로 반환한다.

### Access Modifier

- Java 에서는 `private`, `default`, `protected`, `public` 등의 접근 제어자를 통해
- 사용자에게 내부로직에 대한 접근권한을 제어하여 실제로는 단순한 인터페이스를
- 사용하도록 유도하고 있다.

### Interface

- `public` 에 대해 `Class` 수준에서 `Inheritance` 에 기능(로직)을 강제하는 수단이다.
- 즉, `Class` 수준의 종단 I/O 에 대한 정의로 볼 수 있다.

### 코드 단위에 따른 접근 단순화

#### 데이터의 단순화

- `public`, `private`, `static`, `final` 을 통해 데이터의 접근/수정/초기화에 대한 권한을 제한하여 단순화한다.
- 대표적인 예로 상수나 `getter/setter` 또는 `constructor` 에서의 초기화 등을 생각할 수 있다.

#### 메소드 단위의 단순화

- `public`, `private` 를 사용하여 내부로직을 제한한다. 
- Template Method 등을 활용하여 일련 처리과정을 단순화하여 제공할 수 있다.

#### 클래스 단위의 단순화

- `protected`, `interface`, `default` `interface`, `Nested Class` 등을 활용한다.

#### 패키지 단위의 단순화

- `default` 제어자를 활용한다.
- 동일 패키지로 로직을 제한한다.

#### 라이브러리 단위의 단순화

- `Facade` (조정이 필요한 로직)
- `delegator` (단순 매칭)

## 시스템의 상대성

### 서브시스템

- 접근하는 시스템의 내부 모듈 또는 논리적 분할단위

### 저수준/고수준의 상대성

- 서브라는 개념에서 각 시스템을 저수준 또는 고수준이라 부를 수 있다.
- 하지만 이는 상대적인 개념으로 저수준 시스템은 실 내부는 단순하다고 말할 수 없다.
- 다시 말해, 실제로 복잡한 구조의 시스템이라도 사용자에게는 이를 은폐하여 제공하기에 사용자의 관점에서는 서브시스템의 계층상으로 저수준이라고 불릴 수 있다.

## 논리적 로직의 Aggregator

- Facade 는 결국 폐쇄/제한/분할된 로직의 조합
- 주로 외부 라이브러리 또는 프레임워크의 로직을 `Application` 상에서 필요에 따라 조합하는 것을 말한다.
- 이는 DDD 관점과 유사한데, 실제로 Spring Framework 가 Java 엔터프라이즈의 대세인 환경에서 `Facade` 의 클래스명을 `SomethingFacade` 라 명시하기보단 `Service` 라 칭한다.
- 이는 시스템 수준의 상대성과 관련되며, 실상 모든 로직의 접근은 `Facade` 라 칭할 수 있다.
- `Facade` 라 불리던 로직 또한 다른 단위의 Aggregator 가 필요한 시점에서 `Subsystem` 이 될 수 있기 때문이다.

## Facade 에 따른 혼동

### `ModelMapper` vs `ObjectMapper`

- `ModelMapper` 는 모델(`Class`)의 변경을 위한 도구이다.
- `ObjectMapper` 는 직렬화를 위한 도구이다.
- `ModelMapper` 는 Converter 로 `Subsystem`(처리 가능한 모델의 로직)들을 추가한다.
- `ObjectMapper` 는 `Serializer`, `Deserializer` 유형의 `Subsystem`(처리 가능한 직렬화 로직) 들을 추가한다.
- 하지만 `ObjectMapper` 는 등록된 `Serializer`, `Deserializer` 의 조합으로 `ModelMapper` 의 Converter 와 같은 로직을 달성할 수 있는 상황이 만들어진다.
- 내부 로직은 다르고 그에 따른 확장기능에 차이는 있지만 결국 `Class` 의 변경은 두 객체 모두가 사용할 수 있다는 것이다.
- 이와 같이 서로 다른 구성이지만 논리적으로 같은 종단의 I/O 를 달성할 수 있는 상황이 발생할 수 있다.
- 때문에 Facade 의 사용자는 자신의 `Application` 방향성과  초기에 사용할 `System` 의 방향성이 평행하는지 확인할 필요가 있다.
- 예를 들어 `ObjectMapper` 는 확장성(외부접근)이 높아서 `@JsonProperty` 이나 `@JsonIgnore` 등의 외부 Strategy 에 따라 결과가 바뀔 수 있다.
- 반면 `ModelMapper` 는 이에 대해 사용자가 `validate` 메소드를 수행하므로 `NotNull` 처리를 강제적으로 수행할 수 있기 때문이다.