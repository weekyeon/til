# 추상 팩토리 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 추상 팩토리 패턴
* 실습 



### 추상 팩토리 패턴

* 서로 관련있는 객체의 생성을 가상화할 수 있도록 해주는 패턴
  * 인터페이스를 이용하여 구상 클래스를 지정하지 않고도 객체 생성 가능
  * *관련있다 :* 서로 연관되거나 의존하는 객체
* 즉, 관련있는 객체들을 **한 가지 팩토리**로 묶어서 **동일한 방식**으로 객체를 생성하는 패턴
* 팩토리 메소드 패턴의 확장판
  * 팩토리 메소드 패턴
    * 한 종류의 객체를 생성하는 역할
    * 조건에 따른 객체 생성을 팩토리 클래스에 위임하고 팩토리 클래스에서 객체 생성
  * 추상 팩토리 패턴
    * 관련있는 객체들을 일정하게 생성하는 역할
    * 관련있는 객체들을 묶어 팩토리 클래스로 생성하고 해당 팩토리를 조건에 따라 생성하도록 다시 팩토리를 만들어 객체 생성
* 관련있는 객체의 생성을 캡슐화|추상화하기 위해 사용
* 클라이언트에서 객체가 바뀌어도 코드 변경 불필요
* 팩토리 메소드 패턴을 사용하던 상황에서 팩토리의 종류를 늘려야할 때 사용 가능



![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/abstract-factory1.png)



* AbstractFactory
  * 생성해야 하는 객체들의 **관련성**을 생성
  * 클라이언트에게 인터페이스(API) 제공
    * 인터페이스만 사용하여 부품을 조립하고 생성
    * 즉, 추상적인 부품 조합 ▶ 추상적인 제품 생성
* Client
  * AbstractFactory와 AbstractProduct를 이용하여 활용함
    * 실제로 어떤 제품이 생성되는지 알 필요 없음
    * 즉, 클라이언트와 팩토리에서 생성되는 제품의 분리 가능



### 실습

* ComputerFactory *(AbstractFactory)*
* FactoryInstance.getComputerFactory() *(ConcreteFactory)*
* KeyboardProduct & MouseProduct *(AbstractProduct)*
* AppleKeyboardProduct & AppleMouseProduct *(ConcreteProduct)*
* SamsungKeyboardProduct & SamsungMouseProduct *(ConcreteProduct)*

```java
package exercise.abst;

public interface ComputerFactory {
    public MouseProduct createMouse();
    public KeyboardProduct createKeyboard();
}
```

```java
package exercise.abst;

public interface KeyboardProduct {
    public String getKeyboard();
}
```

```java
package exercise.abst;

public interface MouseProduct {
    public String getMouse();
}
```

```java
package exercise.concrete;

import exercise.abst.ComputerFactory;
import exercise.abst.KeyboardProduct;
import exercise.abst.MouseProduct;

public class FactoryInstance {
    public static ComputerFactory getComputerFactory(String str){
        switch (str){
            case "Apple":
                return new AppleFactory();
            case "Samsung":
                return new SamsungFactory();
        }
        return null;
    }
}

/*
    어떤 환경에서 프로그램을 동작하든, 동일한 동작을 위해서는
    코딩이 OS와 상관없이 동일하게 적용되어야 함

    즉, main 메소드에서 new 연산자를 이용해 객체의 할당을 바꾸어주는 것은
    추상 팩토리 패턴을 적절하게 사용한 것이 아님

    FactoryInstance를 통해 객체를 일정하게 넘겨준다면, main 메소드는 건들 필요가 없음
    다시 말해서, concrete package와 abst package는 라이브러리 형태로 제공될 것이고
    사용자는 인터페이스(API)만 사용해서 원하는 기능을 구현하면 됨
        - ComputerFactory, KeyboardProduct, MouseProduct
 */

// AppleFactory
class AppleFactory implements ComputerFactory {
    @Override
    public MouseProduct createMouse() {
        return new AppleMouseProduct();
    }

    @Override
    public KeyboardProduct createKeyboard() {
        return new AppleKeyboardProduct();
    }
}

// AppleKeyboardProduct
class AppleKeyboardProduct implements KeyboardProduct {
    @Override
    public String getKeyboard() {
        return "Apple Keyboard Product";
    }
}

// AppleMouseProduct
class AppleMouseProduct implements MouseProduct {
    @Override
    public String getMouse() {
        return "Apple Mouse Product";
    }
}

// SamsungFactory
class SamsungFactory implements ComputerFactory {
    @Override
    public MouseProduct createMouse() {
        return new SamsungMouseProduct();
    }

    @Override
    public KeyboardProduct createKeyboard() {
        return new SamsungKeyboardProduct();
    }
}

// SamsungKeyboardProduct
class SamsungKeyboardProduct implements KeyboardProduct {
    @Override
    public String getKeyboard() {
        return "Samsung Keyboard Product";
    }
}

// SamsungMouseProduct
class SamsungMouseProduct implements MouseProduct {
    @Override
    public String getMouse() {
        return "Samsung Mouse Product";
    }
}
```

```java
public class Main {
    public static void main(String[] args) {

        /*ComputerFactory computerFactory = new AppleFactory();*/
        ComputerFactory computerFactory = FactoryInstance.getComputerFactory("Samsung");
        MouseProduct mouseProduct = computerFactory.createMouse();
        KeyboardProduct keyboardProduct = computerFactory.createKeyboard();

        System.out.println(mouseProduct.getMouse());
        System.out.println(keyboardProduct.getKeyboard());

    }
}
```

```text
Samsung Mouse Product
Samsung Keyboard Product
```