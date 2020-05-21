# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# IoC, 제어의 역전

* Inversion of Control

* 일반적으로 자신(객체)이 사용할 의존성은 자신이 만들어서 사용한다.

  ```java
  class OwnerController{
      private OwnerRepository ownerRepository = new OwnerRepository();
  }
  ```

* IoC는 의존성이 자신에게 있지 않고 **외부에서 의존성 주입**을 해주는 것을 말한다.

  ```java
  class OwnerController{
      
      //ownerRepository를 사용하지만, 객체를 이 곳에서 만들진 않음
      private OwnerRepository ownerRepository;
      
      //OwnerController 밖에서 의존성 주입
      public OwnerController(OwnerRepository ownerRepository){
          this.ownerRepository = ownerRepository;
      }
      
  }
  ```

* 참고 자료

  * [Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)