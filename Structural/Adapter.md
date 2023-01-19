## **Adapter pattern**

![img](https://blog.leocat.kr/assets/img/trip/italia/plug-type-1.jpg)

-동일하지 않은 클래스들이 유사한 동작을 하지만 인터페이스가 다르네 ...

-서로 다른 이 두 클래스를 공통 클래스로 만들면 코드가 더 간결해 지겠는걸 ...

-이 라이브러리의 인터페이스를 변경하고 싶은데 이미 상당 부분에서 이 라이브러리를 쓰고 있네 ...

![img](https://sitem.ssgcdn.com/37/10/08/item/1000033081037_i1_1100.jpg)

> Adapter !
>
> 클래스의 인터페이스를 내가 원하는 인터페이스의 형태로 변환시킨다. 어댑터를 사용하면 서로 일치하지 않는 인터페이스들의 호환성 문제를 해결해주고 같이 쓸 수 없는 클래스들을 함께 동작시킬 수 있다.

### **Structure**

![img](https://refactoring.guru/images/patterns/diagrams/adapter/structure-object-adapter.png?id=33dffbe3aece294162440c7ddd3d5d4f)

- Client : 기존의 비지니스 로직을 포함한 클래스
- Client Interface : 서로 다른 클래스들을 같이 쓰기위한 규약
- Adapter : 어댑터 인터페이스를 통해 클라이언트의 호출로 부터 서비스 객체를 변환 전달
- Service : 클라이언트로 직접적으로 호출 되지 않고 Adapter에 의해 호출되는 비지니스 로직

#### **장점**

- 비지니스 로직에서 인터페이스, 변환 로직등을 분리 가능 하다.
- 클라이언트 코드는 서비스 클래스가 아닌 클라이언트 인터페이스를 통해 호출 되기 때문에 기존의 코드의 변화 없이 새로운 유형의 어댑터들을 추가할 수가 있다.

#### **단점**

- 인터페이스 클래스와 어댑터 도입으로 인한 코드 복잡성 및 뎁스 증가

#### 예제

![](http://drive.google.com/uc?export=view&id=19WjhYnOzdgR7W1EP0i3Lg6dxD32XlCjM)

```
public interface CTypeCharger {

    public String chargeReadyCTypeDevice();
}

```

```
public class GalaxyCharger implements CTypeCharger{

    @Override
    public String chargeReadyCTypeDevice() {

        return "Now you can charge Ctype phone";
    }
}

```

```
public class IphoneCharger {

    public String chargeReady8PinTypeDevice() {

        return "Now you can charge 8Pin phone";
    }
}

```

```
public class IphoneChargerAdapter implements CTypeCharger{

    IphoneCharger iphoneCharger;

    public IphoneChargerAdapter(IphoneCharger iphoneCharger) {

        this.iphoneCharger = iphoneCharger;
    }

    @Override
    public String chargeReadyCTypeDevice() {

        return "Using adapter! You can charge Ctype Device";
    }
}

```

```
public class Client {

    public static void main(String[] args) {

        CTypeCharger cTypeCharger = new IphoneChargerAdapter(new IphoneCharger());
        System.out.println(cTypeCharger.chargeReadyCTypeDevice());
    }
}

```

### Peg 예제

![Structure of the Adapter pattern example](https://refactoring.guru/images/patterns/diagrams/adapter/example.png?id=9d2b6857ce256f2c669383ce4df3d0aa)

```
public class RoundPeg {

    private int radius;

    public RoundPeg(int radius) {

        this.radius = radius;
    }

    public int getRadius() {

        return radius;
    }
}
```

```
public class SquarePeg {

    private int width;

    public SquarePeg(int width) {

        this.width = width;
    }

    /*get set*/
}
```

```
public class SquarePegAdapter extends RoundPeg{

    SquarePeg squarePeg;

    public SquarePegAdapter(SquarePeg squarePeg) {

        this.squarePeg = squarePeg;
    }

    @Override
    public int getRadius() {

        return (int) Math.sqrt(squarePeg.getWidth())/2;
    }
}

```

```
public class RoundHole {

    private int radius;

    public RoundHole(int radius) {

        this.radius = radius;
    }

    public boolean fits(RoundPeg roundPeg){

        return roundPeg.getRadius() <= radius;
    }

    /*get set*/
}
```

```
RoundHole roundHole = new RoundHole(5);
SquarePeg squarePeg = new SquarePeg(3);
if( roundHole.fits(new SquarePegAdapter(squarePeg))){
   System.out.println("들어감");
}
```

#### 사용 사례

- java.util.Collections#enumeration()

```
~
Enumeration<String> enumeration = Collections.enumeration(list);    
~

public static <T> Enumeration<T> enumeration(final Collection<T> c) {
       return new Enumeration<T>() {
           private final Iterator<T> i = c.iterator();

           public boolean hasMoreElements() {
               return i.hasNext();
          }

           public T nextElement() {
               return i.next();
          }
      };
  }
```

- java.io.InputStreamReader

```
~
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
~

public InputStreamReader(InputStream in) {
       super(in);
       try {
           sd = StreamDecoder.forInputStreamReader(in, this, (String)null); // ## check lock object
      } catch (UnsupportedEncodingException e) {
           // The default encoding should always be available
           throw new Error(e);
      }
  }
```

- DispatcherServlet HandlerAdapter

```
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
~
                    mappedHandler = this.getHandler(processedRequest);
~

                    HandlerAdapter ha = this.getHandlerAdapter(mappedHandler.getHandler());
~
    }
    
 protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
~
            while(var2.hasNext()) {
                HandlerMapping hm = (HandlerMapping)var2.next();
                if (this.logger.isTraceEnabled()) {
                    this.logger.trace("Testing handler map [" + hm + "] in DispatcherServlet with name '" + this.getServletName() + "'");
                }
~
    }
    
    
protected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {
~
            while(var2.hasNext()) {
                HandlerAdapter ha = (HandlerAdapter)var2.next();
                if (this.logger.isTraceEnabled()) {
                    this.logger.trace("Testing handler adapter [" + ha + "]");
                }
~
    }



```

#### Deprecate 사례

- Spring Security WebSecurityConfigurerAdapter

https://github.com/spring-projects/spring-security/issues/10822

https://www.baeldung.com/spring-deprecated-websecurityconfigureradapter

> Component 기반의 Security 설정을 권장 하기 위함

(기존)

```
@Configuration
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests((authz) -> authz
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults());
    }

}
```

(변경후)

```
@Configuration
public class SecurityConfiguration {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests((authz) -> authz
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults());
        return http.build();
    }

}
```

