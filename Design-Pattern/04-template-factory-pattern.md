# 팩토리 메소드 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 팩토리 메소드 패턴 (Factory Method Pattern)
  * 팩토리 메소드 패턴에서 템플릿 메소드 패턴이 사용되는 것을 알 수 있다.
  * 팩토리 메소드 패턴의 구조와 구현의 분리를 이해할 수 있다.
  * 구조와 구현의 분리에 대한 장점을 알 수 있다.
* 실습 1
* 실습 2



### 팩토리 메소드 패턴 (Factory Method Pattern)

* 상위 클래스에서 객체를 직접 생성하지 않고 하위 클래스에 위임하여 생성하는 디자인 패턴
  * 즉, 객체를 생성하는 인터페이스는 미리 정의하지만
  * 어떤 클래스에서 객체를 만들지는 하위 클래스가 결정
* 템플릿 메소드 패턴을 사용
* 추상 클래스(Interface, Abstract)를 통해 실제 구현 대상인 구체 클래스(Concrete)와 Client 간 결합도를 낮춤
* 하위 클래스에서 객체를 생성하므로 상위 클래스에서는 생성할 객체에 대해 정확히 알고 있지 않아도 됨

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/factorymethod1.png)

* Creator 클래스에 객체를 만들기 위한(원하는 작업을 수행할 수 있는) **추상 메소드** 정의
* Creator 클래스의 create() 메소드는 **실제로 제품을 만드는 메소드**이며, 템플릿 메소드 패턴으로 구현
  * 즉, create() 메소드는 팩토리 메소드
* Creator 클래스의 하위 클래스인 DefaultProductCreator 클래스에서 추상 메소드 **구현**
  * 추상 클래스의 인스턴스를 만드는 작업은 DefaultProductCreator에서 수행
  * 다시 말해서, 실제 객체를 만드는 방법을 알고 있는 유일한 클래스가 DefaultProductCreator



### 실습

* 게임 아이템과 아이템 생성을 구현해주세요.
  * 아이템을 생성하기 전에 데이터베이스에서 아이템 정보를 요청합니다.
  * 아이템을 생성 후 아이템 복제 등의 불법을 방지하기 위해 데이터베이스에 아이템 생성 정보를 남깁니다.
* 아이템을 생성하는 주체를 ItemCreator로 명합니다.
* 아이템은 item이라는 interface로 다룰 수 있도록 합니다.
  * item은 use 함수를 기본 함수로 갖고 있습니다.
* 현재 아이템의 종류는 체력 회복 물약, 마력 회복 물약이 있습니다.

```java
// 디렉토리 구조
framework
    ㄴ item
    ㄴ itemCreator
concrete
    ㄴ HpCreator
    ㄴ HpPotion
    ㄴ Main
    ㄴ MpCreator
    ㄴ MpPorion
```

```java
package study.framework;

public interface Item {
    public void use();
}
```

```java
package study.framework;

public abstract class ItemCreator {

    //아이템 생성
    public Item create(){
        Item item;

        //템플릿 메소드와 같은 형태
        //Step1
        requestItemsInfo();
        //Step2
        item = createItem();
        //Step3
        createItemLog();

        return item;
    }

    //아이템을 생성하기 전에 데이터베이스에서 아이템 정보를 요청
    abstract protected void requestItemsInfo();

    //아이템 생성 후 아이템 생성 정보 로그를 남깁니다.
    abstract protected void createItemLog();

    //아이템을 생성하는 알고리즘
    abstract protected Item createItem();
}
```

```java
package study.concrete;

import study.framework.Item;

public class HpPotion implements Item {
    @Override
    public void use() {
        System.out.println("체력 회복!");
    }
}
```

```java
package study.concrete;

import study.framework.Item;
import study.framework.ItemCreator;

import java.util.Date;

public class HpCreator extends ItemCreator {
    //체력을 회복시켜주는 물약을 생성하는 생성자
    @Override
    protected void requestItemsInfo() {
        System.out.println("데이터베이스에서 체력 회복 물약의 정보를 가져옵니다.");
    }

    @Override
    protected void createItemLog() {
        System.out.println("데이터베이스에서 체력 회복 물약을 새로 생성했습니다. "+new Date());
    }

    @Override
    protected Item createItem() {
        return new HpPotion();
    }
}
```

```java
package study.concrete;

import study.framework.Item;
import study.framework.ItemCreator;

public class Main {

    public static void main(String[] args) {
        ItemCreator itemCreator;
        Item item;

        itemCreator = new HpCreator();
        item = itemCreator.create();
        item.use();
        
        //Mp Potion 생략
    }
}
```



### 실습 2

```java
package exercise.framework;

public interface Weapon {
    public void use();
}
```

```java
package exercise.framework;

public abstract class WeaponCreator {

    public Weapon factoryMethod(){
        Weapon weapon;

        requestWeaponList();
        equipWeapon();
        weapon = attack();
		
        //팩토리 메소드의 return 값은 '객체'
        return weapon;
    }

    //소유한 무기 리스트를 가져옴
    abstract protected void requestWeaponList();

    //무기 장착
    abstract protected void equipWeapon();

    //공격
    abstract protected Weapon attack();

}
```

```java
package exercise.concrete;

import exercise.framework.Weapon;

public class AxProduct implements Weapon {
    @Override
    public void use() {
        System.out.println("도끼 공격");
    }
}
```

```java
package exercise.concrete;

import exercise.framework.Weapon;
import exercise.framework.WeaponCreator;

public class AxCreator extends WeaponCreator {
    @Override
    protected void requestWeaponList() {
        System.out.println("무기 리스트 출력");
    }

    @Override
    protected void equipWeapon() {
        System.out.println("무기 장착");
    }

    @Override
    protected Weapon attack() {
        return new AxProduct();
    }
}
```

```java
package exercise.concrete;

import exercise.framework.Weapon;
import exercise.framework.WeaponCreator;

public class Main {

    public static void main(String[] args) {

        Weapon weapon;
        WeaponCreator weaponCreator;

        weaponCreator = new AxCreator();
        weapon = weaponCreator.factoryMethod();
        weapon.use();

    }
}
```



*어찌저찌 하기는 했는데 이걸 나중에 활용할 수 있을지는 모르겠다:cold_sweat:..*

*좀 더 공부가 필요할 것 같다..*



