# 중첩 클래스와 중첩 인터페이스

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 개념
* 중첩 클래스 (Nested Class)
* 중첩 인터페이스
* 익명 객체



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

  ```java
  AClass aClass = new AClass();
  AClass.BClass bClass = aClass.new BClass();
  
  bClass.field1 = 3;
  bClass.method1();
  ```



* 정적 멤버 클래스

  * static 키워드로 선언된 클래스
  * 모든 종류의 필드와 메소드 선언 가능
  * A 클래스 외부에서 정적 멤버 클래스 C의 객체를 생성하기 위해서는 A 객체를 생성할 필요가 없음

  ```java
  AClass.CClass cClass = new AClass.CClass();
  
  // 인스턴스 필드 사용
  cClass.field1 = 3;
  //인스턴스 메소드 호출
  cClass.method1();
  //정적 필드 사용
  AClass.CClass.field2 = 5;
  //정적 메소드 호출
  AClass.CClass.method2();
  ```

  

* 로컬 클래스

  * 메소드 내부에서 선언되는 중첩 클래스
  * 메소드 실행 시에만 사용 가능, 메소드 실행 종료 시 소멸
  * 접근을 제한할 필요가 없어 접근 제한자(public, private) 및 static을 붙일 수 없음
  * 로컬 클래스 내부에는 인스턴스 필드와 메소드만 선언 가능
  * 정적 필드와 메소드 선언 불가능
  * 메소드가 실행될 때 메소드 내에서 객체를 생성하고 사용해야 함
  * 비동기 처리를 위해 스레드 객체 만들 때 주로 사용

  ```java
  void method(){
      //로컬 클래스
      class DClass{
          DClass(){ }
          int field1;
          void method1(){ }
          //정적 필드, 메소드 선언 X
          //static int field2;
          //static void method2(){ }
      }
      DClass dClass = new DClass();
      dClass.field1 = 3;
      dClass.method1();
  }
  ```

  







