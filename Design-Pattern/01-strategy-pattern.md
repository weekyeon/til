# 스트래티지 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 인터페이스 (Interface)
* 델리게이트 (Delegate)
* 스트래티지 패턴 (Strategy Pattern)
* 실습



### 인터페이스(Interface)

* 기능에 대한 선언과 구현 분리
  * 기능 A에 대해 선언하는 인터페이스와 기능 A의 구현을 분리
  * implement를 사용하여 기능 구현
* 기능을 사용하는 통로
  * 기능을 구현한 클래스의 메소드를 사용할 수 있도록 하는 접점 역할

```java
public interface InterfaceEx{
    //인터페이스 선언
    public void funcA();
}
```

```java
public class InterfaceExImpl implements InterfaceEx{
    //기능 구현   
    @Override
    public void funcA(){
        System.out.println("Hello World!")
    }
}
```

```java
public class Main{
    public static void main(String[] args){
        InterfaceEx interfaceEx = new InterfaceExImpl();
        interfaceEx.funcA();
    }
}
```



### 델리게이트 (Delegate)

* 기능 구현 시 그 책임을 다른 객체에게 위임
  * 다시 말해서, 다른 객체의 기능을 빌려서 기능 구현

```java
public class DelegateEx{
    InterfaceEx interfaceEx;
    
    public DelegateEx(){
        this.interfaceEx = new InterfaceImpl();
    }
    
    public void funcAA(){
        interfaceEx.funcA();
        interfaceEx.funcA();
    }
}
```

```java
public class Main{
    public static void main(String[] args){
        DelegateEx delegateEx = new DelegateEx();
        delegateEx.funcAA();
    }
}
```



### 스트래티지 패턴 (Strategy Pattern)

* 여러 알고리즘을 하나의 **추상적인 접근점**을 만들어 접근점에서 서로 교환 가능하도록 하는 패턴

* 기본 설계

  * 전략을 사용하는 Client가 있고, 이 Client는 전략을 하나 소유함
  * Strategy 인터페이스를 구현한 각각의 Strategy A, B, C를 Client가 사용할 수 있음
  

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/strategy1.png)



### 실습

* 신작 게임의 캐릭터와 무기 구현
* 무기 종류 : 칼, 검

![설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/strategy2.png)

* Weapon
  * Interface
  * 무기를 사용할 수 있는 접근점
  * '어떤 무기를 사용할지'에 대해 attack() 메소드 선언하고 기능에 대한 구현은 분리
* Knife, Sword
  * Strategy
  * attack() 메소드 기능 구현
* GameCharacter
  * Client
  * `setWeapon()`메소드를 통해 교환 가능
  * attack() 메소드 정의 후 Delegate를 통해 기능 위임

```java
public interface Weapon{
    //무기 구현에 접근할 수 있는 접근점
    public void attack();
}
```

```java
public class Knife implements Weapon{
    @Override
    public void attack() {
        System.out.println("칼 공격");
    }
}
```

```java
public class Sword implements Weapon{
    @Override
    public void attack() {
        System.out.println("검 공격");
    }
}
```

```java
public class GameCharacter {

    //접근점
    private Weapon weapon;

    //교환 가능
    public void setWeapon(Weapon weapon){
        this.weapon = weapon;
    }

    public void attack(){
        if(weapon == null){
            System.out.println("맨손 공격");
        }else{
            //delegate
            //어떤 무기를 들고 있는지에 따라 공격 형태가 변경된다.
            //attack()은 어떤 무기를 들고 있는지 모른다.
            //어떤 무기를 들고 있는지는 weapon이 할 것!
            weapon.attack();
        }
    }

}
```

```java
public class Main {

    public static void main(String[] args) {
        
        GameCharacter gameCharacter = new GameCharacter();
        
        //무기값==null==맨손 공격
        gameCharacter.attack();

        //무기 설정
        gameCharacter.setWeapon(new Knife());
        gameCharacter.attack();

        gameCharacter.setWeapon(new Sword());
        gameCharacter.attack();
        
    }
}
```



* 유지보수 요청 : "도끼 무기를 추가해주세요"
  * 다른 코드 변경 없이 도끼 무기를 추가할 수 있음

```java
public class Ax implements Weapon{
    @Override
    public void attack() {
        System.out.println("도끼 공격");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        
        GameCharacter gameCharacter = new GameCharacter();
        gameCharacter.attack();

        gameCharacter.setWeapon(new Knife());
        gameCharacter.attack();

        gameCharacter.setWeapon(new Sword());
        gameCharacter.attack();
        
        gameCharacter.setWeapon(new Ax());
        gameCharacter.attack();
        
    }
}
```