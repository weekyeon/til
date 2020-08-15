# IoC 컨테이너 1

* [백기선, 스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)
* [Source Repo](https://github.com/weekyeon/spring-framework-core)



### 목차

* 스프링 IoC 컨테이너와 빈
* ApplicationContext와 다양한 빈 설정 방법
* @Autowire
* @Component와 @ComponentScan
* 빈의 스코프



### 스프링 IoC 컨테이너와 빈

* IoC

  * Inversion of Control
  * 의존 관계 주입(Dependency Injection)
  * 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는 게 아니라, **주입 받아 사용하는 방법**
  * 스프링이 없어도 장치만 마련되어 있으면 사용 가능

* 스프링 IoC 컨테이너

  * [BeanFactory](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)
    * 스프링 IoC 컨테이너의 가장 최상위에 있는 인터페이스
    * 스프링 IoC 컨테이너의 핵심
  * 애플리케이션 컴포넌트의 중앙 저장소
  * 빈 설정 소스로부터 빈 정의를 읽고, 빈을 구성하고 제공
  * 즉, 스프링 IoC 컨테이너의 역할은 아래와 같음
    * `1` 빈 인스턴스 생성
    * `2` 의존 관계 설정
    * `3` 빈 제공

* Bean

  * 스프링 IoC 컨테이너가 관리하는 객체
  * 장점
    * 의존성 주입을 받기 위함
    * 스코프
      * 싱글톤
        * 객체를 하나만 만들어서 사용하는 것
        * 런타임 시 성능 측면에서 효율적
      * 프로토타입
        * 매번 다른 객체를 만들어서 사용하는 것
    * 라이프사이클 인터페이스 지원
      * 스프링 IoC 컨테이너에 등록된 빈에만 국한됨
      * 빈이 만들어졌을 때 어떤 작업을 하고 싶다!
        * @PostConstruct 등

* 간단한 예제

  ```java
  @Repository
  public class BookRepository{
      public Book save(Book book){
          return null;
      }
  }
  ```

  ```java
  @Service
  public class BookService{
      
      //BookService 객체가 BookRepository 객체를 직접 만들어 사용하지 않고
      //생성자에서 BookRepository 객체를 주입 받음(IoC)
      private BookRepository bookRepository;
      
      public BookService(BookRepository bookRepository){
          this.bookRepository = bookRepository;
      }
      
      @PostConstruct
      public void postConstruct(){
          System.out.println("빈 생성 시 추가로 수행하는 작업");
      }
      
  }
  ```

  

### ApplicationContext와 다양한 빈 설정 방법

* [ApplicationContext](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)

  * 실질적으로 많이 사용하는 BeanFactory
  * BeanFactory를 상속 받음
  * 추가로 아래의 기능 등을 갖고 있음
    * 이벤트 발행 기능(ApplicationEventPublisher)
    * 리소스 로딩 기능(ResourceLoader)
    * 메시지 소스 처리 기능 (MessageSource)
      * i18n
      * 메시지 다국화

* 스프링 ApplicationContext을 이용한 빈 설정

  * **ClassPathXmlApplicationContext (XML)**

    * **현재 흔히 사용하는 방법은 아니기 때문에 가볍게 알고 넘어가자.**
    * `1` 고전적인 XML 빈 설정 파일
      * BookService 객체에 BookRepository를 주입 받기 위해서는 property 설정 필요
      * `<property name="bookRepository" ref="bookRepository" />`
        * `name` : BookRepository 클래스의 Setter에서 가져온 이름
        * `ref` : bookRepository라는 id를 가진 다른 빈을 참조한다는 의미

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        
        <bean id="bookService"
              class="spring.framework.core.BookService">
            <property name="bookRepository" ref="bookRepository" />
        </bean>
    
        <bean id="bookRepository"
              class="spring.framework.core.BookRepository" />
    
    </beans>
    ```

    ```java
    public class BookService {
        
        private BookRepository bookRepository;
        
        public void setBookRepository(BookRepository bookRepository) {
           this.bookRepository = bookRepository;
        }
    }
    ```

    ```java
    public class DemoApplication {
        
        public static void main(String[] args) {
            // ApplicationContext를 이용하여 설정한 빈 사용하기
            ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
            String[] beanDefinitionNames = context.getBeanDefinitionNames();
            System.out.println(Arrays.toString(beanDefinitionNames));
            BookService bookService = (BookService) context.getBean("bookService");
            System.out.println(bookService.bookRepository != null);
        }
    }
    ```

    * `2` component-scan
      * XML로 빈을 일일이 등록하면 굉장히 번거로운데, 이를 개선하기 위해 등장
      * 해당 패키지부터 빈을 스캔해서 등록함
      * 빈 스캔 시 사용하는 애노테이션
        * @Component, @Service, @Repository..

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        
        <context:component-scan base-package="spring.framework.core" />
    
    </beans>
    ```

    ```java
    @Service
    public class BookService {
    
        private BookRepository bookRepository;
    
        public void setBookRepository(BookRepository bookRepository) {
           this.bookRepository = bookRepository;
        }
    }
    ```

    ```java
    @Respository
    public class BookRepository {
    }
    ```

  * **AnnotationConfigApplicationContext (Java)**

    * Java를 이용한 빈 설정 방법
      * @Configuration
      * XML과 비교해 굉장히 유연함

    ```java
    @Configuration
    public class ApplicationConfig {
    
        //빈 설정
        	//Bean id = bookRepository() 메소드
        	//Bean type = BookRepository 객체
        	//실제 객체 = new BookRepository();
        @Bean
        public BookRepository bookRepository(){
            return new BookRepository();
        }
    
        //주입 방법 1. 의존성 주입에 필요한 메소드 직접 호출
        @Bean
        public BookService bookService(){
            BookService bookService = new BookService();
            bookService.setBookRepository(bookRepository());
            return bookService;
        }
        
        //주입 방법 2. 메소드 파라미터로 의존성 주입 받기
        @Bean
        public BookService bookService(BookRepository bookRepository){
            BookService bookService = new BookService();
            bookService.setBookRepository(bookRepository);
            return bookService;
        }
    
    }
    ```

    ```java
    public class DemoApplication {
        
        public static void main(String[] args) {
            ApplicationContext context = new AnnotationConfigApplicationContext(ApplicationConfig.class);
            String[] beanDefinitionNames = context.getBeanDefinitionNames();
            System.out.println(Arrays.toString(beanDefinitionNames));
            BookService bookService = (BookService) context.getBean("bookService");
            System.out.println(bookService.bookRepository != null);
        }
    }
    ```

    * @ComponentScan 사용
      * Java Config 방법 또한 번거로워 등장한 방법
      * @ComponentScan 대상 지정은 `1` basePackages와 `2` basePackageClasses
        * 클래스 기준이 조금 더 타입 세이프함

    ```java
    @Configuration
    @ComponentScan(basePackageClasses = DemoApplication.class)
    public class ApplicationConfig {
    }
    ```

  * **Spring Boot**에서는 @SpringBootApplication 애노테이션이 다 해줌

    * 해당 애노테이션이 붙은 파일이 사실상 빈 설정 파일
    * 별도의 ApplicationConfig가 필요없음
    * @SpringBootApplication
      * @SpringBootConfiguration, @Configuration, @ComponentScan 다 있음

    ```java
    @SpringBootApplication
    public class DemoApplication {
        public static void main(String[] args) {
                
        }
    }
    ```

    

### @Autowire

* 필요한 의존 객체의 **타입**에 해당하는 빈을 찾아 주입함

* @Autowired 사용 위치

  * 생성자

  ```java
  @Repository
  public class BookRepository {
      
  }
  ```

  ```java
  @Service
  public class BookService {
      BookRepository bookRepository;
      
      @Autowired
      public BookService(BookRepository bookRepository) {
          this.bookRepository = bookRepository;
      }
  }
  ```

  * Setter

  ```java
  @Service
  public class BookService {
      BookRepository bookRepository;
  
      @Autowired
      public void setBookRepository(BookRepository bookRepository) {
          this.bookRepository = bookRepository;
      }
  }
  ```

  * 필드

  ```java
  @Service
  public class BookService {
      @Autowired
      BookRepository bookRepository;
  }
  ```

* @Autowired 한계점 1) **해당 타입의 빈이 없거나 한 개인 경우**

  * 생성자
    * 적절한 빈 설정을 하지 않으면, 빈 생성 시 빈에 필요한 의존성 객체를 찾지 못했다는 직관적인 오류 발생
    * EX) BookRepository에 @Repository 설정을 하지 않은 경우
  * Setter
    * 적절한 빈 설정을 하지 않으면 오류가 발생하지만, 생성자에서 발생하는 오류와의 차이점이 있음
      * Setter이기 때문에 BookService 자체의 인스턴스는 만들 수 있지만, @Autowired가 있기 때문에 의존성 주입을 시도하려다 실패함
      * 위 같은 경우, 만약 의존성이 필수가 아닌 옵션이라면 `@Autowired(required = false)` 설정
  * 필드
    * Setter, 필드 VS 생성자
      * 생성자를 사용한 의존성 주입은 빈을 만들 때, 생성자가 전달받아야 하는 빈이 없으면 무조건 인스턴스를 생성하지 못하며, 결국 BookService도 등록할 수 없음
      * 하지만, Setter 또는 필드를 사용할 땐 `required` 설정을 이용해 BookService가 받아야 하는 의존성이 없어도 빈으로 등록될 수 있음

* @Autowired 한계점 2) **해당 타입의 빈이 여러 개인 경우**

  * 어떤 빈을 주입해야 하는지 스프링에서 모르기 때문에 오류 발생

  ```java
  public interface BookRepository{
      
  }
  ```

  ```java
  @Repository
  public class MyBookRepository implements BookRepository{
      
  }
  ```

  ```java
  @Repository
  public class YourBookRepository implements BookRepository{
      
  }
  ```

  ```java
  @Service
  public class BookService{
      @Autowired
      BookRepository bookRepository;
  }
  ```

  * 해결 방안
    * `1` 사용하고 싶은 빈에 @Primary 설정하여 주입 받음
    * `2` @Qualifier("빈 id") 사용하여 주입 받음
    * `3` 모든 빈을 주입 받음
  * @Primary
    * 필드의 이름을 변경하는 것보다 해당 애노테이션 사용 권장

  ```java
  @Repository @Primary
  public class MyBookRepository implements BookRepository {
      
  }
  ```

  * @Qualifier

  ```java
  @Service
  public class BookService {
      @Autowired @Qualifier("myBookRepository")
      BookRepository bookRepository;
  }
  ```

  * 모든 빈을 주입 받음

  ```java
  @Service
  public class BookService{
      @Autowired
      List<BookRepository> bookRepositories;
      
      public void printBookRepository(){
          this.bookRepositories.forEach(System.out::println);
      }
  }
  ```

* @Autowire 동작 원리

  * [BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html) 라이프사이클 인터페이스의 구현체에 의해 동작
    * BeanPostProcessor란
      * 새로 만든 빈 인스턴스를 수정할 수 있는 라이프 사이클 인터페이스
      * 빈의 인스턴스를 만든 다음에 빈 초기화(Initialization) 라이프 사이클이 또 있는데, 그 라이프 사이클 **이전/이후에 어떤 부가적인 작업을 할 수 있는 또 다른 라이프 사이클 콜백**
      * postProcessBeforeInitialization()
      * postProcessAfterInitialization()
  * [AutowiredAnnotationBeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html) extends BeanPostProcessor
    * 스프링이 제공하는 @Autoried와 @Value 애노테이션 그리고 JSR-330의 @Inject 애노테이션을 지원하는 애노테이션 처리기
    * 빈이 Initialization 하기 전에 AutowiredAnnotationBeanPostProcessor에 의해 @Autowired를 처리함

* @BeanPostProcessor 동작 원리

  * BeanFactory가 자신 안에 등록되어 있는 BeanPostProcessor 타입의 빈을 찾음
    * **이 중, AutowiredAnnotationBeanPostProcessor가 빈으로 등록되어 있는 것**
  * BeanPostProcessor가 다른 일반적인 빈들에게 AutowiredAnnotationBeanPostProcessor에 있는 어노테이션을 적용하는 로직 수행(아주 마법같은일..)

  ```java
  @Component
  public class MyRunner implements ApplicationRunner {
    
    @Autowired
    ApplicationContext applicationContext;
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
      AutowiredAnnotationBeanPostProcessor bean =
        applicationContext.getBean(AutowiredAnnotationBeanPostProcessor.class)
        System.out.println(bean);
    }
  }
  ```

* 표준 빈 라이프 사이클 초기화 메소드 순서

```text
Bean factory implementations should support the standard bean lifecycle interfaces as far as possible.
The full set of initialization methods and their standard order is:

1. BeanNameAware's setBeanName
2. BeanClassLoaderAware's setBeanClassLoader
3. BeanFactoryAware's setBeanFactory
4. EnvironmentAware's setEnvironment
5. EmbeddedValueResolverAware's setEmbeddedValueResolver
6. ResourceLoaderAware's setResourceLoader (only applicable when running in an application context)
7. ApplicationEventPublisherAware's setApplicationEventPublisher (only applicable when running in 8. an application context)
9. MessageSourceAware's setMessageSource (only applicable when running in an application context)
ApplicationContextAware's setApplicationContext (only applicable when running in an application context)
10. ServletContextAware's setServletContext (only applicable when running in a web application context)
11. postProcessBeforeInitialization methods of BeanPostProcessors
12. InitializingBean's afterPropertiesSet
13. a custom init-method definition
14. postProcessAfterInitialization methods of BeanPostProcessors

On shutdown of a bean factory, the following lifecycle methods apply:

1. ostProcessBeforeDestruction methods of DestructionAwareBeanPostProcessors
2. DisposableBean's destroy
3. a custom destroy-method definition
```


