# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# Bean

### 정의

* 스프링 IoC 컨테이너가 관리하는 객체
* 스프링 IoC 컨테이너가 관리하지 않는 객체는 **빈이 아니다**.
    * 즉, 빈이 아닌 객체는 의존성 주입이 **불가능**하다.

### 스프링 IoC 컨테이너 안에 빈을 만드는 방법

* Component Scanning

  * @Component
    * @Repository
    * @Service
    * @Controller
      * @Controller는 Component 라는 메타 애노테이션을 사용한 애노테이션이다.
    * @Configuration

* 직접 XML 파일이나 Java 설정 파일에 등록

  * 해당 방법을 사용하면, 애노테이션을 일일이 붙이지 않아도 된다.

  ```java
  @Configuration
  public class SampleConfig{
      
      @Bean
      public SampleController sampleController(){
          return new SampleController();
      }
      
  }
  ```


> * LifeCycle Callback
>   * 애노테이션 프로세서 중, 스프링 IoC 컨테이너가 사용하는 여러가지 인터페이스를 말한다.
>   * 여러 라이프싸이클 콜백 중의 하나인 @ComponentScan @Component 애노테이션을 가진 모든 클래스를 찾고 그 클래스의 인스턴스를 만들어 빈으로 등록하는 애노테이션 프로세서가 등록되어 있다.
>   * @ComponentScan
>     * 여러 라이프싸이클 콜백 중 하나인 인터페이스
>     * @SpringBootApplication 안에 존재한다.
>     * @Component를 가진 모든 클래스를 찾고, 그 클래스의 인스턴스를 만들어 빈으로 등록하는 애노테이션 프로세서가 등록되어 있다.
>     * 즉, @ComponentScan은 어디부터 컴포넌트를 찾아보라고 알려주는 역할을 하며
>     * @SpringBootApplication이 붙은 클래스부터 모든 하위 클래스까지 스캔하여 @Component 클래스를 찾는다.
>     * 위의 과정으로 인해, 개발자가 직접 빈으로 등록하지 않아도 스프링이 알아서 해주는 것이다.

### 스프링 IoC 컨테이너 안에서 빈을 사용하는 방법

* @Autowired
* @Inject
* ApplicationContext getBean()