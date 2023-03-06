# Proxy Pattern

### 프록시 패턴이란
- 대상 원본 객체를 대리하여 대신 처리하게 함으로써 로직의 흐름을 제어하는 행동 패턴
- 클라이언트가 대상 객체를 직접 쓰는게 아니라 중간에 프록시(대리인)을 거쳐서 쓰는 코드 패턴
-- --
### 다양한 프록시 패턴의 종류들
> Proxy 패턴은 단순하면서도 자주 쓰이는 패턴이며, 그 활용 방식도 다양하다.  
> Proxy 패턴의 기본형을 어떤 방식으로 변형하느냐에 따라 프록시 종류가 나뉘어지게 된다.
* 가상 프록시(Virtual Proxy)
* 원격 프록시(Remote Proxy)
* 보호 프록시(Protection Proxy, Access Proxy)
* 캐싱 프록시(Caching Proxy)
* 로깅 프록시(Logging Proxy)
* 방화벽 프록시, 스마트 레퍼런스 프록시, 동기화 프록시, 복잡도 숨김 프록시, 지연 복사 프록시..
## Class Diagram
-- --
![img.png](../asset/decorator/proxy-class-diagram.png)

- `Subject` Proxy 와 RealSubject 모두 Subject 인터페이스를 구현해야 함
- `RealSubject` 진짜 작업을 대부분 처리하는 원본 대상 객체
- `Proxy` 원본 대상 객체로의 접근을 제어하는 대리인
- `Client` Proxy 객체로 데이터를 주고받는 클라이언트

## 예제
-- --
#### Client.java
```java
public class Client {
    public static void main(String[] args) {
        // 원본 객체 실행
        Subject realSubject = new RealSubject();
        realSubject.operation();

        // Proxy 객체를 통한 실행
        Subject proxy = new Proxy(new RealSubject());
        proxy.operation();

        // 가상 프록시 실행
        Subject virtualProxy = new VirtualProxy();
        virtualProxy.operation();

        // 보호 프록시 실행
        Subject protectionProxy = new ProtectionProxy(new RealSubject(), true);
        protectionProxy.operation();
    }
}
```
-- --
#### Subject.java
```java
public interface Subject {
    void operation();
}
```
-- --
#### RealSubject.java
```java
public class RealSubject implements Subject {

    @Override
    public void operation() {
        System.out.println("원본 객체 - 핵심 로직 실행 !!");
    }
}
```
-- --
#### Proxy.java
```java
public class Proxy implements Subject {

    private final RealSubject realSubject;

    Proxy(RealSubject realSubject) {

        this.realSubject = realSubject;
    }

    @Override
    public void operation() {

        System.out.println("\n=== Logging Proxy - 전처리 ===");

        // 핵심 로직 실행
        realSubject.operation();

        System.out.println("=== Logging Proxy - 후처리 ===\n");
    }
}
```
-- --
### 가상 프록시 (Virtual Proxy)
- 지연 초기화 방식
- 서비스가 시작될 때 객체를 생성하는 대신에 객체 초기화가 실제로 필요한 시점에 초기화
- 실제 객체의 생성에 많은 자원이 소모 되지만 사용 빈도는 낮을 때 쓰는 방식
#### VirtualProxy.java
```java
public class VirtualProxy implements Subject {

    private RealSubject realSubject;

    @Override
    public void operation() {

        System.out.println("\n=== Virtual Proxy - 전처리 ===");

        // Runtime 실행 시, 객체 생성
        if (realSubject == null) {
            System.out.println("원본 객체 초기화 !!");
            realSubject = new RealSubject();
        }

        // 핵심 로직 실행
        realSubject.operation();

        System.out.println("=== Virtual Proxy - 후처리 ===\n");
    }
}
```
-- --
### 보호 프록시 (Protection Proxy)
- 프록시 객체를 통해 클라이언트의 자격 증명이 기준과 일치하는 경우에만 서비스 객체에 요청을 전달할 수 있게 한다.
#### ProtectionProxy.java
```java
public class ProtectionProxy implements Subject {

    private final RealSubject realSubject;
    private final boolean     isAccess;

    public ProtectionProxy(RealSubject realSubject, boolean isAccess) {

        this.realSubject = realSubject;
        this.isAccess = isAccess;
    }

    @Override
    public void operation() {

        System.out.println("\n=== Protection Proxy - 전처리 ===");

        // 접근 권한 확인
        if (isAccess) {
            // 핵심 로직 실행
            realSubject.operation();

            System.out.println("=== Protection Proxy - 후처리 ===\n");
        }

    }
}
```

### 관련 패턴
-- --
> **Decorator 패턴**
> 
> 종종 `프록시`와 `데코레이터` 패턴이 똑같아 보이기도 하는데, `용도`나 `목적`으로 구분할 수 있다.   
> - `데코레이터`: 클래스에 새로운 행동을 추가하는 용도로 쓰인다. (샷 추가, 휘핑크림 추가)
> - `프록시`: 어떤 클래스로의 접근을 제어하는 용도로 쓰인다.

### 장점
- 개방 폐쇄 원칙(OCP) 준수: 기존 객체를 수정하지 않고 일련의 로직을 프록시 패턴을 통해 추가할 수 있다.
- 단일 책임 원칙(SRP) 준수: 대상 객체는 자신의 기능에만 집중 하고, 그 이외 부가 기능을 제공하는 역할을 프록시 객체에 위임하여 다중 책임을 회피 할 수 있다.
- 실제 객체를 수행하기 이전에 전처리를 하거나, 기존 객체를 캐싱할 수 있다.
- 기존 객체의 리소스가 무거울 경우, 이러한 캐싱 과정을 통해 부하를 줄일 수 있는 장점이 있다.
- 접근 권한을 부여할 수 있다.

### 단점
- 코드의 복잡도가 증가한다.
- 로직이 난해해져 가독성이 떨어질 수 있다.
- 객체를 생성할때 한단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될 수 있다.

-- --
## Spring AOP (Aspect Oriented Programming) - 프록시 디자인 패턴 사용
### JDK Dynamic Proxy
- proxy 생성을 위해 interface가 필요하다.
- Refelction을 이용해 proxy를 생성한다.
### CGLib Proxy
- 바이트 코드를 조작해 프록시 생성
- Hibernate의 lazy loading, Mockito의 모킹 메서드 등에서 사용
-- --
## 참조
- https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%94%84%EB%A1%9D%EC%8B%9CProxy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90
- 헤드 퍼스트 디자인 패턴