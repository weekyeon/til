# 기본 API 클래스 1

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 자바 API 도큐먼트
* java.lang
* java.lang.Object
* java.lang.System
* java.lang.Class
* java.lang.String
* java.lang.StringBuffer
* java.lang.StringBuilder
* java.lang.Math
* java.lang.Wrapper



### 자바 API 도큐먼트

* [Java 7](https://docs.oracle.com/javase/7/docs/api/) / [Java 8](https://docs.oracle.com/javase/8/docs/api/)

* Application Programming Interface, API
* 자바 라이브러리라고 부르기도 함
* 프로그램 개발에 자주 사용되는 클래스 및 인터페이스 모음
* API 저장 위치
  * <JDK 설치 경로>\jre\lib\rt.jar



### java.lang

* 자바 프로그램의 기본적인 클래스를 담은 패키지
* import 없이 사용 가능
* Object, System, Class, String, StringBuffer, StringBuilder, Math, Wrapper



### java.lang.Object

* 자바의 최상위 부모 클래스
* 필드가 없고 메소드들로 구성되어 있음
  * 모든 클래스가 Object를 상속하기 때문에 모든 클래스에서 사용 가능

#### equals()

* 객체 비교 메소드
* 두 객체가 논리적으로 동일한 객체라면 true, 그렇지 않으면 false
  * 논리적으로 동등하다, 같은 객체든 다른 객체든 상관없이 객체가 **저장하고 있는 데이터**가 동일함
* 매개 타입이 Object이므로 모든 객체를 매개값으로 대입 가능
* 직접 사용되지 않고 하위 클래스에서 재정의하여 이용
* 메소드 재정의 시 매개값(비교 객체)이 기준 객체와 동일한 타입의 객체인지 먼저 확인 필요
  * instanceof 연산자

#### hashCode()

* 객체 비교 메소드

* **객체 해시코드** : 객체를 식별할 하나의 정수값

  

```java
public class HashCodeEx {
	int num;
	
	public HashCodeEx(int num) {
		this.num = num;
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		HashCodeEx hashCodeEx1 = new HashCodeEx(1);
		HashCodeEx hashCodeEx2 = new HashCodeEx(1);
		
		System.out.println("해시코드 : "+hashCodeEx1.hashCode());
		System.out.println("해시코드 : "+hashCodeEx2.hashCode());
	}
}
```

```text
해시코드 : 366712642
해시코드 : 1829164700
```



* hashCode() 메소드는 객체의 메모리 번지를 이용해 해시코드를 만들어 리턴함

* 즉, 객체마다 다른 해시코드 값을 가지고 있음

* 논리적 동등 비교 시 hashCode() 메소드 오버라이딩 필요

  * hashCode() 메소드 실행 후 리턴된 해시코드 값이 같은지 확인
  * 해시코드 값이 다르면 다른 객체로 판단, 같으면 equals() 메소드로 다시 비교

* hashCode() 메소드가 true여도 equals() 리턴값이 false면 **다른 객체**

* 객체의 동등 비교를 위해서는 equals() 메소드뿐만 아니라 hashCoda() 메소드도 재정의해서 논리적 동등 객체일 경우 동일한 해시코드가 리턴되도록 해야 함

  

```java
public class HashCodeEx {
	
	int num;
	
	public HashCodeEx(int num) {
		this.num = num;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Key) {
			HashCodeEx compareNum = (HashCodeEx) obj;
			if(this.num == compareNum.num) {
				return true;
			}
		}
		return false;
	}
	
	//id가 동일한 문자열인 경우 같은 해시 코드 리턴
	@Override
	public int hashCode() {
		return num;
	}
	
}
```

```text
해시코드 : 1
해시코드 : 1
```



#### toString()

* 객체 문자 정보(문자열) 리턴 메소드
* Object 하위 클래스는 toString() 메소드를 재정의하여 문자열을 리턴하도록 되어 있음

#### clone()

* 객체 복제 메소드
  * 객체 복제, 원본 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성하는 것
* 객체를 복제하는 이유는 원본 객체를 안전하게 보호하기 위함
* 얕은 복제
  * 단순히 필드값을 복사하여 객체를 복제함
  * 필드가 **기본 타입**일 경우 **값 복사**
  * 필드가 **참조 타입**일 경우 **객체 번지 복사**
  * clone() 메소드는 자신과 동일한 필드값을 가진 **얕은 복제**된 객체를 리턴
  * clone() 메소드로 객체를 복제하려면 원본 객체는 반드시 java.lang.Cloneable 인터페이스 구현해야 함
    * 클래스 설계자가 복제를 허용한다는 의도적인 표시를 하기 위함
    * Cloneable 인터페이스를 구현하지 않으면 CloneNotSupportedException 예외 발생, 복제 실패
    * 즉, clone() 메소드는 예외 처리 코드가 필요함
  * 얕은 복제의 단점
    * 참조 타입 필드의 경우, 원본 객체의 필드와 복제 객체의 필드는 같은 객체를 참조함
    * 복제 객체에서 참조 객체를 변경하면 원본 객체도 변경된 객체를 가지게 됨
* 깊은 복제
  * 참조하고 있는 **객체도 복제**하는 것
  * Object의 clone() 메소드 재정의하여 참조 객체 복제하는 코드 직접 작성해야 함

#### finalize()

* 객체 소멸자 메소드
* *배경 지식*, 쓰레기 수집기(Garbage Collector, GC)
  * GC는 메모리가 부족할 때 또는 CPU가 한가할 때 JVM에 의해 자동 실행됨
  * Heap 영역에서 참조하지 않는 배열이나 객체를 자동으로 소멸시킴
  * GC는 객체 소멸 직전, 마지막으로 객체 소멸자인 finalize() 메소드를 실행시킴
* 기본적으로 실행 내용이 없음
* 사용했던 자원을 닫거나 중요한 데이터를 저장하고 싶다면 Object의 finalize() 재정의 가능
* finalize() 메소드는 객체를 무작위로 소멸시킴
  * 순서대로 소멸 시키지도 않고, 객체를 전부 소멸시키지도 않음
  * 메모리 상태를 보고 일부만 소멸시킴
  * 즉, finalize() 메소드가 호출되는 시점은 명확하지 않음
* 프로그램이 종료될 때 즉시 자원을 해제하거나 즉시 데이터를 최종 저장해야 하는 경우
  * 일반 메소드에서 작성하고 프로그램이 종료될 때 명시적으로 메소드 호출하는 것이 좋음



### java.lang.System

* 운영체제의 일부 기능을 이용할 수 있도록 해주는 클래스
* System 클래스의 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있음

#### exit()

* 현재 실행되고 있는 프로세스를 강제 종료시키는 메소드
* **종료 상태값** : 지정한 int 매개값
  * 일반적으로 정상 종료일 경우 0
  * 비정상 종료일 경우 0 이외의 다른 값
  * 어떤 값을 주더라도 종료가 됨
  * 특정 값이 입력된 경우만 종료하고 싶다면 자바의 보안 관리자를 직접 설정해서 종료 상태값 확인

#### gc()

* 쓰레기 수집기 실행 메소드
* GC는 개발자가 직접 코드로 실행시킬 수 없고, JVM에게 가능한 빨리 실행해 달라고 요청하는 것
* 쓰레기가 생길 때마다 GC가 동작한다면
  * 정작 수행되어야 할 프로그램의 속도 저하 (성능 저하)
  * 메모리가 충분하다면 굳이 GC를 실행할 필요가 없음
  * 즉, gc() 메소드는 메모리가 열악하지 않은 환경이라면 거의 사용할 일이 없음

#### currentTimeMillis(), nanoTime()

* 현재 시각을 읽는 메소드
* 리턴값은 주로 프로그램의 실행 소요 시간 측정에 사용됨

#### getProperty()

* 시스템 프로퍼티를 읽는 메소드
  * 시스템 프로퍼티, JVM이 시작할 때 자동 설정되는 시스템 속성값

#### getenv()

* 환경 변수를 읽는 메소드



### java.lang.Class

* 자바는 클래스와 인터페이스의 메타 데이터를 java.lang.Class로 관리함
* 메타 데이터 : 클래스명, 생성자 정보, 필드 정보, 메소드 정보

#### getClass(), forName()

* Class 객체를 얻을 수 있는 메소드
* getClass() 메소드는 해당 클래스로 객체를 생성했을 때만 사용 가능
* forName() 메소드는 객체 생성 전에 직접 Class 객체를 얻을 수 있음
  * 패키지명을 포함한 클래스 전체 이름을 매개값으로 받고 Class 객체를 리턴함
  * 매개값으로 주어진 클래스를 찾지 못하면 ClassNotFoundException 예외 발생
  * 예외 처리 코드 필요함

```java
public class Car { ... }
```

```java
public class ClassEx {
	public static void main(String[] args) {
		Car car = new Car();
		Class class1 = car.getClass();
		System.out.println(class1.getName());
		System.out.println(class1.getSimpleName());
		System.out.println(class1.getPackage().getName());
		System.out.println("-----------------------");
		
		try {
			Class class2 = Class.forName("com.java.chap11.cls.Car");
			System.out.println(class2.getName());
			System.out.println(class2.getSimpleName());
			System.out.println(class2.getPackage().getName());
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
}
```



