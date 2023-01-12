# Structural Design Patterns - Bridge Pattern

### 시작하기 전에

`USB Charger` VS `Adapter Charger`

무엇이 달라 보이시나요?

**USB Charger**

![](study/DesignPattern/asset/bridge/bridge-usb-charger.png)

**Adapter Charger**

![](study/DesignPattern/asset/bridge/bridge-adapter-charger.png)

-- --

## 브릿지 패턴의 대표적 특징

* 구조 디자인 패턴 
* IS - A -> HAS - A 관계로 변화
* 관심사를 분리
* 기능과 구현의 대상을 분리해서 독립적인 개발 및 도입이 가능

-- --

### 시나리오

![](study/DesignPattern/asset/bridge/bridge-scenario.png)

그림 상으로는 `원`, `사각형` 의 `Shape` 가 존재한다

하지만 요구사항이 점점 늘어나

`삼각형`, `오각형`, `육각형`, `팔각형` 의 `Shape` 가 추가 된다면

`RedTriangle`, `BlueTriangle` , `RedPentagon`, `BluePentagon`, `RedHexagon` , `BlueHexagon`, `RedOctagon`, `BlueOctagon`

위 구현채들이 생성되어야 한다

한 마디로, `도형` 과 `색` 이 강하게 결합되어 있어

`도형`의 신규 요구사항이 자연스럽게 `색`의 요구사항으로 번진 것이다

그렇다면 기획자가 새로운 `Pink` 라는 `색`을 추가하고자 한다면, 개발자는 단순히 `Pink` 라는 `색`만 추가하면 될까??

정답은 아니다

개발자는 `Pink 색` 추가를 위해

`PinkCircle` ,`PinkTriangle`, `PinkSquare`, `PinkPentagon`, `PinkHexagon`, ,`PinkOctagon` 의 구현체를 추가해야 한다

만약 이런 `도형`이 100 개, `색` 이 100 가지 라면 (이미 구현체는 **10000개**에 달할 것)

기획자가 `신규 색 추가해주세요` 라는 요구사항을 건넸을 때

개발자가 과연 기분 좋게 그 작업을 수행할 수 있을까?

-- --

### 브릿지 패턴

브릿지 패턴을 사용해서 위 사항을 구현했다면

기획자에게 자신 있게 `Yes, of course. why not?` 이라고 대답할 수 있다

`단 하나의 구현체`만 `추가`하면 되기 때문이다

브릿지 턴에서는 이러한 개념을 `관심사의 분리` 라고 표현한다

위 예시에서는 `도형`과 `색` 이라는 동등한 개념의 분리를 에시로 들었지만

`핵심 기능` + `기능 수행 주체` 와 같은 케이스 처럼 다양하게 적용이 가능하다

핵심 내용은 `관심사를 분리하여, 독립적인 개발이 가능하게 하는 것` 이다

-- --

### 장점

    * 코드 재사용성 향상
    * 중복 코드 제거
    * 유지보수성 향상
    * 기능의 관심사 분리로 인한 캡슐화 및 응집도 향상

-- --

### 브릿지 패턴이 적용 된 예시

![](study/DesignPattern/asset/bridge/bridge-uml.png)
[예시 코드](https://github.com/meeingjae/software-design/tree/master/src/main/java/com/software/design/bridge/store)

잠깐 !

Spring에서도 사용할 수 있어요 ! ([예시 코드](https://github.com/meeingjae/software-design/tree/master/src/main/java/com/software/design/bridge/storebean))
-- --

### Device 예시

![](study/DesignPattern/asset/bridge/bridge-guru-example.png)
[예시 코드](https://github.com/meeingjae/software-design/tree/master/src/main/java/com/software/design/bridge/device)

-- --

* Reference
    * https://stacktraceguru.com/bridge-design-pattern/
    * https://refactoring.guru/design-patterns/bridge
