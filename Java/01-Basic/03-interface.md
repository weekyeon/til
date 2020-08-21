# 인터페이스

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 인터페이스
* 인터페이스 구성 멤버
* 구현 객체와 구현 클래스
* 익명 구현 객체 (Anonymouse Class)
* 다중 인터페이스 구현 클래스
* 타입 변환과 다형성
* 인터페이스 상속
* 디폴트 메소드와 인터페이스 확장



### 인터페이스

* **객체 사용 설명서**로 객체의 사용 방법을 정의한 타입
* 객체의 교환성을 높여주기 때문에 다형성을 구현하는 매우 중요한 역할
  * 자바 8의 람다식은 함수적 인터페이스의 구현 객체를 생성함
* 인터페이스는 개발 코드와 객체가 서로 통신하는 **접점** 역할
  * 개발 코드가 인터페이스의 메소드 호출 시, 인터페이스는 객체의 메소드 호출
  * 개발 코드는 객체의 내부 구조를 알 필요가 없고 인터페이스의 메소드만 알고 있으면 됨
* 왜 인터페이스를 사용할까?
  * 개발 코드를 수정하지 않고 사용하는 객체를 변경할 수 있도록 하기 위함
  * 인터페이스는 하나의 객체가 아니라 여러 객체들과 사용 가능
  * 인터페이스가 어떤 객체를 사용하느냐에 따라 실행 내용과 리턴값이 다를 수 있음
  * 개발 코드 측면에서는 코드 변경 없이 실행 내용과 리턴값을 다양화 할 수 있음
* `interface` 키워드 사용하여 선언
* 개발 코드에서 인터페이스 선언 위치
  * 클래스의 필드/생성자
  * 메소드의 매개 변수/생성자
  * 메소드의 로컬 변수



### 인터페이스 구성 멤버

* 상수와 메소드만을 구성 멤버로 가짐
* 객체로 생성할 수 없기 때문에 **생성자를 가질 수 없음**
* 자바 8부터 디폴트 메소드와 정적 메소드도 선언 가능

```java
public interface InterfaceName{
    //상수
    타입 상수명 = 값;
    
    //추상 메소드
    타입 메소드명(매개변수, ...);
    
    //디폴트 메소드
    default 타입 메소드명(매개변수, ...){ ... }
    
    //정적 메소드
    static 타입 메소드명(매개변수){ ... }
}
```

* 상수 필드

  * 인터페이스는 런타임 시 데이터를 저장할 수 있는 필드를 선언할 수 없음
  * 하지만, 상수 필드는 선언 가능
    * 인터페이스에 고정된 값으로, 런타임 시 데이터를 바꿀 수 없음
  * 인터페이스에 선언된 필드는 모두 `public static final` 특성
  * 인터페이스 상수는 static { } 블록으로 초기화 불가능
    * 반드시 선언과 동시에 초기값 지정 필요

* 추상 메소드

  * 추상 메소드는 객체가 가지고 있는 메소드를 설명한 것
    * 호출 시 어떤 매개값이 필요하고, 리턴 타입이 무엇인지만 알려줌
  * 실제 실행부는 구현 객체가 가지고 있음
  * 인터페이스를 통해 호출된 메소드는 최종적으로 객체에서 실행
    * 그렇기 때문에 인터페이스의 메소드는 실행 블록이 필요 없는 추상 메소드로 선언
  * 인터페이스에 선언된 추상 메소드는 모두 `public abstract` 특성
  * 구현 객체가 인터페이스 타입에 대입되면, 인터페이스에 선언된 추상 메소드를 개발 코드에서 호출 가능

  ```JAVA
  InterfaceEx interfaceEx = new Concrete();
  interfaceEx.exMethod();
  ```

* 디폴트 메소드

  * 디폴트 메소드는 인터페이스에 선언되지만, 사실은 구현 객체가 가지고 있는 **인스턴스 메소드**
    * 인터페이스에 선언되지만, 인터페이스에서 바로 사용할 수 없음
    * 디폴트 메소드는 인스턴스 메소드이므로 구현 객체가 있어야 함
  * 자바 8에서 기존 인터페이스를 확장해서 새로운 기능을 추가하기 위해 디폴트 메소드 허용함
  * 클래스의 인스턴스 메소드와 동일하지만, default 키워드가 리턴 타입 앞에 붙음
  * 디폴트 메소드는 `public` 특성
  * 인터페이스의 모든 구현 객체가 가지고 있는 기본 메소드라고 생각하면 됨
  * 구현 클래스 작성 시 디폴트 메소드를 재정의해서 수정하면 재정의한 메소드 호출

* 정적 메소드

  * 디폴트 메소드와 달리 객체가 없어도 인터페이스만으로 호출 가능
  * 자바 8부터 지원
  * 형태는 클래스의 정적 메소드와 완전 동일
  * 정적 메소드는 `public` 특성



### 구현 객체와 구현 클래스

* 구현 객체

  * 실체 메소드를 가진 객체
  * 실체 메소드란 인터페이스에서 정의된 추상 메소드와 동일한 메소드명, 타입, 리턴값을 가진 메소드

* 구현 클래스

  * 구현 객체를 생성하는 클래스
  * 보통의 클래스와 동일하지만, `implements` 키워드 사용하여 인터페이스명 명시
  * 인터페이스에 선언된 추상 메소드의 실체 메소드 선언 필요
  * 인터페이스의 모든 메소드는 **public** 접근 제한
    * 즉, 실체 메소드 작성 시 public 보다 낮은 접근 제한으로 작성 불가
    * public 생략 시 컴파일 에러 발생
  * 구현 클래스가 실체 메소드를 작성하지 않으면, 구현 클래스는 **자동적으로 추상 클래스**가 됨
    * 선언부에 abstract 키워드 추가 필요

  ```java
  public class Concrete implements InterfaceEx{
      //인터페이스에 선언된 추상 메소드의 실체 메소드 선언
  }
  ```

  

### 익명 구현 객체 (Anonymouse Class)

* 일회성의 구현 객체를 만들기 위해 구현 클래스를 만들어 사용하는 것은 비효율적
  * 보통은 클래스 재사용을 위해 구현 클래스를 만들어 사용함
  * 이를 대비하기 위해 자바에서는 익명 구현 객체를 제공함
* UI 프로그래밍에서 이벤트 처리를 위해, 임시 작업 스레드를 만들기 위해 익명 구현 객체 활용
* 자바 8에서 지원하는 람다식은 인터페이스의 익명 구현 객체를 만듦
* 익명 구현 객체에 추가적으로 필드와 메소드를 선언할 수 있지만, 익명 객체 안에서만 사용할 수 있고 인터페이스 변수로 접근 불가
* 익명 구현 객체 작성 시 주의할 점
  * 하나의 실행문이므로 끝에는 세미콜론(;)을 반드시 붙여야 함

```java
인터페이스 변수 = new 인터페이스(){
    //인터페이스에 선언된 추상 메소드의 실제 메소드 선언
}
```

```java
public class AnonymouseEx{
    public static void main(String[] args){
        InterfaceEx interfaceEx = new InterfaceEx(){
            public void funcA(){ /* 실행문 */ }
            public void funcB(){ /* 실행문 */ }
            public void funcC(int num){ /* 실행문 */ }
        };
    }
}
```



### 다중 인터페이스 구현 클래스

* 객체는 다수의 인터페이스 타입으로 사용 가능
* 다중 인터페이스 구현한 경우, 구현 클래스는 모든 인터페이스와 추상 메소드에 대해 실체 메소드 작성 필요
* 만약, 하나라도 없으면 추상 클래스로 선언해야 함

```java
public class Concrete implements InterfaceEx1, InterfaceEx2{
    //InterfaceEx1에 선언된 추상 메소드의 실체 메소드 선언
    //InterfaceEx2에 선언된 추상 메소드의 실체 메소드 선언
}
```



### 타입 변환과 다형성

* 요즘엔 상속보단 인터페이스를 통해 다형성 구현하는 경우가 많음

* 프로그램 소스 코드는 변함 없이 **구현 객체를 교체**함으로써 다형성 구현

* 상속과 인터페이스의 개념적 차이

  * 상속은 같은 종류의 하위 클래스를 만드는 기술
  * 인터페이스는 사용 방법이 동일한 클래스를 만드는 기술

* EX) A 클래스를 이용해서 프로그램 개발한다고 가정

  * 개발 완료 후 A 클래스를 B 클래스로 바꾸기로 함
  * 하지만 B 클래스의 메소드 선언부는 A 클래스의 메소드 선언부와 다름
  * 어쩔 수 없이 A 클래스의 메소드가 사용된 곳을 찾아 B 클래스의 메소드로 변경해야 함
  * 만약, A 클래스와 B 클래스의 메소드 선언부가 동일하다면?
    * 메소드 호출 방법이 동일하므로 메소드 호출 코드는 수정할 필요 없이 객체 생성 부분만 A에서 B로 바꾸면 됨
  * 문제는 A, B 클래스를 설계할 때 메소드 선언부를 완전히 동일하게 설계할 수 있는지?
    * 인터페이스를 작성하고 A, B 클래스는 구현 클래스로 작성하면 됨

  ```JAVA
  public interface InterfaceEx {
  	public void funcA();
  	public void funcB();
  }
  ```

  ```java
  public class AClass implements InterfaceEx{
  	@Override
  	public void funcA() {
  		System.out.println("A 클래스의 funcA");
  	}
  	@Override
  	public void funcB() {
  		System.out.println("A 클래스의 funcB");
  	}
  }
  ```

  ```java
  public class BClass implements InterfaceEx{
  	@Override
  	public void funcA() {
  		System.out.println("B 클래스의 funcA");
  	}
  	@Override
  	public void funcB() {
  		System.out.println("B 클래스의 funcB");
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		//기존 코드
  		InterfaceEx interfaceEx = new AClass();
  		interfaceEx.funcA();
  		interfaceEx.funcB();	
  	}
  }
  ```

  ```text
  A 클래스의 funcA
  A 클래스의 funcB
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		//A 클래스를 B 클래스로 교체
  		InterfaceEx interfaceEx = new BClass();
  		interfaceEx.funcA();
  		interfaceEx.funcB();
  	}
  }
  ```

  ```text
  B 클래스의 funcA
  B 클래스의 funcB
  ```

* 자동 타입 변환 (Promotion)

  * 구현 객체가 인터페이스 타입으로 변환되는 것
  * 인터페이스 구현 클래스를 상속해서 자식 클래스 생성 시,
    * 자식 객체 역시 인터페이스 타입으로 자동 타입 변환 가능
  * 자동 타입 변환 이용 시 필드의 다형성과 매개 변수의 다형성 구현 가능

* 필드의 다형성

  * EX) 자동차 설계
    * 상속에서는 타이어 클래스 타입에 Black 타이어와 Red 타이어라는 자식 객체를 대입해서 교체 가능
    * 인터페이스에서는 타이어가 인터페이스 타입, Black 타이어와 Red 타이어는 구현 클래스

* 매개 변수의 다형성

  * 상속에서는 매개 변수를 부모 타입으로 선언하고 호출 시 자식 객체를 대입
  * 인터페이스에서는 매개 변수를 인터페이스 타입으로 선언하고 호출 시 구현 객체 대입

  ```java
  public interface Vehicle {
  	public void run();
  }
  ```

  ```java
  public class Driver {
  	public void drive(Vehicle vehicle) {
  		//vehicle == 구현 객체
  		//run() == 구현 객체의 run() 메소드 실행
  		vehicle.run();
  	}
  }
  ```

  ```java
  public class Bus implements Vehicle{
  	@Override
  	public void run() {
  		System.out.println("버스가 달립니다");
  	}
  }
  ```

  ```java
  public class Taxi implements Vehicle{
  	@Override
  	public void run() {
  		System.out.println("택시가 달립니다");
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		Driver driver = new Driver();
  		
  		Bus bus = new Bus();
  		Taxi taxi = new Taxi();
  		
  		driver.drive(bus);
  		driver.drive(taxi);
  	}
  }
  ```

* 강제 타입 변환 (Casting)

  * 구현 객체가 인터페이스 타입으로 자동 변환 시, 인터페이스에 선언된 메소드만 사용 가능함
  * 구현 클래스에 선언된 필드와 메소드 사용해야 할 경우 발생 시
    * 강제 타입 변환을 해서 다시 구현 클래스 타입으로 변환한 다음, 구현 클래스의 필드와 메소드 사용 가능
  * 객체 타입 확인
    * instanceof



### 인터페이스 상속

* 인터페이스도 다른 인터페이스 상속 가능
* 다중 상속 가능
* 하위 인터페이스와 상위 인터페이스의 모든 추상 메소드에 대한 실체 메소드 가지고 있어야 함
  * 즉, 구현 클래스로부터 객체를 생성하고 나서 하위 및 상위 인터페이스 타입으로 변환 가능
  * 상위 인터페이스로 타입 변환 시, 하위 인터페이스에 선언된 메소드 사용 불가



### 디폴트 메소드와 인터페이스 확장

* 선언은 인터페이스에서 하고, 사용은 구현 객체를 통해 한다.
* 디폴트 메소드의 필요성
  * 기존 인터페이스를 확장해서 새로운 기능을 추가하기 위함
  * 기존 인터페이스의 이름과 추상 메소드 변경 없이 디폴트 메소드만 추가 가능
  * 즉, 이전에 개발한 구현 클래스를 그대로 사용할 수 있으면서 새로운 클래스 활용 가능
* 디폴트 메소드는 **추상 메소드가 아니기 때문에 구현 클래스에서 실체 메소드 작성 필요 없음**

```java
public interface InterfaceEx {
	public void funcA();
	
	public default void defaultMethod() {
		System.out.println("InterfaceEx 디폴트 메소드");
	}
}
```

```java
public class OldClass implements InterfaceEx{
	@Override
	public void funcA() {
		System.out.println("기존 클래스 funcA");
	}
}
```

```java
public class NewClass implements InterfaceEx{
	@Override
	public void funcA() {
		System.out.println("새로운 클래스 funcA");
	}
	@Override
	public void defaultMethod() {
		System.out.println("새로운 클래스 디폴트 메소드 재정의");
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		
		InterfaceEx interfaceEx1 = new OldClass();
		interfaceEx1.funcA();
		interfaceEx1.defaultMethod();
		
		System.out.println("---------------------");
		
		InterfaceEx interfaceEx2 = new NewClass();
		interfaceEx2.funcA();
		interfaceEx2.defaultMethod();
	}
}
```

```text
기존 클래스 funcA
InterfaceEx 디폴트 메소드
---------------------
새로운 클래스 funcA
새로운 클래스 디폴트 메소드 재정의
```

* 디폴트 메소드가 있는 인터페이스 상속

  * 부모 인터페이스에 디폴트 메소드가 정의되어 있을 경우, 자식 인터페이스에서 이를 활용하는 방법

  ```java
  public interface ParentInterface {
  	public void parentMethod();
  	public default void defaultMethod() {
  		System.out.println("부모 인터페이스의 디폴트 메소드");
  	}
  }
  ```

  ```java
  public interface ChildInterface extends ParentInterface{
  	public void childMethod();
  }
  ```

  * `1` 디폴트 메소드를 단순히 상속만 받음
    * 구현 클래스는 부모와 자식 인터페이스의 실체 메소드를 가지고 있어야 함
    * 구현 클래스는 부모 인터페이스의 디폴트 메소드 호출 가능

  ```java
  public class Concrete implements ChildInterface{
  	@Override
  	public void parentMethod() {
  		System.out.println("구현 클래스의 parentMethod");
  	}
  	@Override
  	public void childMethod() {
  		System.out.println("구현 클래스의 childMethod");
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		ChildInterface childInterface = new Concrete();
  		
  		childInterface.childMethod();
  		childInterface.defaultMethod();
  		childInterface.parentMethod();
  	}
  }
  ```

  ```text
  구현 클래스의 childMethod
  부모 인터페이스의 디폴트 메소드
  구현 클래스의 parentMethod
  ```

  * `2` 디폴트 메소드를 재정의해서 실행 내용 변경
    * 자식 인터페이스는 부모 인터페이스의 디폴트 메소드를 재정의

  ```java
  public interface ChildInterface extends ParentInterface{
  	@Override
  	default void defaultMethod() {
  		System.out.println("자식 인터페이스에서 재정의한 부모 인터페이스의 디폴트 메소드");
  	}
  	public void childMethod();
  }
  ```

  ```text
  구현 클래스의 childMethod
  자식 인터페이스에서 재정의한 부모 인터페이스의 디폴트 메소드
  구현 클래스의 parentMethod
  ```

  * `3` 디폴트 메소드를 추상 메소드로 재선언
    * 부모 인터페이스의 디폴트 메소드를 추상 메소드로 재선언
    * 자식 인터페이스를 구현하는 구현 클래스는 모든 실체 메소드를 가지고 있어야 함

  ```java
  public interface ChildInterface extends ParentInterface{
  	public void defaultMethod();
  	public void childMethod();
  }
  ```

  ```java
  public class Concrete implements ChildInterface{
  	@Override
  	public void parentMethod() {
  		System.out.println("구현 클래스의 parentMethod");
  	}
  	@Override
  	public void childMethod() {
  		System.out.println("구현 클래스의 childMethod");
  	}
  	@Override
  	public void defaultMethod() {
  		System.out.println("구현 클래스의 디폴트 메소드");
  	}
  }
  ```

  ```text
  구현 클래스의 childMethod
  구현 클래스의 디폴트 메소드
  구현 클래스의 parentMethod
  ```

  

