# 빌더 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [기계 인간 John Grib, 빌더 패턴 (Builder Pattern)](https://johngrib.github.io/wiki/builder-pattern/)
* [Ready Kim, 준비된 개발자, [생성 패턴] 빌더 패턴(Builder pattern) 이해 및 예제](https://readystory.tistory.com/121)
* [더블에스 devlog, 이펙티브 자바 - 2. 빌더 패턴](https://doublesprogramming.tistory.com/244)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* GoF 빌더 패턴
* 실습 1
* Effective Java 빌더 패턴
* 실습 2
* 실습 3



### GoF 빌더 패턴

* 복잡한 단계를 거쳐야 생성되는 객체의 구현을 서브 클래스에게 위임하는 패턴
* 즉, 객체를 생성하는 알고리즘과 조립(표현)하는 방법을 분리한 패턴
* 예를 들어, 원하는 객체를 만들기 위해 복잡한 단계를 거쳐야 하는 경우
  * `1` 복잡한 단계를 추상화하여 알고리즘 생성
  * `2` 생성한 알고리즘을 서브 클래스에 위임
  * `3` 서브 클래스가 실제 객체 구현

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/builder1.png)

* 그림의 UML과 동일한 설계가 아니어도 빌더 패턴은 다양한 방식으로 유연하게 구현 가능
* Builder는 부품을 만들고, Director는 Builder가 만든 부품을 조합해 제품을 만듦
* **AbstractBuilder**
  * 원하는 객체를 만들기 위한 설계도
* **ConcreteBuilder**
  * AbstractBuilder 클래스를 실제 구현함
* **Director**
  * Builder 클래스(설계도)를 가지고 원하는 객체를 만드는 주체
  * 설계도를 바탕으로 객체를 생성하고, 생성된 객체를 반환함
  * 즉, Director만 어떤 부품을 어떻게 조합할 것인지를 알고 있음
* **Product**
  * Director가 Builder를 이용하여 생성한 결과물(객체)



### 실습 1

* **복잡한 단계**가 있는 인스턴스 생성 과정 단순화
* 빌더 패턴을 이용하여 컴퓨터 생성하기
  * ComputerDirector = Director
  * AbstComputerBuilder = AbstractBuilder
  * GramBuilder= ConcreteBuilder

```java
public class Computer {
    // 생성할 객체

    private String cpu;
    private String ram;
    private String storage;

    public Computer(String cpu, String ram, String storage) {
        this.cpu = cpu;
        this.ram = ram;
        this.storage = storage;
    }

    public String getCpu() {
        return cpu;
    }
    public void setCpu(String cpu) {
        this.cpu = cpu;
    }
    public String getRam() {
        return ram;
    }
    public void setRam(String ram) {
        this.ram = ram;
    }
    public String getStorage() {
        return storage;
    }
    public void setStorage(String storage) {
        this.storage = storage;
    }

    @Override
    public String toString() {
        return "Computer{" +
                "cpu='" + cpu + '\'' +
                ", ram='" + ram + '\'' +
                ", storage='" + storage + '\'' +
                '}';
    }
}
```

```java
public abstract class AbstComputerBuilder {
    public abstract void setCpu();
    public abstract void setRam();
    public abstract void setStorage();
    public abstract Computer getCom();
}
```

```java
public class GramBuilder extends AbstComputerBuilder{
    /*
        1. Computer를 Builder가 가지고 있는 경우
     */
    private Computer computer;

    public GramBuilder() {
        computer = new Computer("Default", "Default", "Default");
    }

    @Override
    public void setCpu() {
        computer.setCpu("i7");
    }

    @Override
    public void setRam() {
        computer.setRam("16GB");
    }

    @Override
    public void setStorage() {
        computer.setStorage("256GB SSD");
    }

    @Override
    public Computer getCom() {
        return this.computer;
    }
}
```

```java
public class MacBuilder extends AbstComputerBuilder{
    /*
        2. Computer를 Builder가 가지고 있지 않은 경우
     */
    private String cpu;
    private String ram;
    private String storage;

    @Override
    public void setCpu() {
        cpu = "i5";
    }

    @Override
    public void setRam() {
        ram = "8GB";
    }

    @Override
    public void setStorage() {
        storage = "125GB SSD";
    }

    @Override
    public Computer getCom() {
        return new Computer(cpu, ram, storage);
    }
}
```

```java
public class ComputerDirector {

    private AbstComputerBuilder builder;

    // 다양한 설계도를 이용하기 위해
    // 구현체가 아닌 추상 클래스를 인자로 받음
    public void setBuilder(AbstComputerBuilder builder) {
        this.builder = builder;
    }

    public void make(){
        builder.setCpu();
        builder.setRam();
        builder.setStorage();
    }

    public Computer getComputer(){
        return builder.getCom();
    }

}
```

```java
public class Main {

    public static void main(String[] args) {

        ComputerDirector director = new ComputerDirector();

        director.setBuilder(new GramBuilder());
        director.make();
        Computer gram = director.getComputer();
        System.out.println("Gram Computer : "+gram);

        director.setBuilder(new MacBuilder());
        director.make();
        Computer mac = director.getComputer();
        System.out.println("Mac Computer : "+mac);
    }

}
```

```text
Gram Computer : Computer{cpu='i7', ram='16GB', storage='256GB SSD'}
Mac Computer : Computer{cpu='i5', ram='8GB', storage='125GB SSD'}
```



### Effective Java 빌더 패턴

* 많은 인자를 가진 객체의 생성을 다른 객체의 도움으로 생성
  * 생성할 객체의 인자가 많을 경우 빌더 패턴을 사용하면 효율적
    * `1` 받아야 할 인자가 많음 `2` 선택 인자값이 많음 `3` 추가될 인자가 많음
* 팩토리 메소드 패턴과 추상 팩토리 패턴의 한계를 보완하기 위해 등장
  * 팩토리 메소드 패턴의 경우, 객체의 인자가 많으면 타입/순서 등에 관한 관리가 어려움
* 장점
  * 코드 가독성 증가
  * 불필요한 생성자를 만들지 않음
  * 객체 생성 후 변경 불가능하게 만들 수 있음
* 단점
  * 실제 인스턴스 생성 시 코드량 증가



### 실습 2

* 빌더 패턴을 이용하여 컴퓨터 생성하기
* 실습 2의 소스 코드는 강의를 바탕으로 한 코드

```java
public class Computer {
    // 생성할 객체

    private String cpu;
    private String ram;
    private String storage;

    public Computer(String cpu, String ram, String storage) {
        this.cpu = cpu;
        this.ram = ram;
        this.storage = storage;
    }

    public String getCpu() {
        return cpu;
    }
    public void setCpu(String cpu) {
        this.cpu = cpu;
    }
    public String getRam() {
        return ram;
    }
    public void setRam(String ram) {
        this.ram = ram;
    }
    public String getStorage() {
        return storage;
    }
    public void setStorage(String storage) {
        this.storage = storage;
    }

    @Override
    public String toString() {
        return "Computer{" +
                "cpu='" + cpu + '\'' +
                ", ram='" + ram + '\'' +
                ", storage='" + storage + '\'' +
                '}';
    }
}
```



* **Case 1.** CPU / RAM / 저장소 값 입력 받기

```java
public class ComputerBuilder {
    private Computer computer;
    private ComputerBuilder() {
        computer = new Computer("Default", "Default", "Default");
    }
    public static ComputerBuilder start(){
        return new ComputerBuilder();
    }
    public ComputerBuilder setCpu(String string){
        computer.setCpu(string);
        return this;
    }
    public ComputerBuilder setRam(String string){
        computer.setRam(string);
        return this;
    }
    public ComputerBuilder setStorage(String string){
        computer.setStorage(string);
        return this;
    }
    public Computer build(){
        return this.computer;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Computer computer = ComputerBuilder
                .start()
                .setCpu("i7")
                .setRam("32GB")
                .setStorage("1TB SSD")
                .build();
        System.out.println(computer);
    }
}
```

```text
Computer{cpu='i7', ram='32GB', storage='1TB SSD'}
```



* **Case 2.** CPU / RAM 값 입력 받기

```java
public class ComputerBuilder {
    private Computer computer;
    private ComputerBuilder() {
        computer = new Computer("Default", "Default", "Default");
    }
    public static ComputerBuilder start(){
        return new ComputerBuilder();
    }
    public ComputerBuilder setCpu(String string){
        computer.setCpu(string);
        return this;
    }
    public ComputerBuilder setRam(String string){
        computer.setRam(string);
        return this;
    }
    public Computer build(){
        return this.computer;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Computer computer = ComputerBuilder
                .start()
                .setCpu("i7")
                .setRam("32GB")
                .build();
        System.out.println(computer);
    }
}
```

```text
Computer{cpu='i5', ram='16GB', storage='Default'}
```



* **Case 3.** 이렇게도 사용 가능

```java
public class ComputerBuilder {
    private Computer computer;
    
    private ComputerBuilder() {
        computer = new Computer("Default", "Default", "Default");
    }

    public static ComputerBuilder startWithCpu(String cpu){
        ComputerBuilder computerBuilder = new ComputerBuilder();
        computerBuilder.computer.setCpu(cpu);
        return computerBuilder;
    }
    public ComputerBuilder setRam(String string){
        computer.setRam(string);
        return this;
    }
    public Computer build(){
        return this.computer;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Computer computer = ComputerBuilder
                .startWithCpu("i3")
                .setRam("8GB")
                .build();

        System.out.println(computer);
    }
}
```

```text
Computer{cpu='i3', ram='8GB', storage='Default'}
```



### 실습 3

* 빌더 패턴을 이용하여 컴퓨터 생성하기
* 아마도 정석적인(?) 소스 코드
* 구현 방법
  * 생성할 객체 클래스 생성
    * 해당 클래스의 생성자는 private 접근제한
    * setter 메소드 없이 getter 메소드만 가짐
    * 즉, Builder 클래스를 통해서만 객체를 생성할 수 있음
  * 별도의 Builder 클래스를 생성
    * 생성할 객체 클래스의 static 내부 클래스로 생성
    * Builder 클래스의 생성자는 public 접근제한
  * **필수 인자값은 Builder 클래스의 생성자**를 통해 입력 받음
  * **선택 인자값은 Builder 클래스의 메소드**를 통해 입력 받음
    * 메소드 리턴값은 빌더 객체 자신
    * 선택 인자값이 필요하지 않다면, 메소드를 사용하지 않으면 됨
  * `build()` 메소드를 이용해 변경 불가능한 객체를 생성
  * 최종적으로 하나의 인스턴스 반환

```java
public class Computer {

    // 필수 인자값
    private String cpu;
    private String ram;
    private String storage;

    // 선택 인자값
    private boolean isBluetoothEnabled;

    // private 생성자
    private Computer(ComputerBuilder builder) {
        this.cpu = builder.cpu;
        this.ram = builder.ram;
        this.storage = builder.storage;
        this.isBluetoothEnabled = builder.isBluetoothEnabled;
    }

    // getter 메소드만 가짐
    public String getCpu() { return cpu; }
    public String getRam() { return ram; }
    public String getStorage() { return storage; }
    public boolean isBluetoothEnabled() { return isBluetoothEnabled; }

    @Override
    public String toString() {
        return "Computer{" +
                "cpu='" + cpu + '\'' +
                ", ram='" + ram + '\'' +
                ", storage='" + storage + '\'' +
                ", isBluetoothEnabled=" + isBluetoothEnabled +
                '}';
    }

    // static 내부 클래스
    public static class ComputerBuilder {

        private String cpu;
        private String ram;
        private String storage;

        private boolean isBluetoothEnabled;

        // 필수 인자값은 생성자로 받음
        public ComputerBuilder(String cpu, String ram, String storage) {
            this.cpu = cpu;
            this.ram = ram;
            this.storage = storage;
        }

        // 선택 인자값은 메소드로 받음
        public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled){
            this.isBluetoothEnabled = isBluetoothEnabled;
            // 메소드 리턴값은 빌더 객체 자신
            return this;
        }

        public Computer build(){
            return new Computer(this);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Computer computer = new Computer.ComputerBuilder("i7", "16GB", "256GB SSD")
                .setBluetoothEnabled(true)
                .build();

        System.out.println(computer);
    }
}
```

```text
Computer{cpu='i7', ram='16GB', storage='256GB SSD', isBluetoothEnabled=true}
```

