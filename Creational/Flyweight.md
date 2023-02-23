# Flyweight Pattern

#### 왜 Flyweight Pattern이 필요했을까요

- Application 에서 만드는 객체의 수가 엄청 많음
- 객체 생성이 메모리에 부담이 되며 오래걸림

#### 구조

![플라이웨이트 디자인 패턴 구조](https://refactoring.guru/images/patterns/diagrams/flyweight/structure.png?id=c1e7e1748f957a4792822f902bc1d420)

Flyweight : 공유되어 지는 Object의 상태값을 갖는 객체

Context : Flyweight 와 쌍을 이루어 하나의 객체를 표현

Flyweight Factory : Flyweight pool을 관리하여 Flyweight를 생성

#### 적용하기 위해서

> 객체의 속성을 Intrinsic 과 extrinsic 으로 분류해야 한다.

- intrinsic :  Flyweight Object 가 갖는 불변적인 본질의 속성
- extrinsic :  클라이언트 코드등에 의해서 갖는 가변적인 외적의 속성

#### Flyweight pattern 은 ...

- 이미 만들어져 있는 동일한 인스턴스가 있다면 해당 인스턴스를 사용 한다.

- 공통된 인스턴스들을 공유하여 메모리의 사용량을 줄인다.
- 인스턴스의 공유를 통해 new 생성을 줄인다.

![](http://drive.google.com/uc?id=1ulqlUMJ5fDXUuunmjEX0ZP6jr4_I_86P)

```java
public class DefenseItemType {

    private String name;
    private int healthPoint;
    private int shieldPoint;

    public DefenseItemType(String name, int healthPoint, int shieldPoint) {

        this.name = name;
        this.healthPoint = healthPoint;
        this.shieldPoint = shieldPoint;
    }

    public void putOn(String skin) {

        System.out.println(skin + "의" + name + "방어구 착용");
    }
}
```

```java
public class Armor1 extends DefenseItemType{

    public Armor1() {

        super("Armor1", 100, 50);
    }
}
```

```java
public class Armor2 extends DefenseItemType{

    public Armor2() {

        super("Armor2", 200, 100);
    }
}
```

```java
public class DefenseItem {

    private String skin;
    private DefenseItemType defenseItemType;

    public DefenseItem(String skin, DefenseItemType defenseItemType) {

        this.skin = skin;
        this.defenseItemType = defenseItemType;
    }

    public void putOn(){
        defenseItemType.putOn(skin);
    }
}
```

```java
import java.util.HashMap;
import java.util.Map;

public class DefenseItemFactory {

    static Map<String, DefenseItemType> itemPool = new HashMap<>();

    public static DefenseItemType getDefenseItemType(String name) {

        DefenseItemType defenseItemType = itemPool.get(name);
        if (defenseItemType == null) {
            if (name.equals("Armor1")) {
                defenseItemType = new Armor1();
                itemPool.put(name, defenseItemType);
            } else if (name.equals("Armor2")) {
                defenseItemType = new Armor2();
                itemPool.put(name, defenseItemType);
            } else {
                System.out.println("존재하지 않는 아이템");
            }

        }
        return defenseItemType;
    }
}
```

#### 사용되는 곳

Integer valueof

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

- -128과 127로 이 범위의 Integer 객체를 미리 생성해두고 재사용

String constant pool

```java
String a = "abcd";
String b = "abcd";

assertThat(a == b).isTrue();
```

