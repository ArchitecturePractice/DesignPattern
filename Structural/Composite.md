# 🌲 Composite

* Object Tree 구조
* 객체와 복합체에 대해 같은 기능을 수행

## ❔ 도형 보이기

![Shapes by Refactoring Guru](https://refactoring.guru/images/patterns/examples/java/composite/OutputDemo.png)

* 도형을 그리는 메소드를 차례로 모두 호출?

```java
public void paint() {
    paintBlueShapes();
    paintRedShapes();
    paintGreenShapes();
}
```

## ❕ Solution

### 1. Tree 구조로 만들기

![Hierarchial Tree Structure by Spring Framework Guru](https://springframework.guru/wp-content/uploads/2015/07/HierarchialTreeStructure.png)

### 2. Leaf도 Composite (Node)도 모두 Component

![Composite Structure by Refactoring Guru](https://refactoring.guru/images/patterns/diagrams/composite/structure-en.png?id=b7f114558b594dfb220d225398b2b744)

* Client는 Component의 메소드만 반복해주면 되는 형태

## 💻 예제

* [BaseShape](https://refactoring.guru/design-patterns/composite/java/example#example-0--shapes-BaseShape-java)
* [Circle](https://refactoring.guru/design-patterns/composite/java/example#example-0--shapes-Circle-java)
* [CompoundShape](https://refactoring.guru/design-patterns/composite/java/example#example-0--shapes-CompoundShape-java)
* [ImageEditor](https://refactoring.guru/design-patterns/composite/java/example#example-0--editor-ImageEditor-java)

## GUI

* [Hierarchy For Package javax.swing](https://docs.oracle.com/javase/8/docs/api/javax/swing/package-tree.html)

![Hierarchy of Java Swing classes by javatpoint](https://static.javatpoint.com/images/swinghierarchy.jpg)

## 🌗 장단점

### 1. 장점

* 복합체가 섞인 복잡한 구조에 대해 간단하게 같은 기능을 수행하도록 할 수 있음
* 새로운 유형의 요소를 추가하거나 기존 요소를 변경할 경우, 클라이언트 코드의 수정 최소화 (*[Open/Closed Principle](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)*)

### 2. 단점

* 구조는 비슷하지만 요소별로 서로 다른 기능을 수행해야 하는 경우에는 적합하지 않음 👉 ***Overgeneralize Interfaces***

## 📚 Reference

1. [Composite - Refactoring Guru](https://refactoring.guru/design-patterns/composite)
2. [Spring Framework Guru](https://springframework.guru/)
3. [Java™ Platform, Standard Edition 8 API Specification](https://docs.oracle.com/javase/8/docs/api/)
4. [Java Swing Tutorial - javatpoint](https://www.javatpoint.com/java-swing)