# 예외 처리

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 개념
* 실행 예외
* 예외 처리 코드
* 예외 떠넘기기
* 사용자 정의 예외
* 예외 발생 시키기
* 예외 정보 얻기



### 개념

* 에러 (Error)
  * 컴퓨터 하드웨어의 오동작 또는 고장으로 인해 응용프로그램 실행 오류가 발생하는 것
  * 에러 발생 시 프로그램 곧바로 종료
  * JVM 실행에 문제가 생겼다는 것
    * JVM 위에서 실행되는 프로그램을 아무리 견고하게 만들어도 결국 실행 X
  * 개발자는 이런 에러에 대처할 방법이 전혀 없음
  * 자바에서는 에러 이외에 예외(Exception)라고 부르는 오류가 있음
* 예외 (Exception)
  * 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류
  * 예외 발생 시 프로그램 곧바로 종료
  * 예외 처리를 통해 프로그램 종료하지 않고 정상 실행 상태 유지 가능
  * 일반 예외 (Exception)
    * 컴파일러 체크 예외
    * 자바 소스를 컴파일하는 과정에서 예외 처리 코드가 필요한지 검사하는 예외
    * 예외 처리 코드가 없다면 컴파일 오류 발생
  * 실행 예외 (Runtime Exception)
    * 자바 소스를 컴파일하는 과정에서 예외 처리 코드가 필요한지 검사하지 않는 예외
  * 자바에서는 예외를 클래스로 관리함
  * JVM은 프로그램 실행 중, 예외가 발생하면 해당 예외를 예외 클래스로 객체 생성
    * 그 후, 예외 처리 코드에서 예외 객체를 이용할 수 있도록 해줌
  * 모든 예외 클래스들은 java.lang.Exception 클래스를 상속 받음
    * 일반 예외는 Exception을 상속, RuntimeException은 상속받지 않는 클래스
    * 실행 예외는 RuntimeException을 상속 받는 클래스



### 실행 예외

* 자바 컴파일러가 체크하지 않기 때문에, 오로지 **개발자의 경험에 의해** 예외 처리 코드 삽입
* 예외 처리 코드를 넣지 않았을 경우, 예외 발생 시 프로그램 곧바로 종료
* 자바 프로그램에서 자주 발생되는 실행 예외
  * NullPointerException
    * 객체 참조가 없는 상태
    * null 값을 갖는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을 때 발생
    * 즉, 객체가 없는 상태에서 객체를 사용하려 했으니 예외 발생
  * ArrayIndexOutOfBoundsException
    * 배열에서 인덱스 범위를 초과하여 사용했을 때 발생
    * 배열값을 읽기 전에 배열의 길이를 먼저 조사하여 예외 발생 방지
  * NumberFormatException
    * 문자열을 숫자로 변환하는 경우, 숫자로 변환될 수 없는 문자가 포함되어 있을 때 발생
      * Wrapper 클래스
  * ClassCastException
    * 억지로 타입 변환을 시도할 경우 발생
    * 타입 변환은 상위-하위 클래스와 구현 클래스-인터페이스 간에 발생
      * 이러한 관계가 아니라면 클래스는 다른 클래스로 타입 변환 불가능
    * 타입 변환 전에 타입 변환이 가능한지 instanceof 연산자로 확인



### 예외 처리 코드

* 프로그램에서 예외 발생 시, 프로그램 종료를 막고 정상 실행을 유지하도록 처리하는 코드

* 일반 예외의 경우, 자바 컴파일러가 예외 발생 가능성이 있는 코드를 발견하여 컴파일 오류를 발생 시킴

* 일반 예외의 경우

  * 자바 컴파일러가 예외 발생 가능성이 있는 코드를 발견함
  * 컴파일 오류를 발생시켜 개발자로 하여금 예외 처리 코드를 작성하도록 강제함

* 실행 예외의 경우

  * 컴파일러가 체크해주지 않음
  * 예외 처리 코드를 개발자의 경험을 바탕으로 작성해야 함

* try-catch-finally

  * 예외 처리 코드 블록
  * 생성자 내부와 메소드 내부에서 작성
  * 일반 예외와 실행 예외가 발생할 경우 예외를 처리할 수 있도록 해줌
  * try 블록
    * 예외 발생 가능 코드가 위치함
    * 예외 발생 없이 정상 실행 시 catch 블록은 실행되지 않고 finally 블록 코드 실행
  * catch 블록
    * try 블록 코드에서 예외 발생 시 즉시 실행을 멈추고 catch 블록으로 이동하여 예외 처리 코드 실행
  * finally 블록
    * 옵션으로 생략 가능
    * 예외 발생 여부와 상관없이 **항상 실행할** 내용이 있을 경우에만 finally 블록 작성
    * try 블록과 catch 블록에서 return문 사용해도 finally 블록은 항상 실행됨

* 다중 catch

  * 발생되는 예외별로 예외 처리 코드를 다르게 할 때 사용
  * try 블록은 동시다발적으로 예외가 발생하지 않음
    * 하나의 예외가 발생하면 즉시 실행을 멈추고 해당 catch 블록으로 이동
    * 즉, catch 블록이 여러 개여도 하나의 catch 블록만 실행됨

  ```java
  try{
      // ArrayIndexOutOfBoundsException 발생
      // NumberFormatException 발생
  }catch(ArrayIndexOutOfBoundsException e){
      // 예외 처리
  }catch(NumberFormatException e){
      // 예외 처리
  }
  ```

  

  * 다중 catch 블록 작성 시 주의할 점
    * catch 블록은 위에서부터 차례대로 검색됨
    * 즉, 상위 예외 클래스가 하위 예외 클래스보다 위에 있다면 하위 예외 클래스는 실행되지 않음
    * 상위 예외 클래스가 하위 예외 클래스보다 아래쪽에 위치해야 함

* 멀티 catch

  * 자바 7부터 하나의 catch 블록에서 여러 개의 예외를 처리할 수 있는 기능 추가
  * catch 괄호 안에 동일하게 처리하고 싶은 예외를 |로 연결



### 예외 떠넘기기

* 메소드를 호출한 곳으로 예외를 떠넘김
* `throws` 키워드 사용
  * 메소드 선언부 끝에 작성되어 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할
  * thorws 키워드 뒤에는 떠넘길 예외 클래스를 쉼표로 구분해서 나열
  * `throws Exception`
    * 모든 예외를 간단히 떠넘김
* throws 키워드가 붙어있는 메소드는 반드시 try 블록 내에서 호출되어야 하고, catch 블록에서 떠넘겨 받은 예외를 처리해야 함

```java
public void method1(){
    try{
        method2();
    }catch(ClassNotFoundException e){
        //예외 처리 코드
    }
}

public void method2() thorws ClassNotFoundException{
    Class class = Class.forName("java.lan.String2");
}
```



### 사용자 정의 예외

* 애플리케이션 서비스와 관련된 예외로, **애플리케이션 예외(Application Exception)**라 함
  * 자바 표준 API에 존재하지 않는 예외 등
* 개발자가 직접 정의해서 만들어야 하므로 사용자 정의 예외라고도 함
* 일반 예외, 실행 예외 둘 다 선언 가능
  * 일반 예외로 선언할 경우 Exception 상속
  * 실행 예외로 선언할 경우 RuntimeException 상속
* 사용자 정의 예외 클래스명은 Exception으로 끝나는 것이 좋음
* 필드, 생성자, 메소드 선언들을 포함할 수 있지만 **대부분 생성자 선언만 포함**
* 일반적으로 생상자는 두 개를 선언
  * 매개 변수가 없는 기본 생성자
  * 예외 발생 원인(예외 메시지)을 전달하기 위해 String 타입의 매개 변수를 갖는 생성자
    * 상위 클래스의 생성자를 호출하여 예외 메시지를 넘겨줌
    * 예외 메시지의 용도는 catch 블록의 예외 처리 코드에서 이용하기 위함

```java
public class BalanceInsufficientException extends Exception{
    public BalanceInsufficientException() { }
    public BalanceInsufficientException(String message){
        super(message);
    }
}
```



### 예외 발생 시키기

```java
throws new XXXException();
throws new XXXException("메시지");
```

* 예외 객체를 생성할 때는 기본 생성자 또는 예외 메시지를 갖는 생성자 중 어떤 것을 사용해도 무관
* 단, catch 블록에서 예외 메시지가 필요하다면 예외 메시지를 갖는 생성자 이용
* 예외 발생 코드를 가지고 있는 메소드는 내부에서 try-catch 블록으로 처리 가능
  * 하지만 대부분 자신을 호출한 곳에서 예외를 처리하도록 throws, 예외를 떠넘김

```java
public class Account{
    private long balance;
    
    public Account(){ }
    
    public long getBalance(){
        return balance;
    }
    public void deposit(int money){
        balance += money;
    }
                                    // 사용자 정의 예외 떠넘기기
    public void withdraw(int money) throws BalanceInsufficientException{
        if(balance < money){
            // 사용자 정의 예외 발생
            throws new BalanceInsufficientException("잔고 부족");
        }
        balance -= money;
    }
}
```



### 예외 정보 얻기

* try 블록에서 예외 발생 시, 예외 객체는 catch 블록의 매개 변수에서 참조함
  * 즉, 매개 변수를 이용하면 예외 객체의 정보를 알 수 있음

* 예외 메시지를 이용하여 예외를 발생시키면, 예외 메시지는 자동으로 예외 객체 내부에 저장됨
* 예외 메시지의 내용에는 왜 예외가 발생했는지 간단한 설명이 포함됨
* `getMessage( )` 메소드의 리턴값을 이용하여 예외 코드를 포함한 상세한 예외 정보를 얻을 수 있음
* `printStackTrace( )` 메소드는 예외 발생 코드를 추적해서 모두 콘솔에 출력함
  * 어떤 예외가 어디에서 발생했는지 상세하게 출력
  * 프로그램을 테스트하면서 오류 찾을 때 활용함

```java
try{
    
} catch(예외 클래스 e){
    //예외가 가지고 있는 메시지 얻기
    String message = e.getMessage();
    
    //예외 발생 경로 추척
    e.printStackTrace();
}
```