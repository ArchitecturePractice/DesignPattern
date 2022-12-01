# Builder

> ___복잡한 객체들을 단계별로 생성할 수 있도록 하는___ 생성 패턴
- Effective Java 에서 사용하는 Builder Pattern : 생성자 인자가 많을 때
- GoF 디자인 패턴에서의 Builder Pattern : 생성 절차가 복잡할 때

# 1. Effective Java에서 사용하는 Builder Pattern
- 객체를 생성할 때 매개 변수가 여러개인 경우 각각의 매개 변수가 점점 늘어나는 여러 개의 생성자를 사용하기 보다는
  인스턴스 생성을 위한 Builder를 제공함으로써, 오류를 방지하고 이후 매개 변수가 늘어나더라도 유연하게 수정할 수 있는 구조를 제공한다.
- 스프링에서 사용하고 있음

## 불변 객체 생성의 대표적 방법
- 점층적 생성자(Telescoping Constructor) 패턴
- 자바빈(JavaBean) 패턴
- 빌더(Builder) 패턴

### 점층적 생성자 패턴(Telescoping Constructor)

>Person
> ```java
> public class Person {
>    // 필수 맴버변수
>    private String gender;
>    private String name;
>    private int age;
>    // 선택적 맴버변수
>    private String lang;
>    private double height;
>    // 필수 맴버변수 생성자
>    Person(String name,String gender,int age){
>        this.name = name;
>        this.gender = gender;
>        this.age = age;
>    }
>    // 모든 맴버 변수를 설정하는 생성자
>    Person(String name,String gender,int age,String lang,double height){
>        this.name = name;
>        this.age = age;
>        this.gender = gender;
>        this.lang = lang;
>        this.height = height;
>    }
>    // 선택적 맴버변수 생성자(lang)
>    Person(String name,String gender,int age,String lang){
>        this.name = name;
>        this.gender = gender;
>        this.age = age;
>        this.lang = lang;
>    }
>    // 선택적 맴버변수 생성자(height)
>    Person(String name,String gender,int age,double height){
>        this.name = name;
>        this.gender = gender;
>        this.age = age;
>        this.height = height;
>    }
>}
>```

>Main
>```java
>public class Main {
>    public static void main(String[] args) {
>        // 필수 맴버변수 생성
>        Person person = new Person("KIM","M",10);
>        // 선택적 맴버변수(lang) 생성
>        Person person1 = new Person("LEE","F",20,"java");
>        // 선택적 맴버변수(height) 생성
>        Person person2 = new Person("PARK","M",22,170);
>        // 모든 맴버변수 생성
>        Person person3 = new Person("CHOI","F",33,"C",166);
>    }
>}
>```
- 장점
  - 오브젝트의 일관성과 불변성 유지
- 단점
  - 멤버 변수가 늘어날 수록 코드 작업 량이 많아짐
  - 가독성이 떨어짐

### 자바빈(JavaBean) 패턴
>Person
> ```java
>public class Person {
>    private String name;
>    private String gender;
>    private int age;
>    private String lang;
>    private double height;
>    public void setName(String name) {
>        this.name = name;
>    }
>    public void setGender(String gender) {
>        this.gender = gender;
>    }
>    public void setAge(int age) {
>        this.age = age;
>    }
>    public void setLang(String lang) {
>        this.lang = lang;
>    }
>    public void setHeight(double height) {
>        this.height = height;
>    }
>}
>```

> Main
> ```java
>public class Main {
>    public static void main(String[] args) {
>        Person person = new Person();
>        person.setName("KIM");
>        person.setGender("M");
>        person.setAge(28);
>        person.setLang("Java");
>        person.setHeight(170);
>    }
>}
>```
- 장점
  - 코드의 작업량 및 가독성 좋음
  - setter 메소드에서 어떤 해당 파라미터가 어떤 변수인지 메소드를 통해 확인 가능
- 단점
  - Thread-safe 하지 않은 상태에서 절차적으로 실행되기 때문에 객체의 일관성이 깨짐
  

### 빌더(Builder) 패턴
>Person
> ```java
>public class Person {
>    private final String name;
>    private final String gender;
>    private final int age;
>    public static class Builder{
>        private final String name;
>        private final String gender;
>        private final int age;
>        private String lang;
>        private double height;
>        // 필수 맴버변수
>        public Builder(String name,String gender,int age){
>            this.name = name;
>            this. gender = gender;
>            this.age = age;
>        }
>        public Builder lang(String lang){
>            this.lang = lang;
>            return this;
>        }
>        public Builder height(double height){
>            this.height = height;
>            return this;
>        }
>        public Person build(){
>            return new Person(this);
>        }
>    }
>    private Person(Builder builder){
>        this.name = builder.name;
>        this.gender = builder.gender;
>        this.age = builder.age;
>    }
>}
>```

> Main
> ```java
>public class Main {
>    public static void main(String[] args) {
>        Person person = new Person.Builder("KIM","M",22)
>        .lang("M")
>        .height(170)
>        .build();
>    }
>}
>```
- 장점
  - 코드의 가독성 좋음
  - 일관성과 변경 불가능 유지 가능
  
- 단점
  - 빌더 패턴 구현 자체에 손이 많이 감
  - 선택 파라미터가 없거나 총 파라미터 갯수가 적을 때 빛을 발하기 어려움
  - 성능에 민감한 상황에서는 조심해서 사용

# 2. GoF 디자인 패턴에서의 Builder Pattern

- 생성에 대한 과정과 각 결과물을 표현하는 방법을 분리하여 동일한 생성 과정에 서로 다른 여러 결과물이 나올 수 있도록 함
- 클라이언트 코드는 Builder가 제공하는 메서드를 기반으로 원하는 결과물을 얻을 수 있음
- 단계별 생성에 중점을 두는 패턴
- 새로운 결과물이 필요한 경우에도 동일한 과정으로 생성을 할 수 있음

## 의도와 동기

> 생성 과정과 구현 방법을 분리하여 동일한 생성에서 여러 다른 표현이 나올 수 있음

## Class Diagram

![Class Diagram](https://www.plantuml.com/plantuml/png/jPB1JiGW48RlF0L76iDXEMwCMRS-WD4dG3gkH0gROJWOzTrbsmacRUh9xQND___b-u4vPB98PGp21Pkpx8E7IF9JoFg8Ry7oWqTmb90DBL-A3mFWNXxdZqc-QJd5ViUwUxFn19nTcgDz1qKVP-WkG1y9yDKwuAKhatC86KZnNtE3PuBp_LewhgM-IcqxVOeEWO09kxQjYYY1zl8Hqr0SxVw9pD89w6a2gEuNiTdARBKorszbBgru5hI-Q_SgsY2aAhVBvxUy_T9wTRX_kaQ8PNu2jjDPvjdbbWU8Gnd33m00)


## 객체 협력 (collaborations)
- `Builder` Product의 각 요소들을 생성하는데 필요한 추상 메서드가 선언된 클래스나 인터페이스
- `ConcreteBuilder` Builder에 선언된 메서드를 구현한 클래스
- `Director` Builder 인터페이스를 사용하여 Product를 생성
- `Product` 결과물

## 중요한 결론 (consequence)
- 생성과정과 구현을 분리함
- 제품의 다양한 구현이 가능
- 제품의 생산 과정을 더 세분화 할 수 있음
- 클라이언트는 구체적인 사항을 알 필요가 없음


## 예제 Class Diagram

![Class Diagram](https://www.plantuml.com/plantuml/png/nPAnJiGm38RtF4N68fGNg10wKH0J4gB0dkkPKqIILeup0U-EITqsS3lNfV7tf_v_LzubiaWvU3LucvBkl8D8-aDfVjIN4dph40OQKGIZzzdv0s2hR-P3mPSws7VeNruRaIzgF8r8gREuYTpUHG9yimD88tQGRn0IKBDqu7CFbb8JSzW3LlWk7bs41ighui_efcuCKsQ3kitzOy_bEXa7-kriz62n_U7_4KJA8z0JfEpPPxQkyptOcAt_cp8KskfPFLTMSUUuiZd6NA_RefQ37Xclgx9CSlLfZFp_4tuojrBRpMB2fQekRO94sa4yvE2_0G00)

## 예제 Source

### components.Engine.java
```java
public class Engine {
    private final double volume;
    private double mileage;
    private boolean started;

    public Engine(double volume, double mileage) {
        this.volume = volume;
        this.mileage = mileage;
    }

    public void on() {
        started = true;
    }

    public void off() {
        started = false;
    }

    public boolean isStarted() {
        return started;
    }

    public void go(double mileage) {
        if (started) {
            this.mileage += mileage;
        } else {
            System.err.println("Cannot go(), you must start engine first!");
        }
    }

    public double getVolume() {
        return volume;
    }

    public double getMileage() {
        return mileage;
    }
}
```
### components.GPSNavigator.java
```java
public class GPSNavigator {
    private String route;

    public GPSNavigator() {
        this.route = "221b, Baker Street, London  to Scotland Yard, 8-10 Broadway, London";
    }

    public GPSNavigator(String manualRoute) {
        this.route = manualRoute;
    }

    public String getRoute() {
        return route;
    }
}
```

### components.Transmission
```java
public enum Transmission {
    SINGLE_SPEED, MANUAL, AUTOMATIC, SEMI_AUTOMATIC
}
```

### components.TripComputer
```java
public class TripComputer {
    private Car car;

    public void setCar(Car car) {
        this.car = car;
    }

    public void showFuelLevel() {
        System.out.println("Fuel level: " + car.getFuel());
    }

    public void showStatus() {
        if (this.car.getEngine().isStarted()) {
            System.out.println("Car is started");
        } else {
            System.out.println("Car isn't started");
        }
    }
}
```

### cars.Car.java
```java
public class Car {
    private final CarType      carType;
    private final int          seats;
    private final Engine       engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;
    private       double       fuel = 0;

    public Car(CarType carType, int seats, Engine engine, Transmission transmission,
            TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        if (this.tripComputer != null) {
            this.tripComputer.setCar(this);
        }
        this.gpsNavigator = gpsNavigator;
    }

    public CarType getCarType() {
        return carType;
    }

    public double getFuel() {
        return fuel;
    }

    public void setFuel(double fuel) {
        this.fuel = fuel;
    }

    public int getSeats() {
        return seats;
    }

    public Engine getEngine() {
        return engine;
    }

    public Transmission getTransmission() {
        return transmission;
    }

    public TripComputer getTripComputer() {
        return tripComputer;
    }

    public GPSNavigator getGpsNavigator() {
        return gpsNavigator;
    }
}
```

### cars.Manual.java
```java
public class Manual {
    private final CarType      carType;
    private final int          seats;
    private final Engine       engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;

    public Manual(CarType carType, int seats, Engine engine, Transmission transmission,
            TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        this.gpsNavigator = gpsNavigator;
    }

    public String print() {
        String info = "";
        info += "Type of car: " + carType + "\n";
        info += "Count of seats: " + seats + "\n";
        info += "Engine: volume - " + engine.getVolume() + "; mileage - " + engine.getMileage() + "\n";
        info += "Transmission: " + transmission + "\n";
        if (this.tripComputer != null) {
            info += "Trip Computer: Functional" + "\n";
        } else {
            info += "Trip Computer: N/A" + "\n";
        }
        if (this.gpsNavigator != null) {
            info += "GPS Navigator: Functional" + "\n";
        } else {
            info += "GPS Navigator: N/A" + "\n";
        }
        return info;
    }
}
```

### cars.CarType.java
```java
public enum CarType {
    CITY_CAR, SPORTS_CAR, SUV
}
```

### builders.Builder.java
```java
public interface Builder {
    void setCarType(CarType type);
    void setSeats(int seats);
    void setEngine(Engine engine);
    void setTransmission(Transmission transmission);
    void setTripComputer(TripComputer tripComputer);
    void setGPSNavigator(GPSNavigator gpsNavigator);
}
```

### builders.CarBuilder.java
```java
public class CarBuilder implements Builder {
    private CarType      type;
    private int          seats;
    private Engine       engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Car getResult() {
        return new Car(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}
```

### builders.CarManualBuilder.java
```java
public class CarManualBuilder implements Builder {
    private CarType      type;
    private int          seats;
    private Engine       engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    @Override
    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Manual getResult() {
        return new Manual(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}
```

### director.Director.java
```java
public class Director {
    public void constructSportsCar(Builder builder) {
        builder.setCarType(CarType.SPORTS_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(3.0, 0));
        builder.setTransmission(Transmission.SEMI_AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructCityCar(Builder builder) {
        builder.setCarType(CarType.CITY_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(1.2, 0));
        builder.setTransmission(Transmission.AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructSUV(Builder builder) {
        builder.setCarType(CarType.SUV);
        builder.setSeats(4);
        builder.setEngine(new Engine(2.5, 0));
        builder.setTransmission(Transmission.MANUAL);
        builder.setGPSNavigator(new GPSNavigator());
    }
}
```

### Demo.java
```java
public class Demo {

  public static void main(String[] args) {
    Director director = new Director();

    // Director gets the concrete builder object from the client
    // (application code). That's because application knows better which
    // builder to use to get a specific product.
    CarBuilder builder = new CarBuilder();
    director.constructSportsCar(builder);

    // The final product is often retrieved from a builder object, since
    // Director is not aware and not dependent on concrete builders and
    // products.
    Car car = builder.getResult();
    System.out.println("Car built:\n" + car.getCarType());


    CarManualBuilder manualBuilder = new CarManualBuilder();

    // Director may know several building recipes.
    director.constructSportsCar(manualBuilder);
    Manual carManual = manualBuilder.getResult();
    System.out.println("\nCar manual built:\n" + carManual.print());
  }

}
```

## 참조
https://refactoring.guru/ko/design-patterns/builder
