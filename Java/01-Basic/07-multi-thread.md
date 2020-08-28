# 멀티 스레드

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 개념
  * 프로세스
  * 멀티 태스킹
  * 멀티 스레드
  * 메인 스레드
* 작업 스레드 생성과 실행
* 스레드 이름
* 스레드 우선순위
* 동기화(Synchronized)
* 스레드 상태
* 스레드 상태 제어
* 데몬 스레드
* 스레드 그룹
* 스레드 풀



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

  ![메인 스레드](https://github.com/weekyeon/til/blob/master/Java/img/java-thread2.png)

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
  		Thread thread = new Thread(beepTask);
      	// ▲ Runnable을 매개값으로 갖는 생성자 호출
  		
  		// 작업 스레드 수행 작업
  		thread.start();
  		
  		// 메인 스레드 수행 작업
  		for (int i=0; i<5; i++) {
  			System.out.println("띵!");
  			try {
  				Thread.sleep(500);
  			} catch (Exception e) { }
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
  			} catch (Exception e) { }
  		}
  	}
  }
  ```

  

  * 작업 스레드가 실행할 작업을 Thread의 하위 클래스로 정의하면서 작업 내용을 포함

  * Thread 클래스 상속 후 run() 메소드 재정의하여 스레드가 실행할 코드 작성

  * 작업 스레드 객체를 생성하는 방법은 일반적인 객체 생성과 동일

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



### 스레드 이름

* 스레드는 자신의 이름을 가지고 있음
* 디버깅 시 어떤 스레드가 어떤 작업을 하는지 조사할 목적으로 가끔 사용
* 메인 스레드 이름 : main
* 개발자가 직접 생성한 스레드 : Thread-n
  * 자동으로 설정됨
  * n은 스레드의 번호
  * 다른 이름으로 설정하고 싶다면 Thread 클래스의 setName() 메소드 이용
  * 스레드 이름을 알고 싶은 경우 Thread 클래스의 getName() 메소드 이용
  * setName(), getName() 메소드는 Thread의 인스턴스 메소드이므로 스레드 객체 참조 필요
    * 참조를 가지고 있지 않다면, Thread의 정적 메소드인 currentThread() 이용



### 스레드 우선순위

* 멀티 스레드는 **동시성(Concurrency)** 또는 **병렬성(Parallelism)**으로 실행됨

* 동시성

  * 멀티 작업을 위해 **하나의 코어에서** 멀티 스레드가 **번갈아가며** 실행하는 성질
  * 싱글 코어 CPU를 이용한 멀티 스레드 작업은 동시성 작업
    * 동시성 작업이 워낙 빠르다보니 병렬성으로 보일 뿐

* 병렬성

  * 멀티 작업을 위해 **멀티 코어에서** 개별 스레드를 **동시에** 실행하는 성질

* 스레드 스케줄링

  * 스레드 개수가 코어 수보다 많을 때, 스레드를 어떤 순서에 의해 동시성으로 실행할지 결정하는 것

  * 자바의 스레드 스케줄링은 우선순위(Priority) 방식과 순환 할당(Round-Robin) 방식 사용

    * 우선순위 방식

      * 우선순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케줄링
      * 쿼드 코어인 경우 4개 이하의 스레드 실행할 경우 우선순위 방식이 크게 영향 X
      * 개발자가 스레드 객체에 우선 순위 번호를 부여함
      * 우선순위는 1~10
        * 1 : 우선 순위가 가장 낮음
        * 10 : 우선 순위가 가장 높음
      * 우선순위를 부여하지 않으면 모든 스레드들은 기본적으로 5의 우선순위
      * `setPriority()` : 우선순위 변경 메소드
      * 코드 가독성을 높이기 위해 Thread 클래스의 상수를 이용하여 우선순위 부여도 가능

      ```java
      thread.setPriority(Thread.MAX_PRIORITY); // 10
      thread.setPriority(Thread.NORM_PRIORITY); // 5
      thread.setPriority(Thread.MIX_PRIORITY); // 1
      ```

      

    * 순환 할당 방식

      * 시간 할당량을 정해서 하나의 스레드를 정해진 시간만큼 실행하고 다시 다른 스레드 실행
      * JVM에 의해 시간 할당량이 정해지기 때문에 코드로 제어 불가



### 동기화(Synchronized)

* 공유 객체 사용 시 주의점

  * 싱글 스레드 프로그램에서는 한 개의 스레드가 객체를 독차지해서 사용하기 때문에 문제 없음
  * 멀티 스레드 프로그램에서는 스레드들이 객체를 공유해서 작업해야 하는 경우 문제 발생
    * 스레드 A를 사용하던 객체가 스레드 B에 의해 상태 변경될 가능성 있음
    * 즉, 스레드 A가 의도했던 것과 다른 결과 산출 가능

* 임계 영역(Critical section)

  * 멀티 스레드 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역
  * 자바는 임계 영역을 지정하기 위해 동기화 메소드와 동기화 블록 제공
  * 스레드가 객체 내부의 동기화 메소드/블록에 들어가면 즉시 객체 잠금!
  * 동기화 메소드/블록이 여러 개 있을 경우,
    * 스레드가 이들 중 하나를 실행할 때 다른 스레드는 해당 메소드와 다른 동기화 메소드/블록 실행 불가
    * 단, 일반 메소드는 실행 가능

* 동기화 메소드

  * 스레드가 사용 중인 객체를 다른 스레드가 변경할 수 없도록 함
  * 즉, 스레드 작업이 끝날 때까지 **객체에 잠금을 걸어** 다른 스레드가 사용할 수 없도록 함
  * 메소드 전체 내용이 임계 영역
  * 스레드가 동기화 메소드 실행 즉시 객체 잠금
  * 스레드가 동기화 메소드 실행 종료 시 객체 잠금 해제
  * 동기화 메소드 생성 방법
    * 메소드 선언에 synchronized 키워드 설정
    * synchronized 키워드는 인스턴스와 정적 메소드 어디든 사용 가능

  ```java
  public synchronized void method(){
      //임계 영역
      //단 하나의 스레드만 실행
  }
  ```



* 동기화 블록

  * 일부 영역만 임계 영역으로 만들고 싶을 때 사용
  * 동기화 블록의 외부 코드들은 여러 스레드가 동시에 실행할 수 있음
  * 동기화 블록의 내부 코드는 임계 영역이므로 한 번에 한 스레드만 실행 가능
    * 다른 스레드는 실행 불가

  ```java
  public void method(){
      //여러 스레드가 실행 가능 영역
      ...
      synchronized(공유객체){ //공유 객체가 객체 자신이면 this 사용 가능
          //임계 영역
          //단 하나의 스레드만 실행
      }
      //여러 스레드가 실행 가능 영역
      ...
  }
  ```



### 스레드 상태

![스레드 상태](https://github.com/weekyeon/til/blob/master/Java/img/java-thread3.png)

|   상태    | 설명                                                         |
| :-------: | ------------------------------------------------------------ |
| 실행 대기 | 스케줄링이 되지 않아 실행을 기다리고 있는 상태               |
|   실행    | 실행 대기 상태에 있는 스레드 중, CPU를 점유하고 run() 메소드 실행한 상태 |
|   종료    | 실행 상태에서 run() 메소드가 종료되면, 더는 실행할 코드가 없어 스레드의 실행이 멈춘 상태 |
| 일시 정지 | 실행 상태에서 실행 대기 상태로 가지 않고, 일시 정지하여 스레드가 실행할 수 없는 상태 |

* `getState()` : 스레드 상태에 따라 Thread.State 열거 상수 리턴하는 메소드

  |   상태    |  열거 상수   | 설명                                                  |
  | :-------: | :----------: | ----------------------------------------------------- |
  | 객체 생성 |     NEW      | 스레드 객체 생성 O<br>start() 메소드 호출 X           |
  | 실행 대기 |   RUNNABLE   | 실행 상태로 언제든 갈 수 있는 상태                    |
  | 일시 정지 |    WATING    | 다른 스레드가 통지할 때까지 기다리는 상태             |
  |           | TIMED_WATING | 주어진 시간 동안 기다리는 상태                        |
  |           |   BLOCKED    | 사용하고자 하는 객체의 락이 풀릴 때까지 기다리는 상태 |
  |   종료    |  TERMINATED  | 실행을 마친 상태                                      |



### 스레드 상태 제어

* 실행 중인 스레드의 상태를 변경하는 것
* 멀티 스레드 프로그램을 만들기 위해서는 정교한 스레드 상태 제어 필요
* 스레드 제어를 제대로 하기 위해서는 스레드의 상태 변화를 가져오는 메소드들 파악 필요

#### sleep()

* 실행 중인 스레드를 일정 시간 멈추게 하는 메소드
* sleep() 메소드는 Thread 클래스의 정적 메소드
* 주어진 시간 동안 일시 정지 후 **다시 실행 대기**로 돌아감
* 매개값에는 밀리세컨드 단위로 시간 설정
* 일시 정지 상태에서 주어진 시간이 되기 전에 interrupt() 메소드 호출 시 InterruptedException 발생
  * 예외 처리 필요

```java
try{
    Thread.sleep(1000);
}catch(InterruptedException e){
    // interrupt() 메소드 호출 시 실행됨
}
```



#### yield()

* 다른 스레드에게 실행을 양보하는 메소드
* 반복문이 무의미하게 반복되는 경우, 다른 스레드에게 양보하고 실행 대기 상태로 가는 게 성능에 좋음

```java
public class ThreadA extends Thread {	
	public boolean stop = false;
	public boolean work = true;
	
	public void run() {
		while(!stop) {
			if(work) {
				System.out.println("ThreadA 작업 내용");
                /*
                work의 값이 false라면, 그리고 false에서 true로 변경되는 시점이 불명확할 때
                while문은 어떠한 실행문도 실행하지 않고 무의미한 반복을 함
                */
			} else {
				Thread.yield();
			}
		}
		System.out.println("ThreadA 종료");
	}
}
```



#### join()

* 다른 스레드의 종료를 기다리는 메소드
* 스레드는 대게 다른 스레드와 독립적으로 실행하는 것이 기본적
  * 하지만, 계산 작업을 하는 스레드가 모든 계산 작업을 마쳤을 때, 계산 결과값을 받아 이용하는 경우 등 다른 스레드 종료까지 기다렸다가 실행해야 하는 경우도 있음
* ThreadA는 ThreadB가 종료할 때까지 일시 정지 상태
* ThreadB의 run() 메소드가 종료되면 ThreadA는 일시 정지에서 풀려 다음 코드 실행함

```java
public class SumThread extends Thread {	
	private long sum;
	
	public long getSum() {
		return sum;
	}

	public void setSum(long sum) {
		this.sum = sum;
	}

	public void run() {
		for(int i=1; i<=100; i++) {
			sum+=i;
		}
	}
}
```

```java
public class JoinExample {
	public static void main(String[] args) {
		SumThread sumThread = new SumThread();
		sumThread.start();
		try {
            //sunThread 종료할 때까지 메인 스레드 일시 정지
			sumThread.join();
		} catch (InterruptedException e) {
		}
		System.out.println("1~100 합계 : " + sumThread.getSum());
	}
}
```



#### wait(), notify(), notifyAll()

* 스레드 간 협업 메소드
* 경우에 따라 두 개의 스레드를 교대로 번갈아가며 실행해야 할 경우가 있음
  * 자신의 작업이 끝나면 상대방 스레드를 일시 정지 상태에서 풀어주고, 자신은 일시 정지 상태로 만듦
* 협업의 핵심은 공유 객체
  * 공유 객체는 두 스레드가 작업할 내용을 각각 동기화 메소드로 구분해 놓음
* 협업 과정
  * `1` ThreadA 작업 완료
  * `2` notify() 메소드 호출
  * `3` 일시 정지 상태의 ThreadB를 실행 대기 상태로 만듦
  * `4` ThreadA 자신은 두 번 작업하지 않도록 wait() 메소드 호출
  * `5` ThreadA 자신을 일시 정지 상태로 만듦
* wait() 메소드 대신 wait(long timeout) / wait(long timeout, int nanos) 사용 시 notify() 호출 필요 X
  * 지정된 시간이 지나면 스레드가 자동적으로 실행 대기 상태가 됨
* notifyAll()
  * notify()와 동일한 역할을 함
  * wait() 메소드에 의해 일시 정지된 **모든** 스레드들을 실행 대기 상태로 만듦
    * notify()는 **한 개**
* 해당 메소드들은 모두 Object 클래스에 선언된 메소드이므로 모든 공유 객체에서 호출 가능
* **동기화 메소드 또는 동기화 블록 내에서만 사용 가능**

#### stop 플래그, interrupt()

* 스레드 안전 종료 메소드

* 스레드는 기본적으로 자신의 run() 메소드가 모두 실행되면 자동 종료됨

* 경우에 따라 실행 중인 스레드를 즉시 종료할 필요가 있음

* Thread는 스레드를 즉시 종료시키기 위해 stop() 메소드 제공

  * Deprecated된 메소드
  * stop() 메소드로 스레드를 갑자기 종료하게 되면 스레드가 사용 중이던 자원들이 불안전한 상태로 남겨지기 때문임

* stop 플래그

  * run() 메소드가 정상적으로 종료되도록 유도하는 방법

  ```java
  public class PrintThread extends Thread {
  	private boolean stop;
  	
  	public void setStop(boolean stop) {
  	  this.stop = stop;
  	}
  	
  	public void run() {	
  		while(!stop) {
  			System.out.println("실행 중");
  		}	
  		System.out.println("자원 정리");
  		System.out.println("실행 종료");
  	}
  }
  ```

  ```java
  public class StopFlagExample {
  	public static void main(String[] args)  {
  		PrintThread1 printThread = new PrintThread1();
  		printThread.start();
  		
  		try {
  			Thread.sleep(1000);
  		} catch (InterruptedException e) {
  		}
  		
  		printThread.setStop(true);
  	}
  }
  ```

  ```text
  ...
  실행 중
  실행 중
  실행 중
  실행 중
  실행 중
  자원 정리
  실행 종료
  ```



* interrupt() 메소드

  * 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외 발생시키는 역할
  * interrupt() 메소드 이용 시 run() 메소드 정상 종료시킬 수 있음
  * 주의할 점
    * 스레드가 실행 대기 또는 실행 상태에 있을 때 interrupt() 메소드 실행되면 즉시 InterruptedException 예외 발생하지 않음
    * 스레드가 미래에 일시 정지 상태가 되면 예외 발생
    * 다시 말해서, 스레드가 일시 정지 상태가 되지 않으면 해당 메소드 호출은 의미 없음

  ```java
  public class InterruptExample {
  	public static void main(String[] args)  {
  		Thread thread = new PrintThread2();
  		thread.start();
  		try {
  			Thread.sleep(1000);
  		} catch (InterruptedException e) {
  		}
  		// 스레드 종료시키기 위해 InterruptedException 예외 발생 시킴
  		thread.interrupt();
  	}
  }
  ```

  ```java
  public class PrintThread extends Thread {
  	public void run() {	
  		try {
  			while(true) {
  				System.out.println("실행 중");
                  
                  //InterruptedException 예외 발생
  				Thread.sleep(1);
  			}	
  		} catch(InterruptedException e) {		
  		}
  	}
  }
  ```

  

  * 일시 정지 없이 interrupt() 호출 여부 확인하기
    * 아래의 두 메소드 중 어떤 것을 사용해도 무관함
    * interrupted()
      * 정적 메소드
      * 현재 스레드가 interrupted되었는지 확인
      * true 리턴함
    * isInterrupted()
      * 인스턴스 메소드
      * 현재 스레드가 interrupted되었는지 확인
      * true 리턴함

  ```java
  public class PrintThread extends Thread {
  	public void run() {	
  		while(true) {
  			System.out.println("실행 중");
  			if(Thread.interrupted()) {
  				break; // while문 빠져나옴
  			}
  		}
  		
  		System.out.println("자원 정리");
  		System.out.println("실행 종료");
  	}
  }
  ```



### 데몬 스레드

* 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드

* 주 스레드 종료 시 강제 자동 종료됨

  * 주 스레드의 보조 역할을 수행하기 때문에 주 스레드 종료 시 데몬 스레드의 존재 의미 사라짐

* 강제 종료되는 점을 제외하면 일반 스레드와 크게 차이 없음

* 데몬 스레드 적용 예

  * 워드프로세서의 자동 저장
  * 미디어 플레이어의 동영상 및 음악 재생
  * 가비지 컬렉터 등

* 데몬 스레드 생성 방법

  * 주 스레드가 데몬이 될 스레드의 setDaemon(true) 호출

  ```java
  public static void main(String[] args){
      //메인 스레드 == 주 스레드
      //AutoSaveThread == 데몬 스레드
      AutoSaveThread thread = new AutoSaveThread();
      thread.setDaemon(true);
      thread.start();
      ...
  }
  ```

  

  * 주의할 점
    * start() 호출된 후 setDaemon(true) 호출 시 IllegalThreadStateException 발생

* isDaemon()

  * 현재 실행 중인 스레드가 데몬인지 아닌지 구별하는 방법
  * 데몬 스레드일 경우 true 리턴함



### 스레드 그룹

* 관련된 스레드를 묶어 관리할 목적으로 이용
* JVM 실행 ▶ system 스레드 그룹 생성 ▶ JVM 운영에 필요한 스레드들 생성 ▶ system 스레드 그룹에 포함 시킴 ▶ system의 하위 스레드 그룹으로 main 생성 ▶ 메인 스레드를 main 스레드 그룹에 포함 시킴
* 스레드는 **반드시 하나의 스레드 그룹에 포함**
* 명시적으로 스레드 그룹에 포함시키지 않으면 기본적으로 자신을 생성한 스레드와 같은 스레드 그룹에 속함
* 스레드 그룹화의 이점
  * 스레드 그룹에서 제공하는 interrupt() 메소드 이용 시 그룹 내 포함된 모든 스레드를 일괄 interrupt
  * 스레드 그룹의 interrupt() 메소드는 포함된 모든 스레드의 interrupt() 메소드를 내부적으로 호출해줌
    * 스레드 그룹의 interrupt() 메소드는 소속된 스레드의 interrupt() 메소드만 호출함
    * 개별 스레드에서 발생하는 InterruptedException에 대한 예외 처리는 하지 않음
    * 안전한 종료를 위해 개별 스레드가 예외 처리 수행해야 함
* 스레드 그룹 이름 얻기

```java
ThreadGroup group = Thread.currentThread().getThreadGroup();
String groupName = group.getName();
```



* 프로세스 내에서 실행하는 모든 스레드 정보 얻기

```java
Map<Thread, StackTraceElement[]> map = Thread.getAllStackTraces();
```



* 스레드 그룹 생성

  ```java
  ThreadGroup tg = new ThreadGroup(String name);
  ThreadGroup tg = new ThreadGroup(ThreadGroup parent, String name);
  ```

  * 두 생성자 중 하나 이용해서 생성
  * 부모 스레드 그룹 지정하지 않으면 현재 스레드가 속한 그룹의 하위 그룹으로 생성됨
  * 새로운 스레드 그룹 생성 후, 이 그룹에 스레드 포함시키려면 Thread 객체 생성 시 생성자 매개값으로 스레드 그룹 지정



### 스레드 풀





