# 상속

### 목차

* 상속
* 클래스 상속
* 부모 생성자 호출
* 메소드 재정의 (@Override, 오버라이딩)
* final 클래스와 final 메소드
* protected 접근 제한자
* 타입 변환과 다형성
* 추상 클래스 (Abstract)



### 상속

* 기존에 정의된 클래스를 상속 받아 기능을 추가하거나 재정의하여 새로운 클래스를 정의하는 것
  * 기존에 정의된 클래스를 **부모/상위 클래스**라 하고, 새로운 클래스를 **자식/하위 클래스**라 함
* `extends` 키워드 사용
* Java는 다중 상속을 지원하지 않음
* 장점
  * 코드 중복 감소
  * 개발 시간 절약
  * 효율적인 유지보수 가능
* 상속 제외 대상
  * 부모 클래스에서 **private** 접근 제한자를 가지는 필드와 메소드
  * 부모 클래스와 자식 클래스의 패키지가 다를 때, **default** 접근 제한자를 가지는 필드와 메소드
  * 부모 클래스의 생성자와 초기화 블록



### 클래스 상속

* 부모 클래스가 상속할 자식 클래스를 선택하는 것이 아닌, **자식 클래스가 상속 받을 부모 클래스를 선택**
  * 즉, 자식 클래스를 생성하려면 부모 클래스가 먼저 생성되어야 함

```java
public class Parent {
	public void outParent(){
		System.out.println("부모 클래스");
	}
}
```

```java
public class ChildA extends Parent{
	
}
```

```java
public class ChildB extends Parent{
	public void outChild() {
		System.out.println("자식 클래스 B");
	}
}
```

```java
public class Main {
	public static void main(String[] args) {	
		ChildA childA = new ChildA();
		ChildB childB = new ChildB();
		
		childA.outParent();
		
		childB.outParent();
		childB.outChild();
	}
}
```

```text
부모 클래스
부모 클래스
자식 클래스 B
```



### 부모 생성자 호출

* 자식 클래스를 생성하면, 부모 클래스가 먼저 생성되고 자식 클래스가 생성됨

* 부모 클래스의 생성자는 자식 클래스 생성자의 **맨 첫 줄**에서 호출됨

  * 생성자가 명시적으로 선언되지 않은 경우, 컴파일러가 기본 생성자 생성

* 만약, 부모 클래스에 기본 생성자가 없고, 매개변수가 있는 생성자만 있는 경우, 자식 클래스 생성자에서 **반드시** 부모 생성자 호출을 위해 `super(매개값)`를 명시적으로 호출해야 함

  * super(매개값)는 반드시 자식 생성자 첫 줄에 위치

  ```java
  public class People{
      public String name;
      public String ssn;
      
      public People(String name, String ssn){
          this.name = name;
          this.ssn = ssn;
      }
  }
  ```

  ```java
  public class Student extends People{
      public int studentNum;
      
      public Student(String name, String ssn, int studentNum){
          super(name, ssn); //부모 생성자 호출
          this.studentNum = studentNum;
      }
  }
  ```

  ```java
  public class StudentEx{
      public static void main(String[] args){
          
          Student student = new Student("홍길동", "123456-1234567", 7);
          
          //부모에게 상속받은 필드 출력
          System.out.println("name : "+student.name);
          System.out.println("ssn : "+student.ssn);
          
          //자식이 가진 필드 출력
          System.out.println("num : "+student.studentNum);
          
      }
  }
  ```

  ```text
  name : 홍길동
  ssn : 123456-1234567
  num : 7
  ```



### 메소드 재정의 (@Override, 오버라이딩)

* 상속된 메소드 내용이 자식 클래스에 맞지 않을 경우, 자식 클래스에서 동일한 메소드를 재정의하는 것

* 메소드 오버라이딩을 한 경우, 부모 클래스의 메소드는 숨겨짐

  * 즉, 자식 클래스에서 메소드 호출 시 오버라이딩된 자식 메소드 호출됨

* 주의 사항

  * 부모의 메소드와 동일한 시그너처(리턴 타입, 메소드명, 매개변수 리스트)를 가져야 함
  * 접근 제한을 더 강하게 오버라이딩 할 수 없음
    * 반대의 경우는 가능
    * EX) 부모 메소드가 default인 경우, 자식 메소드는 default 또는 public 가능
  * 새로운 예외(Exception)를 throws 할 수 없음

* 부모 메소드 호출(super)

  * 자식 클래스에서 오버라이딩된 부모 클래스의 메소드를 호출해야 할 때, 명시적으로 super 키워드 사용하여 부모 메소드 호출 가능
  * super는 부모 객체를 참조하고 있기 때문에 부모 메소드에 직접 접근할 수 있음

  ```java
  public class Airplane {
  	public void land() { System.out.println("착륙"); }	
  	public void fly() { System.out.println("일반 비행"); }
  	public void takeOff() { System.out.println("이륙"); }
  }
  ```

  ```java
  public class SupersonicAirplane extends Airplane{
  	public static final int NORMAL = 1;
  	public static final int SUPERSONIC = 2;
  	
  	public int flyMode = NORMAL;
  	
  	@Override
  	public void fly() {
  		if(flyMode == SUPERSONIC) System.out.println("초음속 비행");
  		else super.fly(); //부모 클래스의 fly() 메소드 호출
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		
  		SupersonicAirplane supersonicAirplane = new SupersonicAirplane();
  		
  		supersonicAirplane.takeOff();
  		supersonicAirplane.fly();
  		
  		supersonicAirplane.flyMode = SupersonicAirplane.SUPERSONIC;
  		supersonicAirplane.fly();
  		
  		supersonicAirplane.flyMode = SupersonicAirplane.NORMAL;
  		supersonicAirplane.fly();
  		
  		supersonicAirplane.land();
  		
  	}
  }
  ```

  ```text
  착륙
  일반 비행
  초음속 비행
  일반 비행
  이륙
  ```

  

### final 클래스와 final 메소드

* final 클래스는 부모 클래스가 될 수 없어 자식 클래스를 만들 수 없음
* 부모 클래스에 선언된 final 메소드는 자식 클래스에서 재정의할 수 없음
* final
  * 해당 키워드를 클래스 또는 메소드에 붙이면 **최종적인** 클래스/메소드이므로 상속할 수 없음



### protected 접근 제한자

* protected는 public과 default 접근 제한의 중간쯤 해당
  * 즉, 같은 패키지에서는 default와 같이 접근 제한이 없음
  * 다른 패키지에서는 자식 클래스만 접근 허용



### 타입 변환과 다형성

* 다형성

  * 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질
  * 즉, 다형성은 하나의 타입에 여러 객체를 대입함으로써 다양한 기능을 이용할 수 있도록 해주는 성질
  * 자바에서는 다형성을 위해 부모 클래스로 타입 변환을 허용함
    * 즉, 부모 타입에 모든 자식 객체가 대입될 수 있음
    * 객체 부품화 가능
    * EX) 자동차를 설계할 때 타이어 클래스 타입을 적용했다면, 이 클래스를 상속한 실제 타이어들은 어떤 것이든 상관없이 장착(대입) 가능

* 타입 변환

  * 데이터 타입을 다른 데이터 타입으로 변환하는 행위
  * 클래스 타입의 변환은 상속 관계에 있는 클래스 사이에서 발생
  * 자식 타입은 부모 타입으로 자동 타입 변환 가능

* 자동 타입 변환(Promotion)

  * 프로그램 실행 도중에 자동적으로 타입 변환이 일어나는 것
  * 자동 타입 변환이 일어나는 조건
    * `Parent parent = new Child();`
  * 자식은 부모의 특징과 기능을 상속받기 때문에 부모와 동일하게 취급될 수 있음
  * 상속 계층에서 상위 타입이라면 자동 타입 변환 가능
    * 바로 위의 부모가 아니어도 된다는 뜻
  * 부모 타입으로 자동 타입 변환된 이후
    * 부모 클래스에 선언된 필드와 메소드만 접근 가능
    * 변수는 자식 객체를 참조하지 않지만, 변수로 접근 가능한 멤버는 부모 클래스 멤버로만 한정
    * *예외*
      * 메소드가 자식 클래스에서 오버라이딩 되었다면, 자식 클래스의 메소드가 대신 호출

  ```java
  public class Parent {
  	public void parentMethod1() { System.out.println("부모 클래스 메소드 1"); }
  	public void parentMethod2() { System.out.println("부모 클래스 메소드 2"); }
  }
  ```

  ```java
  public class Child extends Parent{
  	@Override
  	public void parentMethod2() {
  		System.out.println("부모 클래스 메소드 2 --> 자식 클래스에서 재정의");
  	}
  	public void childMethod() {
  		System.out.println("자식 클래스 메소드");
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		//자동 타입 변환
  		Parent parent = new Child();
  		
  		parent.parentMethod1();
  		parent.parentMethod2();
  		//parent.childMethod() 호출 불가능
  	}
  }
  ```

  ```text
  부모 클래스 메소드 1
  부모 클래스 메소드 2 --> 자식 클래스에서 재정의
  ```

* 필드의 다형성

  * 필드 타입은 변함 없음
  * 프로그램 실행 도중 **어떤 객체를 필드로 저장하느냐**에 따라 실행 결과 달라질 수 있음
  * EX) 자동차 클래스에 포함된 타이어 클래스
    * 자동차 클래스 설계 시 사용한 타이어 객체는 언제든 성능 좋은 다른 타이어 객체로 교체할 수 있어야 함
    * 새로 교체되는 타이어 객체는 기존 타이어와 사용 방법은 동일하지만,
    * 실행 결과는 더 우수하게 나와야 할 것
    * 이를 프로그램으로 구현하기 위해서는 상속과 오버라이딩, 타입 변환 이용
      * 부모 클래스를 상속하는 자식 클래스는 부모가 가지고 있는 필드와 메소드를 가지고 있으니 사용 방법 동일
      * 자식 클래스는 부모 메소드 재정의를 통해 메소드의 실행 내용을 변경하여 더 우수한 실행 결과가 나오게 할 수 있음
      * 자식 타입을 부모 타입으로 변환 가능

  ```java
  public class Tire {
  	
  	//필드
  	public int maxRotation; //최대 회전수(타이어 수명)
  	public int accumulatedRotation; //누적 회전수
  	public String location; //타이어 위치
  	
  	//생성자
  	public Tire(String location, int maxRotation) {
  		//초기화
  		this.location = location;
  		this.maxRotation = maxRotation;
  	}
  	
  	//메소드
  	public boolean roll() {
  		++accumulatedRotation; //누적 회전수 +1
  		if(accumulatedRotation < maxRotation) {
  			System.out.println(location + " 타이어 수명 : " + (maxRotation - accumulatedRotation) + " 회");
  			return true;
  		}else {
  			System.out.println("****** "+location + " 타이어 펑크 !");
  			return false;
  		}
  	}
  }
  ```

  ```java
  public class Car {
  	
  	//필드
  	Tire[] tires = {
  			new Tire("앞왼쪽", 6),
  			new Tire("앞오른쪽", 2),
  			new Tire("뒤왼쪽", 3),
  			new Tire("뒤오른쪽", 4)
  	};
  	
  	int run() {
  		System.out.println("자동차가 달립니다.");
  		
  		for (int i = 0; i < tires.length; i++) {
  			if(tires[i].roll()==false) {
  				stop();
  				return (i+1);
  			}
  		}
  		return 0;
  	}
  	
  	void stop() {
  		System.out.println("자동차가 멈춥니다.");
  	}
  }
  ```

  ```java
  public class BlackTire extends Tire{
  
  	public BlackTire(String location, int maxRotation) {
  		super(location, maxRotation);
  	}
  	
  	@Override
  	public boolean roll() {
  		//출력 내용을 달리하기 위해 재정의한 roll() 메소드
  		++accumulatedRotation;
  		if(accumulatedRotation < maxRotation) {
  			System.out.println(location + " Black 타이어 수명 : "+(maxRotation-accumulatedRotation)+" 회");
  			return true;
  		}else {
  			System.out.println("****** "+location + " 타이어 펑크 !");
  			return false;
  		}
  	}
  }
  ```

  ```java
  public class RedTire extends Tire{
  
  	public RedTire(String location, int maxRotation) {
  		super(location, maxRotation);
  	}
  	
  	@Override
  	public boolean roll() {
  		//출력 내용을 달리하기 위해 재정의한 roll() 메소드
  		++accumulatedRotation;
  		if(accumulatedRotation < maxRotation) {
  			System.out.println(location + " Red 타이어 수명 : "+(maxRotation-accumulatedRotation)+" 회");
  			return true;
  		}else {
  			System.out.println("****** "+location + " 타이어 펑크 !");
  			return false;
  		}
  	}
  }
  ```

  ```java
  public class Main {
  	
  	public static void main(String[] args) {
  		
  		Car car = new Car();
  		
  		for (int i = 1; i<=5; i++) {
  			int problemLocation = car.run();
  			if(problemLocation != 0) {
  				System.out.println(car.tires[problemLocation-1].location+" Red 타이어로 교체");
  				car.tires[problemLocation-1] = new RedTire(car.tires[problemLocation-1].location, 15);
  			}
  			System.out.println("================================");
  		}
  	}
  }
  ```

  ```text
  자동차가 달립니다.
  앞왼쪽 타이어 수명 : 5 회
  앞오른쪽 타이어 수명 : 1 회
  뒤왼쪽 타이어 수명 : 2 회
  뒤오른쪽 타이어 수명 : 3 회
  ================================
  자동차가 달립니다.
  앞왼쪽 타이어 수명 : 4 회
  ****** 앞오른쪽 타이어 펑크 !
  자동차가 멈춥니다.
  앞오른쪽 Red 타이어로 교체
  ================================
  자동차가 달립니다.
  앞왼쪽 타이어 수명 : 3 회
  앞오른쪽 Red 타이어 수명 : 14 회
  뒤왼쪽 타이어 수명 : 1 회
  뒤오른쪽 타이어 수명 : 2 회
  ================================
  자동차가 달립니다.
  앞왼쪽 타이어 수명 : 2 회
  앞오른쪽 Red 타이어 수명 : 13 회
  ****** 뒤왼쪽 타이어 펑크 !
  자동차가 멈춥니다.
  뒤왼쪽 Red 타이어로 교체
  ================================
  자동차가 달립니다.
  앞왼쪽 타이어 수명 : 1 회
  앞오른쪽 Red 타이어 수명 : 12 회
  뒤왼쪽 Red 타이어 수명 : 14 회
  뒤오른쪽 타이어 수명 : 1 회
  ================================
  ```

* 매개 변수의 다형성

  * 매개값으로 **어떤 자식 객체가 제공되느냐**에 따라 메소드의 실행 결과는 다양해질 수 있음
  * 자동 타입 변환은 주로 메소드 호출 시 많이 발생함
  * 매개 변수 타입이 클래스일 경우, 해당 클래스의 객체뿐 아니라 **자식 객체까지도** 매개값으로 사용 가능
  * 자식 객체가 부모 메소드를 재정의했다면, 메소드 내부에서 오버라이딩된 메소드를 호출함으로써 메소드 실행 결과 다양해짐

  ```java
  public class Vehicle {
  	//부모 클래스
  	public void run() {
  		System.out.println("차량이 달립니다.");
  	}
  }
  ```

  ```java
  public class Driver {
  	//Vehicle을 이용하는 클래스
  	//Vehicle 타입의 매개값을 받아서 run() 메소드 실행
  	public void drive(Vehicle vehicle) {
  		vehicle.run();
  	}
  }
  ```

  ```java
  public class Bus extends Vehicle{
  	@Override
  	public void run() {
  		System.out.println("버스가 달립니다");
  	}
  }
  ```

  ```java
  public class Taxi extends Vehicle{
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
  		
  		//자동 타입 변환 Vehicle vehicle = bus;
  		driver.drive(bus);
  		
  		//자동 타입 변환 Vehicle vehicle = taxi;
  		driver.drive(taxi);
  	}
  }
  ```

  ```text
  버스가 달립니다
  택시가 달립니다
  ```

* 강제 타입 변환(Casting)

  * 부모 타입을 자식 타입으로 변환하는 것
    * 모든 부모 타입을 자식 클래스 타입으로 강제 변환할 수 있는 건 아님
    * 자식 타입이 부모 타입으로 자동 변환한 후, 다시 자식 타입으로 변환할 때 강제 타입 변환 사용 가능
  * 자식 타입이 부모 타입으로 자동 변환하면
    * 부모 타입에 선언된 필드와 메소드만 사용 가능함
    * 혹, 자식 타입에 선언된 필드와 메소드를 꼭 사용해야 한다면!
      * 강제 타입 변환을 해서 다시 자식 타입으로 변환한 다음 자식 타입의 필드와 메소드를 사용하면 됨
  * 객체 타입 확인
    * instanceof



### 추상 클래스 (Abstract)

* 사전적 의미
  * 실체 간에 공통되는 특성을 추출한 것
  * 구체적인 실체라기보단 실체들의 공통되는 특성을 가지고 있는 추상적인 것

* 객체를 직접 생성할 수 있는 **실체 클래스의 공통적인 특성을 추출**해서 선언한 클래스
* 추상 클래스와 실체 클래스는 상속 관계
  * 부모 : 추상 클래스 / 자식 : 실체 클래스
  * 실체 클래스는 추상 클래스의 필드와 메소드를 상속받고, 추가적인 필드와 메소드를 가질 수 있음
* `abstract` 키워드 사용하여 선언
* 추상 클래스의 용도
  * 실체 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적
    * 동일한 데이터와 기능임에도 이름이 달라 객체마다 사용 방법이 달라지는 것을 방지
  * 실체 클래스 작성 시 시간 절약
    * 공통적인 필드와 메소드는 추상 클래스에 선언
    * 다른 점만 실체 클래스에 선언
* 추상 클래스의 특징
  * 실체 클래스의 공통 필드와 메소드를 추출해 생성했기 때문에 **직접 객체 생성 불가**
  * 즉, new 연산자를 사용해서 인스턴스 생성 불가 (부모 클래스로만 사용)
* 추상 메소드와 오버라이딩
  * 추상 클래스는 추상 메소드 선언 가능
    * 메소드의 선언부만 있고, 메소드 실행 내용인 중괄호{ }가 없는 메소드
    * 추상 메소드는 추상 클래스에서만 선언 가능
  * 메소드의 선언만 통일화하고 실행 내용은 실체 클래스마다 달라야 하는 경우 사용
    * 실체 클래스들이 가지는 메소드 실행 내용이 동일하다면 추상 클래스에 메소드 작성
  * 추상 클래스 설계 시, 하위 클래스가 **반드시 실행 내용을 채우도록 강요**하고 싶은 메소드가 있는 경우, 해당 메소드를 추상 메소드로 선언
    * 하위 클래스는 반드시 추상 메소드를 **재정의**해서 실행 내용을 작성해야 함

```java
public abstract class AbstAnimal {
	
	public String kind;
	
	public void breathe() {
		System.out.println("숨쉬기");
	}
	
	//추상 메소드
	public abstract void sound();
}
```

```java
public class Dog extends AbstAnimal{
	
	public Dog() {
		this.name = "강아지";
	}

	//추상 메소드 재정의
	@Override
	public void sound() {
		System.out.println("멍멍");
	}
}
```

```java
public class Cat extends AbstAnimal{

	public Cat() {
		this.name = "고양이";
	}
	
	//추상 메소드 재정의
	@Override
	public void sound() {
		System.out.println("야옹");
	}
}
```

```java
public class Main {
	
	public static void main(String[] args) {
		
		Dog dog = new Dog();
		Cat cat = new Cat();
		
		dog.sound();
		cat.sound();
		System.out.println("----------------");
		
		//변수의 자동 타입 변환
		AbstAnimal abstAnimal = new Dog();
		abstAnimal.sound();
		
		abstAnimal = new Cat();
		abstAnimal.sound();
		System.out.println("----------------");
		
		//메소드의 다형성
		animalSound(new Dog());
		animalSound(new Cat());
	}

	public static void animalSound(AbstAnimal abstAnimal) {
		abstAnimal.sound();
	}

}
```

```text
멍멍
야옹
----------------
멍멍
야옹
----------------
멍멍
야옹
```



## Reference

* **신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)**

