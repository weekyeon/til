# 퍼사드 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [삐멜 소프트웨어 엔지니어, 퍼사드 패턴(Facade Pattern)](https://imasoftwareengineer.tistory.com/29)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 퍼사드 패턴
* 실습 1



### 퍼사드 패턴

![기본 설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/facade1.png)



* *Facade, 건물의 전면(외관)*
* 서브 시스템을 편리하게 사용하기 위해 고수준의 인터페이스를 제공하는 패턴
* 복잡하고 다양한 내부 구조(서브 시스템)와 클라이언트 사이에 거대한 외벽(퍼사드)을 세워 편리한 인터페이스를 제공
* Facade Class
  * 서브 시스템을 감싸고 있는 고수준의 인터페이스
  * 클라이언트에게 단순한 메소드를 제공하여 클라이언트가 간단하게 사용할 수 있도록 함
  * 클라이언트의 요청이 발생하면 서브 시스템에 요청 전달
* Sub System
  * 퍼사드 클래스로부터 들어온 요청을 받아 처리하는 작업 수행
  * 퍼사드 클래스의 정보를 알 필요가 없음
* Client
  * 서브 시스템의 존재를 모름
  * 퍼사드 클래스만 알기 때문에, 퍼사드 클래스에만 접근 가능
  * 즉, 클라이언트는 서브 시스템으로부터 분리되어 의존성 감소
* Third Party API 같은 외부 라이브러리 추상화 시 사용됨
* 퍼사드 클래스의 장점
  * S/W 라이브러리를 쉽게 사용할 수 있게 해줌
    * 개발 유연성 향상
    * 라이브러리 밖의 코드가 라이브러리 안의 코드에 의존하는 것을 감소시켜줌
  * 공통 작업에 대해 간편한 메소드 제공함



### 실습

```java
package study.system;

class SubSystem01 {
// default 접근제한
    public SubSystem01() {
        System.out.println(getClass().getSimpleName()+" 호출");
    }
    public void process(){
        System.out.println(getClass().getSimpleName()+" 프로세스 호출");
    }
}
```

```java
package study.system;

class SubSystem02 {
// default 접근제한
    public SubSystem02() {
        System.out.println(getClass().getSimpleName()+" 호출");
    }
    public void process(){
        System.out.println(getClass().getSimpleName()+" 프로세스 호출");
    }
}
```

```java
package study.system;

class SubSystem03 {
// default 접근제한
    public SubSystem03() {
        System.out.println(getClass().getSimpleName()+" 호출");
    }
    public void process(){
        System.out.println(getClass().getSimpleName()+" 프로세스 호출");
    }
}
```

```java
package study.system;

public class Facade {

    // 퍼사드 클래스는
    // 서브 시스템의 기능을 알고 있어야 함
    private SubSystem01 subSystem01;
    private SubSystem02 subSystem02;
    private SubSystem03 subSystem03;

    public Facade() {
        subSystem01 = new SubSystem01();
        subSystem02 = new SubSystem02();
        subSystem03 = new SubSystem03();
    }

    // 서브 시스템 프로세스 호출
    public void process(){
        subSystem01.process();
        subSystem02.process();
        subSystem03.process();
    }
}
```

```java
package study;

import study.system.Facade;

public class Main {

    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.process();
    }

}
```

```text
SubSystem01 호출
SubSystem02 호출
SubSystem03 호출
SubSystem01 프로세스 호출
SubSystem02 프로세스 호출
SubSystem03 프로세스 호출
```

