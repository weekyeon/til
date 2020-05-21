# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# DI, 의존성 주입

### @Autowired

* 스프링 버전 4.3부터 @Autowired 생략 가능

  * 어떠한 클래스에 생성자가 하나뿐이고, 그 생성자로 주입 받는 레퍼런스 변수들이 빈으로 등록되어 있다면 해당 빈을 자동으로 주입해주는 기능이 있기 때문이다.

* 생성자

  ```JAVA
  @Controller
  class OwnerController{
      
      private final PetRepository petRepository;
      public OwnerController(PetRepository petRepository){
          this.petRepository = petRepository;
      }
      
  }
  ```

  * final 키워드를 사용하는 이유
    * 의존성 주입을 한 번 받은 다음 다른 레퍼런스로 바뀌지 않게끔 보장하기 위함이다.
  * 스프링 프레임워크 레퍼런스에서 권장하는 방법
  * 생성자 사용 시 장점
    * 필수로 사용해야 하는 레퍼런스 없이는 인스턴스를 만들지 못하도록 강제할 수 있다.
      * EX) OwnerController는 OwnerRepository가 없으면 제대로 동작할 수 없는 클래스이다.
      * 즉, OwnerRepository가 반드시 필요한데, 이때 @Autowired를 이용하여 강제 가능
  * 생성자 사용 시 단점
    * 순환참조 발생
      * EX) A가 B를 참조하고 B가 A를 참조하는 경우,
      * 둘 다 인스턴스를 만들지 못해 애플리케이션이 제대로 동작하지 않는다.
      * 이런 경우 필드나 세터 인젝션을 사용하면 되지만, 되도록 순환참조가 되지 않도록 생성자 주입을 하는 게 좋다.

* 필드

  ```java
  @Controller
  class OwnerController{
      
      @Autowired
      private PetRepository petRepository;
      
  }
  ```

  * final 키워드를 사용하면 안 되는 이유
    * PetRepository 인스턴스를 무조건 OwnerController에 생성해야 하기 때문이다.

* Setter

  ```java
  @Controller
  class OwnerController{
      
      private  PetRepository petRepository;
      @Autowired
      public void setPetRepository(PetRepository petRepository){
          this.petRepository = petRepository;
      }
      
  }
  ```

  * final 키워드를 사용하면 안 되는 이유
    * PetRepository 인스턴스를 무조건 OwnerController에 생성해야 하기 때문이다.