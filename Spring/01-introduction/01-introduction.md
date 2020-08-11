# 예제로 배우는 스프링 입문 (개정판)

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)
* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)
* [Source Repo](https://github.com/weekyeon/spring-petclinic)



### 목차

* 스프링 IoC
* 스프링 IoC 컨테이너
* 스프링 빈(Bean)
* 의존성 주입(DI)
* 스프링 AOP
* 스프링 PSA
* 스프링 웹 MVC
* 스프링 트랜잭션
* 스프링 캐시



### 스프링 IoC

* 제어의 역전, Inversion of Control

* 일반적으로 자신(객체)이 사용할 의존성은 자신이 만들어 사용

  ```java
  class OwnerController{
      
      //일반적인 경우
      private OwnerRepository ownerRepository = new OwnerRepository();
      
  }
  ```

* IoC는 의존성이 자신에게 있지 않고 외부에서 의존성 주입을 해주는 것을 말함

  ```java
  class OwnerController{
      
      //ownerRepository를 사용하지만, 객체를 OwnerController에서 만들진 않음
      private OwnerRepository ownerRepository;
      
      //OwnerController 밖에서 의존성 주입(DI)
      public OwnerController(OwnerRepository ownerRepository){
          this.ownerRepository = ownerRepository;
      }
      
  }
  ```

* 참고 자료
  * [Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)



### 스프링 IoC 컨테이너

* 빈(Bean)을 만들고, 빈 사이의 의존성을 엮어주며, 컨테이너가 가지고 있는 빈들을 제공해주는 역할
* 모든 클래스가 빈으로 등록되어 있는 것은 아님
* 무엇을 보고 Bean인지 알 수 있을까?
  * class 왼쪽에 녹색 콩이 있으면 Bean으로 등록되어 있는 것
  * Annotation @Bean을 이용해서 Bean으로 등록 가능
* 의존성 주입은 Bean끼리만 가능
  * 즉, 스프링 IoC 컨테이너 안에 있는 빈들끼리만 의존성 주입이 가능함
* ApplicationContext 안에 모든 빈이 있음
  * 즉, ApplicationContext가 해당 타입의 Bean을 찾아서 의존성 주입 수행
* 참고 자료
  * [Understanding Application Context](https://github.com/spring-guides/understanding/tree/master/application-context)
  * [docs, Interface ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)
  * [docs, Interface BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)



### 스프링 빈(Bean)

* 스프링 IoC 컨테이너가 관리하는 객체

* 스프링 IoC 컨테이너가 관리하지 않는 객체는 빈이 아님

  * 즉, 빈이 아닌 객체는 의존성 주입이 불가능

* 스프링 IoC 컨테이너 안에 빈을 반드는 방법

  * Component Scanning

    * @Component
      * @Repository
      * @Service
      * @Controller
        * @Controller는 Component라는 메타 애노테이션을 사용한 애노테이션
      * @Configuration

  * 직접 XML 파일이나 Java 설정 파일에 등록

    * 해당 방법을 사용하면, 애노테이션을 일일이 붙이지 않아도 됨

    ```java
    //Java 설정 파일을 이용하여 Bean 직접 등록하는 방법
    @Configuration
    public class SampleConfig{
        
        @Bean
        public SampleController sampleController(){
            return new SampleController();
        }
        //이렇게 Bean으로 직접 등록하면 애노테이션을 일일이 붙이지 않아도 된다.
    }
    ```

    

> * LifeCycle Callback
>   * 애노테이션 프로세서 중, 스프링 IoC 컨테이너가 사용하는 여러가지 인터페이스를 말한다.
>   * 여러 라이프싸이클 콜백 중 하나인 @ComponentScan @Component 애노테이션을 가진 모든 클래스를 찾고 그 클래스의 인스턴스를 만들어 빈으로 등록하는 애노테이션 프로세서가 등록되어 있다.
>   * @ComponentScan
>     * 여러 라이프싸이클 콜백 중 하나인 인터페이스
>     * @SpringBootApplication 안에 존재
>     * @Component를 가진 모든 클래스를 찾고, 그 클래스의 인스턴스를 만들어 빈으로 등록하는 애노테이션 프로세서가 등록되어 있음
>     * 즉, @ComponentScan은 어디부터 컴포넌트를 찾아보라고 알려주는 역할을 수행
>     * @SpringBootApplication이 붙은 클래스부터 모든 하위 클래스까지 스캔하여 @Component 클래스를 찾음
>     * 위의 과정으로 인해 개발자가 직접 빈으로 등록하지 않아도 스프링이 알아서 해주는 것



* 스프링 IoC 컨테이너 안에서 빈을 사용하는 방법
  * @Autowired
  * @Inject
  * ApplicationContext `getBean()`



### 의존성 주입(DI)

* @Autowired

  * @Autowired는 생성자, 필드, Setter에 붙일 수 있음

  * 스프링 버전 4.3부터 @Autowired 생략 가능

    * 어떠한 클래스에 생성자가 하나뿐이고, 그 생성자로 주입 받는 레퍼런스 변수들이 빈으로 등록되어 있다면 해당 빈을 자동으로 주입해주는 기능이 있기 때문

  * 생성자

    ```java
    @Controller
    class OwnerController{
        //@Autowired 생략
        private final PetRepository petRepository;
        
        public OwnerController(PetRepository petRepository){
            this.petRepository = petRepository;
        }
        
    }
    ```

    * 스프링 프레임워크 레퍼런스에서 권장하는 방법
      * 장점
        * 필수적으로 사용해야 하는 레퍼런스 없이는 인스턴스를 만들지 못하도록 강제할 수 있음
        * 예를 들어, OwnerController는 PetRepository가 없으면 제대로 동작할 수 없는 클래스임
        * 즉, PetRepository가 반드시 필요한데, 이때 @Autowired를 이용하여 강제할 수 있음
      * 단점
        * 순환참조 발생
          * A가 B를 참조, B가 A를 참조.. 둘 다 생성자 인젝션 사용한다고 가정했을 때, 둘 다 생성하지 못함.
          * 애플리케이션이 제대로 동작하지 않음
          * 이런 경우 필드나 세터 인젝션 사용
    * final 키워드 사용 이유
      * 의존성 주입을 한 번 받은 다음 다른 레퍼런스로 바뀌지 않게끔 보장하기 위함

  * 필드

    ```JAVA
    @Controller
    class OwnerController{
        
        @Autowired
    	private PetRepository petRepository;
        
    }
    ```

    * final 키워드를 사용하면 안 되는 이유
      * PetRepository 인스턴스를 무조건 OwnerController에 생성해야 하기 때문

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
      * PetRepository 인스턴스를 무조건 OwnerController에 생성해야 하기 때문



### 스프링 AOP

* 여러 곳에서 반복적으로 사용되는 동일한 코드를 한 곳으로 모으는 방법
* AOP 구현 방법
  * 컴파일
    * A.java ----(AOP)----> A.class
    * AspectJ 제공
  * 바이트코드 조작
    * A.java ---> A.class ----(AOP)----> 메모리
    * Class Loader가 클래스를 읽어와서 메모리에 올릴 때 조작
    * AspectJ 제공
  * 프록시 패턴 (Proxy Pattern)
    * 스프링 AOP가 사용하는 방법
    * 디자인 패턴 중 하나를 사용해서 AOP와 같은 효과를 냄
  * 참고 자료
    * [AspectJ](https://www.eclipse.org/aspectj/)
    * [Proxy Parttern](https://refactoring.guru/design-patterns/proxy)



### 스프링 PSA

* [Service Abstraction](https://en.wikipedia.org/wiki/Service_abstraction)

* 스프링 PetClinic은 서블릿 애플리케이션을 만들고 있음에도 서블릿을 전혀 쓰지 않음

  * @GetMapping, @PostMapping 등 애노테이션 사용
  * 이러한 코드의 기반에 서블릿 코드가 있음

  ```java
  public class OwnerCreateServelt extends HttpServlet{
      
      //서블릿을 사용하는 코드
      doGet~
      doPost~
      
  }
  ```



### 스프링 웹 MVC

* 스프링이 제공하는 스프링 웹 MVC 추상화 계층

  ```java
  //Controller
  @GetMapping("/owners/new")
  public String initCreationForm(Map<String, Object> model){
      //Model
      Owner owner = new Owner();
      model.put("owner", owner);
      //View
      return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
  }
  ```

* @Controller

  * 요청을 맵핑할 수 있는 Controller 역할을 수행하는 클래스
  * 해당 클래스 안에 @GetMapping, @PostMapping 등으로 요청을 맵핑함
    * **맵핑한다** : url에 해당하는 요청이 들어왔을 때, 그 요청을 메소드가 처리하게끔 맵핑한다.

* 왜 추상화 계층일까?

  * 편의성을 위해
  * 서블릿을 로우 레벨로 사용하지 않아도 됨(즉, 직접 쓰지 않아도 됨)



### 스프링 트랜잭션

* 데이터에 데이터를 넣을 때 A를 하고 B를 하고 C까지 해야 하나의 작업으로 완료되는 경우
* All or Nothing
* [Platform Transaction Manager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html)
* @Transactional
  * 스프링이 제공해주는 추상화 레벨
  * 해당 애노테이션이 붙어 있는 메소드에는 트랜잭션 처리가 됨
    * 즉, 명시적 코딩을 하지 않아도 됨



### 스프링 캐시

* [CacheManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/cache/CacheManager.html)
* @Cacheable