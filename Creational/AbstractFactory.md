# 👷 추상 팩토리 패턴 (Abstract Factory)

* factory에 대한 추상화(interface)
* 관련 객체들의 모음(product family)을 생성

## ❔ 가구점

![가구점의 제품 패밀리들과 그 변형들](https://refactoring.guru/images/patterns/diagrams/abstract-factory/problem-ko.png?id=75caa0103b449313ce1b9eb894aaef4a)

## ❕ Solution

### 1. 추상 팩토리

![추상 팩토리 패턴 구조](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure.png?id=a3112cdd98765406af94595a3c5e7762)

### 2. 가구점을 위한 프로그램

![가구점을 위한 프로그램의 클래스 다이어그램](https://www.plantuml.com/plantuml/png/lLInJiCm5Dpz5Taegfhk2A5MHMM12SJ-choqbfouiXqa1FmxSTnA9uU26wbaIkxT-PtFprbOHyQrjH9NE-VszcO-tojgF98NkfuNYgoTbPAsXceXPEFOL5HmjcvD8rrhU4s5q-Oz-n3JrOwaoGh3vBX_drnteAugvXVQA3wK0F64PrtHeU9VE-XBYC7ojxDifIGtJeZUq-6hQpiRSAi84DD0dlp9xEoyQLbcUwCBjjPxmKs73NOxtMYjc9fiHtPfQiz3w96tGGfAI07qqZueiWbwdqnQurFvVUPvjZJkdWiT5q-mdtJ9faaalqh_8Md83-S5lIyuDw5LCFuVdN3N5smcJeMNTy_TCTg-HneLUgO9l1KX8DWXCnI3YGGYMaYW1mDntZ7tM0ysqkbe9baKB8LJnklL8u1ZCW17PgDxSkwl1YT_d1Rjd2KNEDCO1E38mL1-fVnqF75Ce_wh6DRu5J2oKOk8_gvZOXu0D3YCm6Z-WXFGZnoQKLBqSevwmKtPrkeR)

## 💻 예제

* **📚 Reference**의 [Refactoring.Guru - Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) 참고
* 이해하기 쉽게 가구점에 비유해서 아래와 같이 예제를 작성하였으나, 이보다는 위 참고사이트의 [OS별 GUI 컴포넌트 생성 예제](https://refactoring.guru/ko/design-patterns/abstract-factory/java/example) 가 더 적절한 것으로 보임

### 1. Abstract Factory

```java
public interface FurnitureFactory {
    Chair createChair();
    CoffeeTable createCoffeeTable();
    Sofa createSofa();
}
```

### 2. Product

```java
public interface Chair {
    boolean hasArms();
    boolean hasLegs();
}
```

```java
public interface CoffeeTable {
    boolean isGlass();
    boolean hasLegs();
}
```

```java
public interface Sofa {
    boolean hasArms();
    boolean hasLegs();
}
```

### 3. Concrete Factory

```java
public class ArtDecoFurnitureFactory implements FurnitureFactory {
    @Override
    public Chair createChair() {
        return new ArtDecoChair();
    }
    @Override
    public CoffeeTable createCoffeeTable() {
        return new ArtDecoCoffeeTable();
    }
    @Override
    public Sofa createSofa() {
        return new ArtDecoSofa();
    }
}
```

```java
public class ModernFurnitureFactory implements FurnitureFactory {
    @Override
    public Chair createChair() {
        return new ModernChair();
    }
    @Override
    public CoffeeTable createCoffeeTable() {
        return new ModernCoffeeTable();
    }
    @Override
    public Sofa createSofa() {
        return new ModernSofa();
    }
}
```

```java
public class VictorianFurnitureFactory implements FurnitureFactory {
    @Override
    public Chair createChair() {
        return new VictorianChair();
    }
    @Override
    public CoffeeTable createCoffeeTable() {
        return new VictorianCoffeeTable();
    }
    @Override
    public Sofa createSofa() {
        return new VictorianSofa();
    }
}
```

### 4. Concrete Product

```java
public class ArtDecoChair implements Chair {
    @Override
    public boolean hasArms() {
        return true;
    }
    @Override
    public boolean hasLegs() {
        return false;
    }
}
```

```java
public class ArtDecoCoffeeTable implements CoffeeTable {
    @Override
    public boolean isGlass() {
        return false;
    }
    @Override
    public boolean hasLegs() {
        return false;
    }
}
```

```java
public class ArtDecoSofa implements Sofa {
    @Override
    public boolean hasArms() {
        return true;
    }
    @Override
    public boolean hasLegs() {
        return false;
    }
}
```

```java
public class ModernChair implements Chair {
    @Override
    public boolean hasArms() {
        return false;
    }
    @Override
    public boolean hasLegs() {
        return false;
    }
}
```

```java
public class ModernCoffeeTable implements CoffeeTable {
    @Override
    public boolean isGlass() {
        return true;
    }
    @Override
    public boolean hasLegs() {
        return true;
    }
}
```

```java
public class ModernSofa implements Sofa {
    @Override
    public boolean hasArms() {
        return false;
    }
    @Override
    public boolean hasLegs() {
        return false;
    }
}
```

```java
public class VictorianChair implements Chair {
    @Override
    public boolean hasArms() {
        return true;
    }
    @Override
    public boolean hasLegs() {
        return true;
    }
}
```

```java
public class VictorianCoffeeTable implements CoffeeTable {
    @Override
    public boolean isGlass() {
        return false;
    }
    @Override
    public boolean hasLegs() {
        return true;
    }
}
```

```java
public class VictorianSofa implements Sofa {
    @Override
    public boolean hasArms() {
        return true;
    }
    @Override
    public boolean hasLegs() {
        return true;
    }
}
```

### 5. Client

```java
public class Client {
    private final FurnitureFactory furnitureFactory;

    public Client(FurnitureFactory furnitureFactory) {
        this.furnitureFactory = furnitureFactory;
    }

    public void process() {
        Chair chair = furnitureFactory.createChair();
        CoffeeTable coffeeTable = furnitureFactory.createCoffeeTable();
        Sofa sofa = furnitureFactory.createSofa();
        ...
    }
}
```

## Java API

* [nowonbun.tistory.com - [Java] XML를 Xpath를 이용하여 데이터를 취득하는 방법(XPathFactory)](https://nowonbun.tistory.com/562?category=507117)

### 1. DocumentBuilderFactory

#### javax.xml.parsers.DocumentBuilderFactory

```java
public abstract class DocumentBuilderFactory {
    ...
    public static DocumentBuilderFactory newInstance() {
        return FactoryFinder.find(
                /* The default property name according to the JAXP spec */
                DocumentBuilderFactory.class, // "javax.xml.parsers.DocumentBuilderFactory"
                /* The fallback implementation class name */
                "com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderFactoryImpl");
    }
    ...
    public static DocumentBuilderFactory newInstance(String factoryClassName, ClassLoader classLoader){
        //do not fallback if given classloader can't find the class, throw exception
        return FactoryFinder.newInstance(DocumentBuilderFactory.class,
                factoryClassName, classLoader, false);
    }
    ...
    public abstract DocumentBuilder newDocumentBuilder()
            throws ParserConfigurationException;
    ...
}
```

#### com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderFactoryImpl

```java
public class DocumentBuilderFactoryImpl extends DocumentBuilderFactory {
    ...
    public DocumentBuilder newDocumentBuilder()
            throws ParserConfigurationException {
        /** Check that if a Schema has been specified that neither of the schema properties have been set. */
        if (grammar != null && attributes != null) {
            if (attributes.containsKey(JAXPConstants.JAXP_SCHEMA_LANGUAGE)) {
                throw new ParserConfigurationException(
                        SAXMessageFormatter.formatMessage(null,
                                "schema-already-specified", new Object[]{JAXPConstants.JAXP_SCHEMA_LANGUAGE}));
            } else if (attributes.containsKey(JAXPConstants.JAXP_SCHEMA_SOURCE)) {
                throw new ParserConfigurationException(
                        SAXMessageFormatter.formatMessage(null,
                                "schema-already-specified", new Object[]{JAXPConstants.JAXP_SCHEMA_SOURCE}));
            }
        }

        try {
            return new DocumentBuilderImpl(this, attributes, features, fSecureProcess);
        } catch (SAXException se) {
            // Handles both SAXNotSupportedException, SAXNotRecognizedException
            throw new ParserConfigurationException(se.getMessage());
        }
    }
    ...
}
```

#### com.caucho.xml.parsers.XmlDocumentBuilderFactory

```java
public class XmlDocumentBuilderFactory extends DocumentBuilderFactory {
    ...
    public DocumentBuilder newDocumentBuilder() {
        return new XmlDocumentBuilder();
    }
    ...
}
```

#### oracle.xml.jaxp.JXDocumentBuilderFactory

```java
public class JXDocumentBuilderFactory extends DocumentBuilderFactory {
    ...
    public DocumentBuilder newDocumentBuilder() throws ParserConfigurationException {
        JXDocumentBuilder var1 = new JXDocumentBuilder();
        DOMParser var2 = var1.getDOMParser();
        if (!this.isExpandEntityReferences()) {
            var2.setAttribute(8, Boolean.FALSE);
        }

        var2.setValidationMode(this.isValidating());
        var2.retainCDATASection(!this.isCoalescing());
        var2.setIgnoringComments(this.isIgnoringComments());
        if (this.ignoreSchemaDuplicate) {
            var2.setAttribute(4, Boolean.TRUE);
        }

        if (this.isIgnoringElementContentWhitespace()) {
            var2.resetPreserveWhitespace();
        } else {
            var2.setPreserveWhitespace(true);
        }

        var2.setAttribute(30, new Boolean(this.compact_doc));
        if (!this.deterministic) {
            var2.setAttribute(11, new Boolean(this.deterministic));
        }

        if (this.qnameLocal) {
            var2.setAttribute(34, new Boolean(this.qnameLocal));
        }

        if (!this.resolveEntityDefault) {
            var2.setAttribute(9, Boolean.FALSE);
        }

        if (this.isSecure) {
            var2.setEntityDepth(11);
            var2.setAttribute(9, Boolean.FALSE);
            var2.setAttribute(13, 64000);
        }

        if (this.standalone) {
            var2.setAttribute(7, Boolean.TRUE);
        }

        if (this.attributes != null) {
            Enumeration var3 = this.attributes.keys();

            label67:
            while (true) {
                String var4;
                do {
                    if (!var3.hasMoreElements()) {
                        var1.setConnection((Connection) this.attributes.get("oracle.xml.parser.XMLDocument.Connection"));
                        var1.setDOMKind((String) this.attributes.get("oracle.xml.parser.XMLDocument.Kind"));
                        break label67;
                    }

                    var4 = (String) var3.nextElement();
                } while (!this.isValidating() && (var4.equals("http://java.sun.com/xml/jaxp/properties/schemaLanguage") || var4.equals("http://java.sun.com/xml/jaxp/properties/schemaSource")));

                var2.setAttribute(var4, this.attributes.get(var4));
            }
        }

        if (this.jxSchema != null) {
            if (this.attributes != null && (this.attributes.get("http://java.sun.com/xml/jaxp/properties/schemaLanguage") != null || this.attributes.get("http://java.sun.com/xml/jaxp/properties/schemaSource") != null)) {
                throw new ParserConfigurationException();
            }

            var1.setSchema(this.jxSchema);
        }

        return var1;
    }
    ...
}
```

### 2. XPathFactory

#### javax.xml.xpath.XPathFactory

```java
public abstract class XPathFactory {
    ...
    public static XPathFactory newInstance() {

        try {
            return newInstance(DEFAULT_OBJECT_MODEL_URI);
        } catch (XPathFactoryConfigurationException xpathFactoryConfigurationException) {
            throw new RuntimeException(
                    "XPathFactory#newInstance() failed to create an XPathFactory for the default object model: "
                            + DEFAULT_OBJECT_MODEL_URI
                            + " with the XPathFactoryConfigurationException: "
                            + xpathFactoryConfigurationException.toString()
            );
        }
    }
    ...
    public static XPathFactory newInstance(final String uri)
            throws XPathFactoryConfigurationException {

        if (uri == null) {
            throw new NullPointerException(
                    "XPathFactory#newInstance(String uri) cannot be called with uri == null");
        }

        if (uri.length() == 0) {
            throw new IllegalArgumentException(
                    "XPathFactory#newInstance(String uri) cannot be called with uri == \"\"");
        }

        ClassLoader classLoader = ss.getContextClassLoader();

        if (classLoader == null) {
            //use the current class loader
            classLoader = XPathFactory.class.getClassLoader();
        }

        XPathFactory xpathFactory = new XPathFactoryFinder(classLoader).newFactory(uri);

        if (xpathFactory == null) {
            throw new XPathFactoryConfigurationException(
                    "No XPathFactory implementation found for the object model: "
                            + uri);
        }

        return xpathFactory;
    }
    ...
    public static XPathFactory newInstance(String uri, String factoryClassName, ClassLoader classLoader)
            throws XPathFactoryConfigurationException{
        ClassLoader cl = classLoader;

        if (uri == null) {
            throw new NullPointerException(
                    "XPathFactory#newInstance(String uri) cannot be called with uri == null");
        }

        if (uri.length() == 0) {
            throw new IllegalArgumentException(
                    "XPathFactory#newInstance(String uri) cannot be called with uri == \"\"");
        }

        if (cl == null) {
            cl = ss.getContextClassLoader();
        }

        XPathFactory f = new XPathFactoryFinder(cl).createInstance(factoryClassName);

        if (f == null) {
            throw new XPathFactoryConfigurationException(
                    "No XPathFactory implementation found for the object model: "
                            + uri);
        }
        //if this factory supports the given schemalanguage return this factory else thrown exception
        if (f.isObjectModelSupported(uri)) {
            return f;
        } else {
            throw new XPathFactoryConfigurationException("Factory "
                    + factoryClassName + " doesn't support given " + uri
                    + " object model");
        }

    }
    ...
    public abstract XPath newXPath();
}
```

#### com.sun.org.apache.xpath.internal.jaxp.XPathFactoryImpl

```java
public  class XPathFactoryImpl extends XPathFactory {
    ...
    public javax.xml.xpath.XPath newXPath() {
        return new com.sun.org.apache.xpath.internal.jaxp.XPathImpl(
                xPathVariableResolver, xPathFunctionResolver,
                !_isNotSecureProcessing, _featureManager );
    }
    ...
}
```

## 🌗 장단점

### 1. 장점

* 객체 생성을 한 곳에서 관리 (*[Single Responsibility Principle](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)*)
* 제품군 추가 및 변경 시 클라이언트 코드의 수정 최소화 (*[Open/Closed Principle](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)*)

### 2. 단점

* 새로운 유형의 제품이 추가될 경우에는 많은 수정이 필요하기 때문에 부적합
* 코드가 필요 이상으로 복잡해질 수 있음

## 📚 Reference

1. [Refactoring.Guru - Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)
2. [Wikipedia - Abstract factory pattern](https://en.wikipedia.org/wiki/Abstract_factory_pattern)
3. [Weber Informatics LLC - com.caucho.xml.parsers.XmlDocumentBuilderFactory Maven / Gradle / Ivy](https://jar-download.com/artifacts/com.caucho/resin-kernel/4.0.52/source-code/com/caucho/xml/parsers/XmlDocumentBuilderFactory.java)
