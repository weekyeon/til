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
  * BeanPostProcessor가 다른 일반적인 빈들에게 AutowiredAnnotationBeanPostProcessor에 있는 애노테이션을 적용하는 로직 수행(아주 마법같은일..)

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



### @Component와 @ComponentScan

* @ComponentScan **주요 기능**

  * 스캔할 위치를 선정함
    * 어디부터 어디까지
  * Filter
    * 어떤 애노테이션을 스캔할지 또는 하지 않을지

* @ComponentScan **대상**

  * @Component
    * @Repository
    * @Service
    * @Controller
    * @Configuration 등

* @ComponentScan 동작 원리

  * @ComponentScan은 스캔할 패키지와 애노테이션에 대한 정보
  * 실제 스캐닝은 [ConfigurationClassPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ConfigurationClassPostProcessor.html)라는 [BeanFactoryPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html)에 의해 처리됨
    * BeanFactoryPostProcessor
      * 우리가 직접 등록하는 빈들을 포함한 다른 빈들을 모두 등록하기 전에 컴포넌트 스캔을 해서 빈으로 등록해줌

* 펑션을 사용한 빈 등록하는 방법

  ```java
  public static void main(String[] args) {
   new SpringApplicationBuilder()
   .sources(Demospring51Application.class)
   .initializers((ApplicationContextInitializer<GenericApplicationContext>)
  applicationContext -> {
   applicationContext.registerBean(MyBean.class);
   })
   .run(args);
   }
  ```

  * ApplicationContext의 싱글톤 스코프 빈들은 초기에 다 생성함
    * 등록해야 할 빈이 많은 경우 초기 구동 시간이 오래 걸림
    * 하지만, 애플리케이션 구동 초기 1번만 구동 시간을 잡아먹음
  * 구동 시간에 예민하다면 펑션을 사용한 빈 등록 방법 사용 고려
    * Spring 5부터 지원
    * Application Initialize 때 컴포넌트 스캔 패키지 외부의 빈을 직접 등록 가능
    * 리플렉션과 프록시 기법을 사용하지 않아 성능(애플리케이션 구동 시간) 상의 이점이 있음
    * <u>@ComponentScan을 버리고 이 방법을 쓰면 더 불편하지 않을까?</u>
      * 이 방법은 @ComponentScan을 대체하는 방법이라기 보단, 직접 빈으로 등록하게 되는 빈들을 대체할 때 나쁘지 않을 것으로 생각



### 빈의 스코프

* ApplicationContext에 생성된 빈들은 전부 스코프가 있음 (Default : 싱글톤 스코프)

* 스코프

  * 싱글톤
    * 애플리케이션 전반에 걸쳐서 해당 빈의 인스턴스가 오직 한 개
    * 대부분의 상황에서는 싱글톤 스코프를 사용함

  * 프로토타입
    * 빈을 주입 받아올 때마다 새로운 빈 생성됨
    * 짧은 생명주기를 가진 빈들을 주입 받을 때 고려해야 함
    * Request, Session, WebSocket ...

  ```java
  @Component
  public class Single {
      @Autowired
      Proto proto;
  
      public Proto getProto() {
          return proto;
      }
  }
  ```

  ```java
  @Component @Scope("prototype")
  public class Proto {
  }
  ```

  ```java
  @Component
  public class AppRunner implements ApplicationRunner {
  
      @Autowired
      ApplicationContext ctx;
  
      @Override
      public void run(ApplicationArguments args) throws Exception {
  
          System.out.println("Single ==========");
          System.out.println(ctx.getBean(Single.class));
          System.out.println(ctx.getBean(Single.class));
          System.out.println(ctx.getBean(Single.class));
  
          System.out.println("Proto ==========");
          System.out.println(ctx.getBean(Proto.class));
          System.out.println(ctx.getBean(Proto.class));
          System.out.println(ctx.getBean(Proto.class));
  
      }
  }
  ```

  ```java
  @SpringBootApplication
  public class DemoApplication {
      public static void main(String[] args) {
          SpringApplication.run(DemoApplication.class, args);
      }
  }
  ```

  ```text
  Single ==========
  spring.framework.core.l05BeanScope.Single@44d70181
  spring.framework.core.l05BeanScope.Single@44d70181
  spring.framework.core.l05BeanScope.Single@44d70181
  Proto ==========
  spring.framework.core.l05BeanScope.Proto@742d4e15
  spring.framework.core.l05BeanScope.Proto@88a8218
  spring.framework.core.l05BeanScope.Proto@50b1f030
  ```

* 프로토타입 빈이 싱글톤 빈을 참조하면 **아무 문제 없음**

* 싱글톤 빈이 프로토타입 빈을 참조하면

  * 프로토타입 빈 업데이트 X
    * 싱글톤 스코프의 빈은 인스턴스가 **한 번만** 만들어지기 때문에
    * 한 번 설정된 프로토타입 스코프의 property가 변경되지 않고
    * 항상 동일한 객체를 사용하게 됨

  ```java
  @Component
  public class AppRunner implements ApplicationRunner {
  
      @Autowired
      ApplicationContext ctx;
  
      @Override
      public void run(ApplicationArguments args) throws Exception {
          System.out.println("싱글톤 빈이 프로토타입 빈 참조 ==========");
          System.out.println(ctx.getBean(Single.class).getProto());
          System.out.println(ctx.getBean(Single.class).getProto());
          System.out.println(ctx.getBean(Single.class).getProto());
      }
  }
  ```

  ```text
  싱글톤 빈이 프로토타입 빈 참조 ==========
  spring.framework.core.l05BeanScope.Proto@38600b
  spring.framework.core.l05BeanScope.Proto@38600b
  spring.framework.core.l05BeanScope.Proto@38600b
  ```

  * 업데이트 하기위한 방법
    * `1` scoped-proxy
    * `2` Object-Provider : 소스 코드에 침투
    * `3` Provider (표준)
  * scoped-proxy
    * [프록시 패턴](https://en.wikipedia.org/wiki/Proxy_pattern)
    * 프록시 모드를 설정해주는 것 (Default : 프록시 사용 X)
    * `proxyMode = ScopedProxyMode.TARGET_CLASS`
      * 클래스 기반의 **프록시로 Proto 빈을 감싸고, 다른 빈들은 프록시로 감싼 빈을 사용하게 하는 설정**
      * 싱글톤 스코프가 프로토타입의 스코프를 가진 빈을 직접 참조하면 안되기 때문에 위의 설정이 필요함
      * 그렇다면 왜 프록시를 거쳐야 할까?
        * 직접 쓰게 되면 프로토타입 빈을 바꿔줄 수가 없음
        * 하지만 프록시는 매번 새로 바꿔줄 수 있음

  ```java
  @Component
  @Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
  public class Proto {
      
  }
  ```

  ```text
  싱글톤 빈이 프로토타입 빈 참조 ==========
  spring.framework.core.l05BeanScope.Proto@6826c41e
  spring.framework.core.l05BeanScope.Proto@3003697
  spring.framework.core.l05BeanScope.Proto@64d43929
  ```

* 싱글톤 객체 사용 시 주의할 점

  * 프로퍼티가 공유됨
    * 멀티스레드 환경에서 빈의 프로퍼티를 변경하려고 하기 때문에 스레드 세이프한 코딩 필요
  * ApplicationContext 초기 구동 시 인스턴스 생성