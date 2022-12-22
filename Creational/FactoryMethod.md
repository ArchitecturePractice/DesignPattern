## Factory method pattern

### Factory method?

> Factory Method Pattern (팩토리 메소드 패턴) 은 객체를 생성할 때 부모 클래스에서 객체 생성 인터페이스를 제공하고 인스턴스 생성을 서브 클래스에 위임하는 생성 패턴 이다.



![팩토리 메서드 패턴 구조](https://refactoring.guru/images/patterns/diagrams/factory-method/structure.png?id=4cba0803f42517cfe8548c9bc7dc4c9b)

* Product (Interface)

  > 공통의 객체생성 인터페이스

* ConcreteProdect

  > 인터페이스를 통해 구현된 결과

* Creator

  > Product를 반환

* ConcreateCreator

  > 알맞는 Product의 생성 및 반환

#### Java 구현

```java
public interface Send {

    void sendMessage(String msg);
}
```

```java
public class SendSms implements Send {

    @Override
    public void sendMessage(String msg) {
        /*Send Sms Message Biz*/
    }
}
```

```java
public class SendMms implements Send {

    @Override
    public void sendMessage(String msg) {
        /*Send Mms Message Biz*/
    }
}
```

```java
public class SendKakaotalk implements Send{

    @Override
    public void sendMessage(String msg) {
        /*Send KakaoTalk Message Biz*/
    }
}
```

```java
public class SendFactory {

    public static final String SMS   = "SMS";
    public static final String MMS = "EMAIL";
    public static final String KAKAOTALK  = "PUSH";

    public Send createSend(String type) {

        if (type == null || type.isEmpty()) {
            return null;
        } else {
            switch (type) {
                case SMS:
                    return new SendSms();
                case MMS:
                    return new SendMms();
                case KAKAOTALK:
                    return new SendKakaotalk();
                default:
                    throw new IllegalArgumentException("Unknown Send Type");
            }
        }
    }
}
```

```java
public class SendService {

    public static void main(String[] args) {
        try {
            SendFactory sendFactory = new SendFactory();
            Send smsSend = sendFactory.createSend("SMS");
            smsSend.sendMessage("It's time to go home");
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

#### Spring 구현

```java
public interface SendService {

    SendType getSendType();

    void sendMessage(String message);
}
```

```java
@Service
public class SmsSendServiceImpl implements SendService{

    @Override
    public SendType getSendType() {

        return SendType.SMS;
    }

    @Override
    public void sendMessage(String message) {

        /*message with sms*/
    }
}
```

```java
@Component
public class SendServiceFactory {

    private final Map<SendType, SendService> sendServices = new HashMap<>();

    public SendServiceFactory(List<SendService> sendServices) {
        if (CollectionUtils.isEmpty(sendServices)) {
            throw new IllegalArgumentException("SendType 읎다");
        }
        for (SendService sendService : sendServices) {
            this.sendServices.put(sendService.getSendType(), sendService);
        }
    }

    public SendService getService(SendType sendType) {
        return sendServices.get(sendType);
    }
}
```

```java
@Service
public class MessageSendService {

    private final SendServiceFactory sendServiceFactory;

    public MessageSendService(
            SendServiceFactory sendServiceFactory
    ) {

        this.sendServiceFactory = sendServiceFactory;
    }

    public void sendOrder(MessageReceipt messageReceipt) {

        sendServiceFactory.getService(messageReceipt.getSendType()).deliverItem(messageReceipt.getContent);
    }
}
```

### Factory Method 장단점

#### 장점

- Product 와 Creator 간의 결합도가 낮다.
- Create Product Code를 더 쉽게 유지보수 할 수 있다.
- 기존 Client Code 의 수정 없이 새로운 제품에 대한 생성 로직을 추가하는데 있어 용이하다.

#### 단점

- Factory Method 도입으로 인해 구현된 클래스들로 인해 코드량이 증가하고 복잡해 질 수 있다.

### Factory Method in Java Api

> java.util.Calendar#getInstance

```java
private static Calendar createCalendar(TimeZone zone,Locale aLocale){
...
    if (aLocale.hasExtensions()) {
        String caltype = aLocale.getUnicodeLocaleType("ca");
        if (caltype != null) {
            switch (caltype) {
            case "buddhist":
            cal = new BuddhistCalendar(zone, aLocale);
                break;
            case "japanese":
                cal = new JapaneseImperialCalendar(zone, aLocale);
                break;
            case "gregory":
                cal = new GregorianCalendar(zone, aLocale);
                break;
            }
        }
    }
...
}
```

> java.text.NumberFormat#getInstance

```java
private static NumberFormat getInstance(LocaleProviderAdapter adapter,
                                            Locale locale, int choice) {
        NumberFormatProvider provider = adapter.getNumberFormatProvider();
        NumberFormat numberFormat = null;
        switch (choice) {
        case NUMBERSTYLE:
            numberFormat = provider.getNumberInstance(locale);
            break;
        case PERCENTSTYLE:
            numberFormat = provider.getPercentInstance(locale);
            break;
        case CURRENCYSTYLE:
            numberFormat = provider.getCurrencyInstance(locale);
            break;
        case INTEGERSTYLE:
            numberFormat = provider.getIntegerInstance(locale);
            break;
        }
        return numberFormat;
    }

```

> java.nio.charset.Charset#forName()

```java
public static Charset forName(String charsetName) {
        Charset cs = lookup(charsetName);
        if (cs != null)
            return cs;
        throw new UnsupportedCharsetException(charsetName);
    }
```

> 그 외
>
> java.util.ResourceBundle#getBundle()
>
> java.net.URLStreamHandlerFactory#createURLStreamHandler(String)
>
> java.util.EnumSet#of()
>
> javax.xml.bind.JAXBContext#createMarshaller()

### Factory Method In Spring

![img](https://velog.velcdn.com/images%2Fweekbelt%2Fpost%2Fc9cfe500-9285-439c-a617-dd37af10befc%2Ffactorymethod01.png)

```java
public interface BeanFactory {

    Object getBean(String name) throws BeansException;

    <T> T getBean(String name, Class<T> requiredType) throws BeansException;

    Object getBean(String name, Object... args) throws BeansException;

    <T> T getBean(Class<T> requiredType) throws BeansException;

    <T> T getBean(Class<T> requiredType, Object... args) throws BeansException;
    
    //...
}
```

- DI(Dependency Injection) 컨테이너에서 이 패턴을 사용
- 각각의 getBean의 Factory Method 이다.
- pplicationContext 인터페이스로 BeanFactory를 확장하여 추가 기능들을 추가하여 구성한다.

## Static Factory Method

> 팩토리 패턴에서 용어를 가져와 정의한 객체 생성 메서드 기법



ex) LocalTime

```java
public static LocalTime of(int hour, int minute) {
  ChronoField.HOUR_OF_DAY.checkValidValue((long)hour);
  if (minute == 0) {
    return HOURS[hour];
  } else {
    ChronoField.MINUTE_OF_HOUR.checkValidValue((long)minute);
    return new LocalTime(hour, minute, 0, 0);
  }
}
```

ex) Optional

```java
public static <T> Optional<T> of(T value) {
        return new Optional<>(value);
    }
```

- 직접적으로 생성자를 통해 객체를 생성 하지 않고 of 메서드 자체를 통해서 객체를 생성

### Static Factory Method 의 장단점

#### 장점

1. 이름을 가질 수 있다.

   > 객체 내부의 구조를 알고 있어야 목적에 맞는 객체를 생성할 수 있는 new 를 통한 생성 방법 과는 대조적으로 메서드 이름만으로 생성 목적을 담아낼 수 있다.

   ##### Naming Convention

    - `from` : 하나의 매개 변수를 받아서 객체를 생성
    - `of` : 여러개의 매개 변수를 받아서 객체를 생성
    - `getInstance` | `instance` : 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
    - `newInstance` | `create` : 새로운 인스턴스를 생성
    - `get[OtherType]` : 다른 타입의 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
    - `new[OtherType]` : 다른 타입의 새로운 인스턴스를 생성.

2. 객체 생성의 관리가 가능하다

   ```java
   class Singleton {
       private static Singleton singleton = null;
   
       private Singleton() {}
   
       static Singleton getInstance() {
           if (singleton == null) {
               singleton = new Singleton();
           }
   
           return singleton;
       }
   }
   ```

3. 객체 생성의 캡슐화가 가능하다

   ```java
   public class ProductDto {
     private String name;
     private int productType;
   
     public static ProductDto from(Product product) {
       return new ProductDto(product.getName(), product.getProductType());
     }
   }
   ```

   ```java
   ProductDto productDto = productDto.from(product) // Static Factory Method사용
   ```

   ```java
   ProductDto productDto = new ProductDto(product.getName(), product.getProductType) // 생성자 사용
   ```

4. 반환 타입의 하위 타입의 객체의 반환이 가능하다

   ```java
   public static <T> List<T> asList(T... a) {
       return new ArrayList<>(a);
   }
   ```

5. 정적 팩토리 메소드 작성 시점에는 반환할 클래스가 존재하지 않아도 된다.

   ex) JDBC

   Connection: 서비스 인터페이스 역할

   DriverManager.registerDriver: 제공자 등록 API 역할

   DriverManager.getConnection : 서비스 접근 API 역할

   Driver: 서비스 제공자 인터페이스 역할

   ```java
   Class.forName("Driver full Name");
   Connection conn = null;
   conn = DriverManager.getConnection("url", "user", "password");
   ```

> Driver Class의 API 설명

```
When a Driver class is loaded, it should create an instance of itself and register it with the DriverManager. This means that a user can load and register a driver by calling
   Class.forName("foo.bah.Driver")
```

#### 단점

Static Factory Method 만으로는 하위 클래스의 생성이 불가하다.

### 정리

Static Factory Method 는 생성자를 대신하는 용도와 더불어 효율적이고 가독성이 좋은 프로그래밍을 제공하는 하나의 기법이라고 할 수 있다. 


