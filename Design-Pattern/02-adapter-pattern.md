# 어댑터 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 어댑터 패턴 (Adapter Pattern)
* 실습
* 유지보수
* 정리



### 어댑터 패턴 (Adapter Pattern)

* 사전적 의미 : 기계/기구 등을 다목적으로 사용하기 위한 부가 기구

* 연관성 없는 두 객체를 묶어 사용할 수 있음

* 기본 설계

  * 이미 주어진 알고리즘을 Adapter 패턴을 통해 원하는 기능으로 변경

  ![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/adapter1.png)



### 실습

* 두 수에 대한 다음 연산을 수행하는 객체를 만들어 주세요.

  * 수의 두 배의 수를 반환 : twiceOf(Float):Float
  * 수의 반(1/2)의 수를 반환 : halfOf(Float):Float

* 구현 객체 이름은 'Adapter'로 해주세요.

* Math 클래스에서 두 배와 절반을 구하는 함수는 이미 구현되어 있습니다.

* 설계

  ![설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/adapter2.png)

* 요구사항의 데이터 타입은 Float형, 이미 구현되어 있는 Math 클래스의 데이터 타입은 double형

  * 즉, Math 클래스의 메소드들을 직접 사용할 수 없음

* Adapter 패턴을 통해 Math 클래스의 메소드들을 Float형으로 변환하여 요구사항 해결 가능

```java
public class Math { //이미 구현되어 있는 Math 클래스
	//두 배
	public static double twoTime(double num){
		return num*2;
	}
	//절반
	public static double half(double num){
		return num/2;
	}
}
```

```java
public interface Adapter {
    //기능과 구현의 분리를 위해 Adapter 인터페이스 생성
    public Double twiceOf(Float num);
    public Double halfOf(Float num);
}
```

```java
public class AdapterImpl implements Adapter {
	//기능 구현
    @Override
    public Double twiceOf(Float num) {
        return Math.twoTime(num.doubleValue());
    }
    @Override
    public Double halfOf(Float num) {
        return Math.half(num.doubleValue());
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Adapter adapter = new AdapterImpl();

        System.out.println(adapter.twiceOf(100f));
        System.out.println(adapter.halfOf(80f));
    }
}
```



### 유지보수

* 알고리즘 변경을 원합니다.
  * Math 클래스에 새롭게 두 배를 구할 수 있는 함수가 추가 되었습니다.
  * 새로 구현된 알고리즘을 이용하도록 프로그램을 수정해주세요.
* 절반을 구하는 기능에서 로그를 찍는 기능을 추가해 주시기 바랍니다.

```java
public class Math {
	//두 배
	public static double twoTime(double num){
		return num*2;
	}
	//절반
	public static double half(double num){
		return num/2;
	}
	//새롭게 추가된 함수
	public static Double doubled(Double d){ 
        return d*2;
    }
}
```

```java
public class AdapterImpl implements Adapter {

    @Override
    public Double twiceOf(Float num) { 
        return Math.doubled(num.doubleValue()); 
    }

    @Override
    public Double halfOf(Float num) {
        //라이브러리 형태로 배포된 Math 클래스에 Log를 찍는 것보다
        //구현체인 AdapterImpl에 찍는 것이 더 좋음
        System.out.println("Log:::: 절반 함수 호출");
        return Math.half(num.doubleValue());
    }
}
```

* AdapterImpl 구현체 클래스 수정만으로 새로운 요구사항 반영 가능



### 정리

* Adapter 패턴은 이미 구현되어 있는 알고리즘을 요구사항에 맞춰 사용할 수 있음

* 실습을 예로 들면,

  * Math 클래스에 정의된 알고리즘은 Double형 자료이기 때문에 직접 호출 할 수 없음
  * Adapter 패턴을 이용하여 Double형 자료를 Float형 자료로 변환하여 Math 알고리즘 사용

* 자바를 예로 들면,

  * 'ArrayList를 버블소트로 소팅한 결과를 갖고 싶다' 라는 요구사항이 있을 때
  * 무수히 많은 버블소트 알고리즘 중 하나를 가져왔는데, 이 알고리즘이 List 형태이다.
  * 그때, Adapter 패턴을 이용하여 List를 ArrayList로 바꾸어 소트하고 원하는 결과를 얻음

  > List -> ArrayList --- Sort --- ArrayList -> List