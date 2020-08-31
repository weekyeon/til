# 프로토 타입 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 프로토 타입 패턴
  * 프로토 타입 패턴을 통해 복잡한 인스턴스를 복사할 수 있다.
* 요구사항
* 실습
* 얕은 복사(Shallow copy)
* 깊은 복사(Deep copy)



### 프로토 타입 패턴

* 생산 비용이 높은 인스턴스를 복사하여 쉽게 생성할 수 있도록 하는 패턴
* 인스턴스 생산 비용이 높은 경우
  * 종류가 너무 많아 클래스로 정리되지 않는 경우
  * 클래스로부터 인스턴스 생성이 어려운 경우

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/prototype1.png)



### 요구사항

* 일러스트레이터와 같은 그림 그리기 툴을 개발 중입니다.
* 모양을 그릴 수 있도록 하고, 복사/붙여넣기 기능을 구현해 주세요.
* 모양을 붙여넣기 할 때 겹치지 않도록 해주세요.



### 실습

* 자바는 Cloneable 인터페이스를 지원함
  * java.lang.clone()
* 명시적으로 Cloneable 인터페이스를 구현한 Circle 클래스 생성
  * 즉, Shape 클래스가 인터페이스 역할을 하게 됨
* 실제 Shape을 상속받은 Circle 클래스에서 clone 메소드 구현

```java
public class Shape implements Cloneable{
    // Shape은 ID 값을 통해 컨트롤 할 수 있다고 가정
    private String id;

    public void setId(String id) {
        this.id = id;
    }
    public String getId() {
        return id;
    }
}
```

```java
public class Circle extends Shape{

    private int x,y,r;

    public Circle(int x, int y, int r) {
        this.x = x;
        this.y = y;
        this.r = r;
    }

    public int getX() { return x; }
    public void setX(int x) { this.x = x; }
    public int getY() { return y; }
    public void setY(int y) { this.y = y; }
    public int getR() { return r; }
    public void setR(int r) { this.r = r; }

    // Shape 복사
    public Circle shapeCopy() throws CloneNotSupportedException {
        Circle circle = (Circle) clone();

        // 복사 후 붙여넣기 시 도형 겹치지 않게 하기
        circle.x += 1;
        circle.y += 1;

        return circle;
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Circle circle1 = new Circle(1, 1, 3);
        Circle circle2 = circle1.shapeCopy();
        System.out.println(circle1.getX()+", "+circle1.getY()+", "+circle1.getR());
        System.out.println(circle2.getX()+", "+circle2.getY()+", "+circle2.getR());
    }
}
```

```text
1, 1, 3
2, 2, 3
```



### 얕은 복사(Shallow copy)

* 객체의 **주소값**을 복사함
* 자바의 argument 전달 방식은 call by value
  * 즉, 얕은 복사를 하게 되면 참조값만 복사하여 같은 참조를 가지는 두 개의 변수가 생김

```java
public class Cat{
    private String name;
    private Age age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public Age getAge() { return age; }
    public void setAge(Age age) { this.age = age; }
}
```

```java
public class Age {
    int year;
    public Age(int year) { this.year = year; }
    public int getYear() { return year; }
    public void setYear(int year) { this.year = year; }
}
```

```java
public class Main {
    public static void main(String[] args){

        Cat navi = new Cat();
        navi.setName("나비");
        navi.setAge(new Age(2017));

        // 얕은 복사
        Cat maru = navi;
        maru.setName("마루");
        navi.setAge(new Age(2018));
        System.out.println("---------- 얕은 복사 ----------");
        System.out.println(navi.getName()+"는 "+navi.getAge().getYear()+"년생 (Hash code : "+navi.hashCode()+")");
        System.out.println(maru.getName()+"는 "+maru.getAge().getYear()+"년생 (Hash code : "+maru.hashCode()+")");
    }
}
```

```text
---------- 얕은 복사 ----------
마루는 2018년생 (Hash code : 460141958)
마루는 2018년생 (Hash code : 460141958)
```



### 깊은 복사(Deep copy)

* 객체가 가지고 있는 **값**을 복사함
* Cloneable 인터페이스를 이용하여 깊은 복사 가능

```java
public class Cat implements Cloneable{
    private String name;
    private Age age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public Age getAge() { return age; }
    public void setAge(Age age) { this.age = age; }

    public Cat catCopy() throws CloneNotSupportedException {
        Cat cat = (Cat)this.clone();
        return cat;
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {

        Cat navi = new Cat();
        navi.setName("나비");
        navi.setAge(new Age(2017));

        // 깊은 복사
        Cat sandy = navi.catCopy();
        sandy.setName("샌디");
        navi.setAge(new Age(2019));
        System.out.println("---------- 깊은 복사 ----------");
        System.out.println(navi.getName()+"는 "+navi.getAge().getYear()+"년생 (Hash code : "+navi.hashCode()+")");
        System.out.println(sandy.getName()+"는 "+sandy.getAge().getYear()+"년생 (Hash code : "+sandy.hashCode()+")");
    }
}
```

```text
---------- 깊은 복사 ----------
나비는 2019년생 (Hash code : 460141958)
샌디는 2017년생 (Hash code : 1163157884)
```



* 주의할 점
  * Cloneable 인터페이스를 이용하여 깊은 복사를 할 때, **객체에 참조타입 멤버가 있으면 해당 객체는 얕은 복사**가 됨

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {

        Cat navi = new Cat();
        navi.setName("나비");
        navi.setAge(new Age(2017));

        // 깊은 복사 시 주의할 점
        Cat rami = navi.catCopy();
        rami.setName("라미");
        rami.getAge().setYear(2020);
        System.out.println("----- 깊은 복사 시 주의할 점 -----");
        System.out.println(navi.getName()+"는 "+navi.getAge().getYear()+"년생 (Hash code : "+navi.hashCode()+")");
        System.out.println(rami.getName()+"는 "+rami.getAge().getYear()+"년생 (Hash code : "+rami.hashCode()+")");
        
        System.out.println("-----------------------------------");
        System.out.println("navi.hashCode() : "+navi.hashCode());
        System.out.println("rami.hashCode() : "+rami.hashCode());
        System.out.println("navi.getAge().hashCode() : "+navi.getAge().hashCode());
        System.out.println("rami.getAge().hashCode() : "+rami.getAge().hashCode());

    }
}
```

```text
----- 깊은 복사 시 주의할 점 -----
나비는 2020년생 (Hash code : 460141958)
라미는 2020년생 (Hash code : 1163157884)
-----------------------------------
navi.hashCode() : 460141958
rami.hashCode() : 1956725890
navi.getAge().hashCode() : 356573597
rami.getAge().hashCode() : 356573597
```



* 해결
  * Cloneable 인터페이스를 구현한 객체에서 깊은 복사를 할 때, 참조타입 멤버도 명시적으로 깊은 복사 수행
    * `cat.setAge(new Age(this.age.year));`

```java
public class Cat implements Cloneable{
    private String name;
    private Age age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public Age getAge() { return age; }
    public void setAge(Age age) { this.age = age; }

    public Cat catCopy() throws CloneNotSupportedException {
        Cat cat = (Cat)this.clone();
        cat.setAge(new Age(this.age.getYear()));
        return cat;
    }
}
```

```text
----- 깊은 복사 시 주의할 점 -----
나비는 2017년생 (Hash code : 460141958)
라미는 2020년생 (Hash code : 1163157884)
-----------------------------------
navi.hashCode() : 460141958
rami.hashCode() : 1956725890
navi.getAge().hashCode() : 356573597
rami.getAge().hashCode() : 1735600054
```