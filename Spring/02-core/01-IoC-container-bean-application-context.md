# Spring Framework Core

* [백기선, 스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)

* [Spring Framework Repo](https://github.com/spring-projects/spring-framework)

* [Source Repo](https://github.com/weekyeon/spring-framework-core)



# IoC

* Inversion of Control
* 의존 관계 주입
* 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는 게 아니라 **주입 받아 사용**하는 방법



# IoC 컨테이너

* BeanFactory
  * 스프링 IoC 컨테이너의 가장 최상위에 있는 인터페이스
  * 해당 인터페이스가 스프링 IoC 컨테이너의 핵심
  * [docs, Interface BeanFactory](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)
* 애플리케이션 컴포넌트의 중앙 저장소
* 빈 설정 소스로부터 빈 정의를 읽고, 빈을 구성하고 제공한다.
  * 빈 설정 파일 필요



# Bean

* 스프링 IoC 컨테이너가 관리하는 객체

  * 다시 말해서, 컨테이너 안에 있는 객체들을 Bean이라고 한다.

* **왜** Bean을 사용하는가?

  * 의존성 주입을 받기 위함

  * Scope

    * 싱글톤
      * 객체를 하나만 만들어서 사용하는 것
      * 런타임 시 성능 측면에서 효율적

    > cf) 프로토타입 Scope
    >
    > * 매번 다른 객체를 만들어서 사용하는 것

  * 라이프사이클 인터페이스

    * 스프링 IoC 컨테이너에 등록된 빈에만 국한됨
    * 빈이 만들어졌을 때 무언가 추가 작업이 하고 싶다!



# ApplicationContext

* 빈 설정

  * 빈 명세서
  * 빈에 대한 정의가 있다
    * name, class, scope, constructor, setter ...

* [docs, Interface ApplicationContext](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)

* 스프링 부트의 @SpringBootApplication 애노테이션 자체가 빈 설정 파일이다.

* ApplicationContext 설정 방법

  * ClassPathXmlApplicationContext (XML)

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        
    <!-- 
    ======== 고전적인 xml 빈 설정 파일 ========
    -->
        <bean id="bookService"
              class="com.example.springframeworkcore.BookService">
            <property name="bookRepository" ref="bookRepeository" />
        </bean>
        
    <!--
    이 상태로 등록하면, BookService가 BookRepository를 주입받지 못한다.
    이 코드는 단순히 BookService를 만들고 끝인 것.
    주입을 하려면 BookService Property에 BookRepository를 주입해주어야 한다.
    **ref의 값이 bean id 값이어야 한다.**
    -->
        <bean id="bookRepository"
              class="com.example.springframeworkcore.BookRepository" />
    
    </beans>
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        
    <!--
    ======== Component Scan 방법 ========
    고전적인 xml 빈 설정 방법을 사용하면 굉장히 번거로워 등장한 방법
    해당 패키지부터 빈을 스캔해서 등록하겠다는 의미이다.
    * 빈 스캔 시 사용하는 애노테이션 : @Component
    * 컴포넌트 애노테이션을 확장한 몇 가지 애노테이션 : @Service, @Repository
    -->
        <context:component-scan base-package="com.example.springframeworkcore" />
    
    </beans>
    ```

  * AnnotationConfigApplicationContext (Java)

    ```java
    @Configuration
    @ComponentScan(basePackageClasses = SpringFrameworkCoreApplication.class)
    public class ApplicationConfig {
    
        @Bean
        public BookRepository bookRepository(){
            return new BookRepository();
        }
    
        @Bean
        public BookService bookService(){
            BookService bookService = new BookService();
            //Setter가 있으니, 의존성 주입을 직적 해줄 수 있음
            bookService.setBookRepository(bookRepository());
            return bookService;
        }
    
    }
    ```

    