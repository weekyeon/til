# 데코레이터 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 데코레이터 패턴
* 실습 1
* 실습 2



### 데코레이터 패턴

* 기존에 구현되어 있는 클래스에 다양한 기능을 **동적으로 추가(덧붙임)**하는 설계 패턴
* 상황이나 조건에 따라 객체에 다양한 기능이 추가/삭제되어야 할 때 유용하게 사용
* 기능 확장이 필요할 때 상속의 대안으로 사용 가능
* 자바 I/O 클래스가 데코레이터 패턴으로 설계되어 있음
* 단점
  * 데코레이터 클래스가 계속 추가되면 클래스가 많아짐
  * 객체의 정체를 알기 힘들고 복잡해짐



![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/decorator1.png)



* Component
  * 실질적인 인스턴스를 컨트롤함
  * Decorator와 ConcreteComponent 객체를 동등하게 **다루기 위해** 존재
* ConcreteComponent
  * Component의 실질적인 인스턴스
  * Implements (realization)
  * 기능의 주체
* Decorator
  * Component와 ConcreteDecorator를 **동일 시** 하도록 해주는 역할
  * Implements (equaling)
  * Decorator는 Component가 될 수 있으며, Component를 가지고 있는 형태
    * 즉, Decorator를 가지고 Component를 감쌀 수 있음
* ConcreteDecorator
  * Decorator의 실질적인 인스턴스
  * Implements (generalization)
  * 추가된 기능의 주체
    * addedMethod()



### 실습 1

* 기본 생크림 케이크에 토핑을 추가해 주세요.
  * 토핑은 딸기와 초콜릿이 있습니다.

##### 구현 방법

* Cake *(Component)*
  * 기본 생크림 케익 추상화

* ConcreteCake *(ConcreteComponent)*
  * 기본 생크림 케익 구현체
* AbstCakeDecorator *(Decorator)*
  * 기본 생크림 케익에 추가할 토핑 추상화
  * Equaling
    * Cake 클래스를 implements 해서 Cake 클래스와 같은 역할을 함
    * 즉, AbstCakeDecorator 생성자로 Cake (Component)를 받기 때문에
    * AbstCakeDecorator의 기능을 덧붙일 수 있음
* Strawberry & Chocolate *(ConcreteDecorator)*
  * 기본 생크림 케익에 추가할 토핑 구현체
  * AbstCakeDecorator 클래스를 상속 받아 구현
  * Generalization
    * Strawberry 생성자로 Cake (Component)를 받음
    * `super(cake)`를 통해 `Cake.baseCake()` 메소드를 먼저 수행함



```java
public interface Cake {
    String baseCake();
}
```

```java
public class ConcreteCake implements Cake {
	@Override
    public String baseCake() {
        return "케이크";
    }
}
```

```java
abstract public class AbstCakeDecorator implements Cake{
    private Cake cake;
    public AbstCakeDecorator(Cake cake) {
        this.cake = cake;
    }
    
    @Override
    public String baseCake() {
        return cake.baseCake();
    }
}
```

```java
public class Strawberry extends AbstCakeDecorator {
    public Strawberry(Cake cake) {
        super(cake);
    }
    public String addStrawberry(){
        return " + 딸기";
    }
    @Override
    public String baseCake() {
        return super.baseCake() + addStrawberry();
    }
}
```

```java
public class Chocolate extends AbstCakeDecorator {
    public Chocolate(Cake cake) {
        super(cake);
    }
    public String addChocolate(){
        return " + 초콜릿";
    }
    @Override
    public String baseCake() {
        return super.baseCake()+addChocolate();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // Cake
        Cake cake = new ConcreteCake();
        System.out.println(cake.baseCake());

        // Cake + Strawberry
        Cake strawberryCake = new Strawberry(cake);
        System.out.println(strawberryCake.baseCake());

        // Cake + Strawberry + Chocolate
        Cake chocolateStrawberryCake = new Chocolate(strawberryCake);
        System.out.println(chocolateStrawberryCake.baseCake());
    }
}
```

```text
케이크
케이크 + 딸기
케이크 + 딸기 + 초콜릿
```



### 실습 2

* 주문한 음료의 총 가격을 산출하는 프로그램을 만들어 주세요.
  * 바닐라 라떼에 샷 1개 추가해서 한 잔
  * 아메리카노에 헤이즐넛 시럽 추가해서 한 잔

##### 기본 설계

* 음료 총 가격 측정 클래스 *(Component, ConcreteComponent)*
* 음료 총 가격과 음료에 추가한 커스텀(토핑)을 동일 시 해주는 클래스 *(Decorator)*
* 음료 커스텀 구현 클래스 *(ConcreteDecorator)*

```java

```







