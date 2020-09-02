# 상태 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 상태 패턴
* 실습



### 상태 패턴

* **상태** 자체를 객체화하여 각각의 상태에 따른 액션도 상태 객체 내부에 구현하는 패턴
* 즉, 상태를 캡슐화하여 객체로 만들고, 해당 객체를 참조해 상태에 따라 다르게 처리함
* EX)
  * 엘리베이터의 상태 : `1` 상승 `2` 하강 `3` 정지
    * 엘리베이터가 상승 상태일 땐 위로 올라감
    * 엘리베이터가 하강 상태일 땐 아래로 내려감
    * 엘리베이터가 정지 상태일 땐 위/아래 이동 불가
* 상태 패턴을 사용하지 않고 엘리베이터 객체를 구현하면 대게 조건문을 이용하여 구현함
  * 상태의 종류가 많아질 수록 조건문이 많아짐
  * 코드 가독성 저하
  * 유지보수 어려움
* 상태 패턴을 사용하면, 엘리베이터 객체는 상태를 검사하지 않고, 상태 객체의 액션만 실행시키면 됨
  * 새로운 상태가 추가되어도 코드 수정 최소화 가능
  * 유지보수 용이
* 상태 패턴은 상태 종류에 따라 클래스가 많아질 수 있고, 상태와 행동에 강력한 결합이 생김

* 엘리베이터의 상승/하강과 같이 동일한 계열의 동작을 객체 상태에 따라 다르게 처리해야 할 때 사용
* 전략 패턴과의 비교
  * 공통점
    * 두 패턴 모두 델리게이트를 이용함
  * 차이점
    * 전략 패턴은 **알고리즘**을 변경
      * 즉, 전략 A를 몇 번을 사용한다고 해도 전략 B로 상태가 바뀌지 않음
      * 전략 B로 변경은 외부에서 또는 사용자가 가능하게 함
    * 상태 패턴은 내부 상태에 따른 액션을 다르게 취함
      * 액션 또한 내부 상태에 영향을 줌
      * 외부에서는 내부 상태가 어떻게 되어있는지 확인할 필요가 없음
        * 그냥 현재 상태만 알면 됨



![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/state1.png)



* Context
  * 상태값을 가지고 있는 객체
* State
  * 상태를 나타내는 객체
  * 각 상태에 따른 action() 인터페이스 정의
* StateA / StateB (ConcreteState)
  * State를 상속 받아 구체적인 상태를 나타내는 객체
  * ConcreteState가 액션을 취하면, State에 영향을 줌



### 실습

* 상태 패턴을 이용해 스위치 ON/OFF 구현
* Light *(Context)*
* LightState *(State)*
* onState / offState *(ConcreteState)*



```java
public class Light{

    // Light(Context)는 상태값을 가지고 있어야 함
    protected LightState lightState;

    // offState(ConcreteState)
    private LightState offState = new LightState() {
        @Override
        public void on() {
            System.out.println("Light ON");
            lightState = onState;
        }

        @Override
        public void off() {
            System.out.println("Nothing");
        }
    };

    // onState(ConcreteState)
    private LightState onState = new LightState() {
        @Override
        public void on() {
            System.out.println("Nothing");
        }

        @Override
        public void off() {
            System.out.println("Light OFF");
            lightState = offState;
        }
    };

    // 상태 초기값 설정
    public Light() {
        lightState = offState;
    }

    // Light action()
    public void on(){
        lightState.on();
    }

    // Light action()
    public void off(){
        lightState.off();
    }
}

// LightState(State)
interface LightState{
    void on();
    void off();
}
```

```java
public class Main {
    public static void main(String[] args) {
        Light light = new Light();

        light.off();
        light.off();
        light.off();

        light.on();
        light.on();
        light.on();

        light.off();
        light.on();
        light.off();
        light.on();
        light.off();
        light.on();
    }
}
```

```text
Nothing
Nothing
Nothing
Light ON
Nothing
Nothing
Light OFF
Light ON
Light OFF
Light ON
Light OFF
Light ON
```



