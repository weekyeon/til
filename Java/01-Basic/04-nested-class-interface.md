# 중첩 클래스와 중첩 인터페이스

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 개념
* 중첩 클래스 (Nested Class)
* 중첩 인터페이스 (Nested Interface)
* 익명 객체 (Anonymous)



### 개념

* 중첩 클래스

  * 클래스 내부에 선언한 클래스

  * 두 클래스의 멤버들을 서로 쉽게 접근할 수 있음

  * 외부에 불필요한 관계 클래스를 감춰 코드의 복잡성 감소

  * 어떤 클래스가 특정 클래스와 관계를 맺을 경우, 관계 클래스를 클래스 내부에 선언하는 것이 좋음

    * 어떤 클래스가 여러 클래스와 관계를 맺는다면, 독립적인 클래스 선언이 좋음

    

  ```java
  class ClassName{ //바깥 클래스
      class NestedClassName{
          //중첩 클래스
      }
  }
  ```

  

* 중첩 인터페이스

  * 클래스 내부에 선언한 인터페이스
  * 해당 클래스와 긴밀한 관계를 맺는 구현 클래스를 만들기 위해 선언함

  

  ```java
  class ClassName{
      interface NestedInterfaceName{
          //중첩 인터페이스
      }
  }
  ```

  

### 중첩 클래스 (Nested Class)

* 클래스 내부에 선언한 클래스
* 선언되는 위치에 따라 `1` 멤버 클래스, `2` 로컬 클래스로 분류
* 멤버 클래스
  * 클래스의 멤버로서 선언되는 중첩 클래스
  * 클래스나 객체가 사용 중이라면 언제든 재사용 가능
* 인스턴스 멤버 클래스
  * static 키워드 없이 선언된 클래스
  * 인스턴스 필드와 메소드만 선언 가능
  * A 클래스 외부에서 인스턴스 멤버 클래스 B 객체를 생성하려면 A 객체 생성 후 B 객체 생성

* 정적 멤버 클래스
  * static 키워드로 선언된 클래스
  * 모든 종류의 필드와 메소드 선언 가능
  * A 클래스 외부에서 정적 멤버 클래스 C의 객체를 생성하기 위해서는 A 객체를 생성할 필요가 없음
* 로컬 클래스
  * 메소드 내부에서 선언되는 중첩 클래스
  * 메소드 실행 시에만 사용 가능, 메소드 실행 종료 시 소멸
  * 접근을 제한할 필요가 없어 접근 제한자(public, private) 및 static을 붙일 수 없음
  * 로컬 클래스 내부에는 인스턴스 필드와 메소드만 선언 가능
  * 정적 필드와 메소드 선언 불가능
  * 메소드가 실행될 때 메소드 내에서 객체를 생성하고 사용해야 함
  * 비동기 처리를 위해 스레드 객체 만들 때 주로 사용

* 예제

```java
public class NestedClass {
	
	// 바깥 클래스
	public NestedClass() {
		System.out.println("----- NestedClass 객체 생성");
	}
	
	//인스턴스 멤버 클래스
	class instanceMemClass{
		public instanceMemClass() { System.out.println("인스턴스 멤버 클래스 객체 생성"); }
		int field1;
		void instanceMethod() { }
	}
	
	//정적 멤버 클래스
	static class staticMemClass{
		public staticMemClass() { System.out.println("정적 멤버 클래스 객체 생성"); }
		int field1;
		static int field2;
		void instanceMethod() { }
		static void staticMethod() { }
	}
	
	void method() {
		//로컬 클래스
		class LocalClass {
			public LocalClass() { System.out.println("로컬 클래스 객체 생성"); }
			int field1;
			void localMethod() { }
		}
		//메소드 실행 시 메소드 내에서 객체 생성&사용
		LocalClass localClass = new LocalClass();
		localClass.field1 = 3;
		localClass.localMethod();
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		// 바깥 클래스 객체 생성
		NestedClass nestedClass = new NestedClass();
		
		// 인스턴스 멤버 클래스 객체 생성
		NestedClass.instanceMemClass instance = nestedClass.new instanceMemClass();
		instance.field1 = 10;
		instance.instanceMethod();
		
		// 정적 멤버 클래스 객체 생성
		NestedClass.staticMemClass statics = new NestedClass.staticMemClass();
		statics.field1 = 5;
		statics.instanceMethod();
		NestedClass.staticMemClass.field2 = 3;
		NestedClass.staticMemClass.staticMethod();

		// 로컬 클래스 객체 생성을 위한 메소드 호출
		nestedClass.method();
	}
}
```

```text
----- NestedClass 객체 생성
인스턴스 멤버 클래스 객체 생성
정적 멤버 클래스 객체 생성
로컬 클래스 객체 생성
```



* 중첩 클래스의 접근 제한

  * 바깥 필드와 메소드에서 사용 제한

    * 멤버 클래스가 인스턴스 또는 정적으로 선언됨에 따라 **바깥 클래스의 필드와 메소드 사용 제한**이 생김
    * 바깥 클래스의 인스턴스 필드 초기값이나 인스턴스 메소드에서는 객체 생성 **가능**
    * 바깥 클래스의 정적 필드의 초기값이나 정적 메소드에서는 객체 생성 **불가능**
    * 정적 멤버 클래스는 모든 필드의 초기값이나 모든 메소드에서 객체 생성 **가능**

    

  ```java
  public class A {
  
  	// 바깥 클래스의 인스턴스 필드
  	B field1 = new B();
  	C field2 = new C();
  	
  	// 바깥 클래스의 인스턴스 메소드
  	void method1() {
  		B var1 = new B();
  		C var2 = new C();
  	}
  	
  	// 바깥 클래스의 정적 필드 초기화
  //	static B field3 = new B();
  	static C field4 = new C();
  	
  	// 바깥 클래스의 정적 메소드 초기화
  	static void method2() {
  //		B var1 = new B();
  		C var2 = new C();
  	}
  
  	// 인스턴스 멤버 클래스
  	class B { }
  	
  	// 정적 멤버 클래스
  	static class C { }
  }
  ```

  

  * 멤버 클래스에서 사용 제한

    * 멤버 클래스가 인스턴스 또는 정적으로 선언됨에 따라 **멤버 클래스 내부에서** 바깥 클래스의 필드와 메소드 접근 시 제한이 따름

    * 인스턴스 멤버 클래스 안에서는 바깥 클래스의 모든 필드와 모든 메소드에 접근 **가능**

    * 정적 멤버 클래스 안에서는 바깥 클래스의 정적 필드와 메소드에만 접근 **가능**

      * 인스턴스 필드와 메소드는 접근 **불가능**

      

  ```java
  public class A {
  	
  	int field1;
  	void method1() { }
  	
  	static int field2;
  	static void method2() { }
  	
  	class B{
  		void method() {
  			field1 = 10;
  			method1();
  			
  			field2 = 10;
  			method2();
  		}
  	}
  	
  	static class C{
  		void method() {
  //			field1 = 10;
  //			method1();
  			
  			field2 = 10;
  			method2();
  		}
  	}
  }
  ```

  

  * 로컬 클래스에서 사용 제한

    * **로컬 클래스 내부**에서는 바깥 클래스의 필드나 메소드를 제한 없이 사용 **가능**
    * 문제는 <u>메소드의 매개 변수나 로컬 변수를 로컬 클래스에서 사용할 때</u>
    * **로컬 클래스의 객체**는 메소드 실행이 끝나도 힙 메모리에 존재해서 계속 사용 가능
    * **매개 변수나 로컬 변수는** 메소드 실행이 끝나면 스택 메모리에서 사라짐
    * 자바에서는 문제 해결을 위해 컴파일 시 로컬 클래스에서 사용하는 매개 변수나 로컬 변수의 값을 로컬 클래스 내부에 복사해두고 사용
    * 매개 변수나 로컬 변수가 수정되어 값이 변경되면, 로컬 클래스에 복사해 둔 값과 달라지는 문제를 해결하기 위해 매개 변수나 로컬 변수를 **final로 선언**해서 수정을 막음
    * 즉, **로컬 클래스에서 사용 가능한 것은 final로 선언된 매개 변수와 로컬 변수뿐**
    * 자바 8부터 final 키워드 없이 선언된 매개 변수와 로컬 변수를 사용해도 컴파일 에러 X
      * final 선언을 하지 않아도 여전히 값을 수정할 수 없는 final 특성을 갖는다는 의미
    * final 키워드 존재 여부의 차이점은 로컬 클래스의 복사 위치
      * final 키워드가 있다면, 로컬 클래스의 메소드 내부에 지역 변수로 복사
      * final 키워드가 없다면 로컬 클래스의 필드로 복사
    * <u>로컬 클래스의 내부 복사 위치에 신경 쓸 필요 없이</u> **로컬 클래스에서 사용된 매개 변수와 로컬 변수는 모두 final 특성**을 갖는다는 것만 기억하자

    

  ```java
  public class Outter {
  	
  	//Java 7 이전
      public void prevJava7(final int arg){
          final int localVar = 1;
          
          //수정 불가능
          //arg = 100;
          //localVar = 100;
          
          class Inner{
          	public void method() {
          		int rst = arg + localVar;
          	}
          }
      }
      
      //Java 8 이후
      public void afterJava8(int arg) {
      	int localVar = 1;
      	
      	//수정 불가능
      	//arg = 100;
      	//localVar = 100;
      	
      	class Inner{
      		public void method() {
      			int rst = arg + localVar;
      		}
      	}
      }
  }
  ```



* 중첩 클래스에서 바깥 클래스 참조 얻기

  * 클래스 내부에서 this는 객체 자신의 참조
  * 중첩 클래스에서 this 키워드 사용 시 바깥 클래스의 객체 참조가 아니라, **중첩 클래스 객체 참조**
  * 중첩 클래스 내부에서 바깥 클래스의 객체 참조를 얻으려면 바깥 클래스의 이름을 this 앞에 붙여줌

  

  ```java
  Outter.this.field1;
  Outter.this.method1();
  ```



### 중첩 인터페이스 (Nested Interface)

* 클래스의 멤버로 선언된 인터페이스

* 클래스 내부에 선언하는 이유는 해당 클래스와 긴밀한 관계를 맺는 구현 클래스를 만들기 위함

* UI 프로그래밍에서 이벤트 처리 목적으로 많이 활용

* Button을 클릭했을 때 이벤트를 처리하는 객체를 받고 싶다고 가정

  * 아무 객체나 받을 수 없음

  * Button 내부에 선언된 중첩 인터페이스를 구현한 객체만 받아야 함

    * 중첩 인터페이스 타입으로 필드 선언
    * Setter 메소드로 구현 객체를 받아 필드에 대입
    * 버튼 이벤트 발생 시 인터페이스를 통해 구현 객체의 메소드 호출

    

  ```java
  public class Button {
  	// 인터페이스 타입 필드
  	OnClickListener listener;
  	
  	// 매개 변수의 다형성
  	void setOnClickListener(OnClickListener listener) {
  		this.listener = listener;
  	}
  	
  	// 구현 객체의 onClick() 메소드 호출
  	void touch() {
  		listener.onClick();
  	}
  	
  	//중첩 인터페이스
  	interface OnClickListener{
  		void onClick();
  	}
  }
  ```



* Button의 중첩 인터페이스인 OnClickListener를 구현한 두 개의 클래스

  * 버튼 이벤트 발생 시 두 가지 방법으로 이벤트를 처리함

  * 어떤 구현 객체를 생성해서 Button 객체의 setOnClickListener() 메소드로 셋팅하느냐에 따라 버튼의 touch() 메소드 실행 결과가 달라짐

    

  ```java
  public class Button {
  	OnClickListener listener;
  	void setOnClickListener(OnClickListener listener) {
  		this.listener = listener;
  	}
  	void touch() {
  		listener.onClick();
  	}
  	interface OnClickListener{
  		void onClick();
  	}
  }
  ```

  ```java
  public class CallListener implements Button.OnClickListener{
  	@Override
  	public void onClick() {
  		System.out.println("전화를 건다");
  	}
  }
  ```

  ```java
  public class MessageListener implements Button.OnClickListener{
  	@Override
  	public void onClick() {
  		System.out.println("메세지를 보낸다");
  	}
  }
  ```

  ```java
  public class Main {
  	public static void main(String[] args) {
  		Button button = new Button();
  		
  		button.setOnClickListener(new CallListener());
  		button.touch();
  		
  		button.setOnClickListener(new MessageListener());
  		button.touch();
  	}
  }
  ```

  ```text
  전화를 건다
  메세지를 보낸다
  ```



### 익명 객체 (Anonymous)

* 이름이 없는 객체

* 단독으로 생성할 수 없음

* 클래스를 상속하거나 인터페이스를 구현해야만 생성할 수 있음

* 필드의 초기값이나 로컬 변수의 초기값, 매개 변수의 매개값으로 주로 대입됨

* UI 이벤트 처리 객체나 스레드 객체를 간편하게 생성할 목적으로 많이 활용

* 익명 자식 객체 생성

  

  ```java
  ParentClass [필드|변수] = new ParentClass(){
      //필드
      //메소드
      //부모 클래스 메소드 재정의
  };
  
  /*
  	코드 해석
  	ParentClass(){}; : 부모 클래스를 상속해서 자식 클래스를 선언하라는 의미
  	new 연산자 : 선언된 자식 클래스를 객체로 생성
  	ParentClass() : 부모 생성자 호출
  */
  ```

  

  * 부모 타입으로 필드나 변수를 선언하고 자식 객체를 초기값으로 대입할 때
    * 만약 자식 클래스가 재사용 되지 않고, 오로지 해당 필드와 변수의 초기값으로만 사용한다면
    * 익명 자식 객체를 생성해서 초기값으로 대입하는 것이 좋음
    * 익명 자식 객체와 일반 클래스의 차이점은 생성자를 생성할 수 없다는 것
  * 익명 자식 객체에 새롭게 정의된 필드와 메소드는 **익명 자식 객체 내부에서만 사용**
    * 외부에서는 필드와 메소드에 접근할 수 없음
    * 익명 자식 객체는 부모 타입 변수에 대입되므로 부모 타입에 선언된 것만 사용할 수 있음
  * 익명 자식 객체 생성 예제

  

  ```java
  public class Child extends Parent{
      // 부모 타입으로 필드나 변수를 선언하고 자식 객체를 초기값으로 대입
  	// 기본적인 방법
  	class A {
  		Parent field = new Child();
  		void method() {
  			Parent localVar = new Child();
  		}
  	}
  }
  ```

  ```java
  public class Child extends Parent{
  	// 필드 선언할 때 초기값으로 익명 자식 객체 생성해서 대입하기
  	class A{
  		Parent field = new Parent() {
  			int childField;
  			void childMethod() { }
  			
  			@Override
  			public void parentMethod() {
  				System.out.println("부모 메소드 오버라이딩");
  			};
  		};
  	}
  }
  ```

  ```java
  public class Child extends Parent{
  	// 메소드 내에서 로컬 변수 선언할 때 초기값으로 익명 자식 객체 생성해서 대입
  	class A{
  		void method() {
  			Parent localVar = new Parent() {
  				int childField;
  				void childMethod() { }
  				
  				@Override
  				public void parentMethod() {
  					System.out.println("부모 메소드 오버라이딩");
  				}
  			};
  		}
  	}
  }
  ```

  ```java
  public class Child extends Parent{
  	// 메소드의 매개 변수가 부모 타입일 경우
  	// 메소드 호출 코드에서 익명 자식 객체를 생성해서
  	// 매개값으로 대입
  	class A{
  		void method1(Parent parent) { }
  		void method2() {
  			
  			//method1() 호출
  			method1(
  				new Parent() { // method1()의 매개값으로 익명 자식 객체 대입
  					int childField;
  					void childMethod() { }
  					@Override
  					public void parentMethod() { }
  				}
  			);
  		}
  	}
  }
  ```



* 익명 구현 객체 생성

  

  ```java
  Interface [필드|변수] = new Interface(){
      //인터페이스에 선언된 추상 메소드의 실체 메소드 선언
      //필드
      //메소드
  };
  
  /*
  	코드 해석
  	Interface(){}; : 인터페이스를 구현해서 클래스를 선언하라는 의미
  	new 연산자 : 선언된 클래스를 객체로 생성
  */
  ```

  

  * 인터페이스 타입으로 필드나 변수를 선언하고 구현 객체를 초기값으로 대입할 때
    * 구현 클래스가 재사용되지 않고, 오로지 해당 필드와 변수의 초기값으로만 사용한다면
    * 익명 구현 객체를 초기값으로 대입하는 것이 좋음
  * 익명 구현 객체 생성 예제

  

