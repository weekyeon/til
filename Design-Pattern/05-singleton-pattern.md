# 싱글톤 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 객체
* 싱글톤 패턴
  * 하나의 인스턴스만 있도록 하기
* 실습



### 객체

* **객체** : 속성과 기능을 갖춘 것
* **클래스** : 속성과 기능을 정의한 것
* **인스턴스** : 속성과 기능을 가진 것 중 실제하는 것



### 싱글톤 패턴

* 하나의 인스턴스만 생성해야 할 객체를 위한 패턴
  * 싱글톤 인스턴스와 이 인스턴스를 return할 수 있는 메소드를 가지고 있음
  * 인스턴스는 한 개만 있어야 하기 때문에 static 접근제한자 사용
  * 외부에서 싱글톤 클래스를 생성하면 안되기 때문에 private 생성자 사용

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/singleton1.png)



### 실습

* 개발 중인 시스템에서 스피커에 접근할 수 있는 클래스를 만들어 주세요
  * 만약, 스피커에 접근할 수 있는 클래스가 한 개 이상인 경우, 스피커 볼륨을 높일 때 스피커를 사용하는 클래스마다 작업이 필요함
  * 이 경우, 개발의 복잡도가 증가하며 리소스를 많이 사용하게 됨

```java
public class SystemSpeaker {
    static private SystemSpeaker instance;
    private int volume;
    private SystemSpeaker(){
        volume = 5;
    }

    public static SystemSpeaker getInstance() {
        if(instance == null){
            instance = new SystemSpeaker();
            System.out.println("객체 생성 완료");
        }else{
            System.out.println("객체가 이미 생성됨");
        }
        return instance;
    }

    public int getVolume() {
        return volume;
    }

    public void setVolume(int volume) {
        this.volume = volume;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        SystemSpeaker systemSpeaker1 = SystemSpeaker.getInstance();
        SystemSpeaker systemSpeaker2 = SystemSpeaker.getInstance();

        System.out.println(systemSpeaker1.getVolume()+" / "+systemSpeaker1.toString());
        System.out.println(systemSpeaker2.getVolume()+" / "+systemSpeaker2.toString());

        systemSpeaker1.setVolume(11);
        System.out.println(systemSpeaker1.getVolume()+" / "+systemSpeaker1.toString());
        System.out.println(systemSpeaker2.getVolume()+" / "+systemSpeaker2.toString());

        systemSpeaker2.setVolume(22);
        System.out.println(systemSpeaker1.getVolume()+" / "+systemSpeaker1.toString());
        System.out.println(systemSpeaker2.getVolume()+" / "+systemSpeaker2.toString());
    }
}
```

```text
객체 생성 완료
객체가 이미 생성됨
5 / study.SystemSpeaker@1b6d3586
5 / study.SystemSpeaker@1b6d3586
11 / study.SystemSpeaker@1b6d3586
11 / study.SystemSpeaker@1b6d3586
22 / study.SystemSpeaker@1b6d3586
22 / study.SystemSpeaker@1b6d3586
```