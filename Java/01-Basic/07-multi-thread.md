# 멀티 스레드

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 개념
  * 프로세스
  * 멀티 태스킹
  * 멀티 스레드
  * 메인 스레드
* 작업 스레드 생성과 실행



### 개념

* 프로세스

  * 실행 중인 하나의 애플리케이션
  * 사용자가 애플리케이션 실행 ▶ 운영체제로부터 실행에 필요한 메모리 할당 ▶ 애플리케이션 코드 실행
  * 하나의 애플리케이션은 다중 프로세스를 만들기도 함
    * 브라우저를 두 개 실행했다면, 두 개의 브라우저 프로세스 생성한 것

* 멀티 태스킹

  * 두 가지 이상의 작업을 동시에 처리함
  * 운영체제는 멀티 태스킹을 할 수 있도록 CPU 및 메모리 자원을 프로세스마다 적절히 할당해 병렬 실행
  * 멀티 태스킹이 꼭 멀티 프로세스인 것은 아님
    * 한 프로세스 내에서 멀티 태스킹하도록 만들어진 애플리케이션도 있음
    * EX) 미디어 플레이어, 메신저 등

* 멀티 스레드

  * 하나의 프로세스가 두 가지 이상의 작업을 처리함
  * 애플리케이션 개발에 꼭 필요한 기능
  * 스레드(Thread)는 사전적 의미로 한 가닥의 실을 뜻함
    * 한 가지 작업을 실행하기 위해 순차적으로 실행할 코드를 실처럼 이어 놓았다고 해서 유래된 이름
    * **하나의 스레드는 하나의 코드 실행 흐름**
    * 한 프로세스 내에 스레드가 두 개라면 **두 개의 코드 실행 흐름**이 생긴다는 뜻
  * 멀티 프로세스와의 차이점
    * 멀티 프로세스
      * 애플리케이션 단위의 멀티 태스킹
      * 운영체제에서 할당받은 메모리 이용해 실행하기 때문에 **독립적**임
      * 즉, 하나의 프로세스에서 오류가 발생해도 다른 프로세스에게 영향 X
    * 멀티 스레드
      * 애플리케이션 내부에서의 멀티 태스킹
      * 하나의 프로세스 내부에 생성되어 하나의 스레드가 예외 발생 시 프로세스 자체가 종료될 수 있음
      * 즉, **다른 스레드에게 영향을 미치기**때문에 예외 처리에 신경써야 함
  * 멀티 스레드 사용처
    * 대용량 데이터 처리 시간을 줄이기 위해 데이터를 분할해서 병렬로 처리하는 곳
    * UI가 있는 애플리케이션에서 네트워크 통신할 때
    * 다수 클라이언트의 요청을 처리하는 서버 개발

  ![멀티 프로세스와 멀티 스레드 개념](https://github.com/weekyeon/til/blob/master/Java/img/java-thread1.png)

* 메인 스레드

  * 모든 자바 애플리케이션은 메인 스레드가 main() 메소드 실행하며 시작
  * 메인 스레드는 main() 메소드의 첫 코드부터 아래로 순차적으로 실행
  * main() 메소드의 마지막 코드를 실행하거나 return문 만나면 실행 종료
  * 필요에 따라 작업 스레드들을 만들어 병렬로 코드 실행 가능
    * 즉, 멀티 스레드를 생성해서 멀티 태스킹 수행

  ![메인 스레드](https://github.com/weekyeon/til/blob/master/Java/img/java-thread1.png)

  * 작업 스레드 1을 생성하고 실행한 다음, 작업 스레드 2를 생성하고 실행함
  * 싱글 스레드 애플리케이션에서는 메인 스레드 종료 시 프로세스도 종료
  * 멀티 스레드 애플리케이션에서는 실행 중인 스레드가 하나라도 있다면, 프로세스 종료 X
    * 메인 스레드가 작업 스레드보다 먼저 종료돼도 작업 스레드가 계속 실행 중이라면 프로세스 종료 X



### 작업 스레드 생성과 실행

* 멀티 스레드로 실행하는 애플리케이션 개발을 위해 **몇 개의 작업을 병렬로 실행할지 결정**하고 **각 작업 별로 스레드 생성** 필요

* 메인 스레드는 **반드시 존재**하기 때문에 메인 작업 이외에 추가적인 병렬 작업 수만큼 스레드 생성

* 자바에서는 작업 스레드도 **객체로 생성**되기 때문에 클래스 필요함

  * `1` java.lang.Thread 클래스 직접 객체화해서 작업 스레드 생성

  ```java
  import java.awt.Toolkit;
  
  public class BeepTask implements Runnable{
  	@Override
  	public void run() {
  		//스레드 실행 내용
  		Toolkit toolkit = Toolkit.getDefaultToolkit();
  		for(int i=0; i<5; i++) {
  			toolkit.beep();
  			try {
  				Thread.sleep(500);
  			} catch (Exception e) {
  			
  			}
  		}
  	}
  }
  ```

  ```java
  public class BeepPrintEx {
  	
  	// 메인 스레드
  	public static void main(String[] args) {
  		
  	// 작업 스레드 생성
  		Runnable beepTask = new BeepTask();
          // Runnable을 매개값으로 갖는 생성자 호출
  		Thread thread = new Thread(beepTask);
  		
  	// 작업 스레드 수행 작업
  		thread.start();
  		
  	// 메인 스레드 수행 작업
  		for (int i=0; i<5; i++) {
  			System.out.println("띵!");
  			try {
  				Thread.sleep(500);
  			} catch (Exception e) {
  				
  			}
  		}
  	}
  }
  ```

  

  * Runnable을 매개값으로 갖는 생성자 호출 필요

  * Runnable

    * 작업 스레드가 실행할 수 있는 코드를 가지고 있는 객체
    * 작업 내용을 가지고 있는 객체일뿐, 실제 스레드 X
    * 인터페이스 타입, 구현 객체 만들어 대입해야 함
    * Runnable에 정의된 run() 메소드를 구현 클래스에서 재정의해서 작업 스레드가 실행할 코드 작성
    * **작업 스레드 생성**의 정의
      * Runnable 구현 객체 생성 후 이를 매개값으로 Thread 생성자 호출까지
    * 작업 스레드는 start() 메소드 호출 시 실행됨 (즉시 실행 X)
    * start() 메소드 호출하면 작업 스레드는 매개값으로 받은 Runnable의 run() 메소드 실행과 함께 자신의 작업을 처리함

  * 코드 절약하기

    * `1` Runnable 익명 객체를 매개값으로 사용하기
      * 많이 사용되는 방법

    ```java
    Thread thread = new Thread(new Runnable(){
       public void run(){
           // 스레드가 실행할 코드
       } 
    });
    ```

    

    * `2` 람다식을 매개값으로 사용하기
      * Runnable 인터페이스는 run() 메소드 하나만 정의되어 있어 함수적 인터페이스에 해당
      * 자바 8부터 지원

    ```java
    Thread thread = new Thread(()->{
      // 스레드가 실행할 코드  
    });
    ```

    

  * `2` Thread 하위 클래스로부터 생성

  ```java
  import java.awt.Toolkit;
  
  public class BeepThread extends Thread{
  	@Override
  	public void run() {
  		//스레드 실행 내용
  		Toolkit toolkit = Toolkit.getDefaultToolkit();
  		for(int i=0; i<5; i++) {
  			toolkit.beep();
  			try {
  				Thread.sleep(500);
  			} catch (Exception e) {
  			
  			}
  		}
  	}
  }
  ```

  ```java
  public class BeepPrintEx {
  	
  	// 메인 스레드
  	public static void main(String[] args) {
  		
  	// 작업 스레드 생성
  		Thread thread = new BeepThread();
  		thread.start();
  		
  	// 메인 스레드 수행 작업
  		for (int i=0; i<5; i++) {
  			System.out.println("띵!");
  			try {
  				Thread.sleep(500);
  			} catch (Exception e) {
  				
  			}
  		}
  	}
  }
  ```

  

  * 작업 스레드가 실행할 작업을 Runnable로 만들지 않고, Thread의 하위 클래스로 작업 스레드를 정의하면서 작업 내용을 포함시키는 방법

  * Thread 클래스 상속 후 run() 메소드 재정의하여 스레드가 실행할 코드 작성

  * 작업 스레드 클래스로부터 작업 스레드 객체를 생성하는 방법은 일반적인 객체를 생성하는 방법과 동일

  * 코드 절약하기

    * `1` Thread 익명 객체로 작업 스레드 객체 생성

    ```java
    Thread thread = new Thread(){
        @Override
    	public void run() {
    		Toolkit toolkit = Toolkit.getDefaultToolkit();
    		for(int i=0; i<5; i++) {
    			toolkit.beep();
    			try {
    				Thread.sleep(500);
    			} catch (Exception e) {
    			
    			}
    		}
    	}
    };
    ```

    