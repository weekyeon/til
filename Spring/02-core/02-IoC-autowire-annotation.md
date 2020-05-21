# Spring Framework Core

* [백기선, 스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)

* [Spring Framework Repo](https://github.com/spring-projects/spring-framework)

* [Source Repo](https://github.com/weekyeon/spring-framework-core)



# @Autowire

### 정의

* 필요한 의존 객체의 타입에 해당하는 빈을 찾아 주입한다.

### 사용 위치

* 생성자
* Setter
* 필드

### 동작 원리

* BeanPostProcessor

  * 새로 만든 빈 인스턴스를 수정할 수 있는 라이프 사이클 인터페이스

    > "빈 생성 후, 빈을 초기화(Initialization)하는 LifeCycle이 있는데, 이 사이클 이전/이후에 부가적인 작업을 할 수 있는 또 다른 LifeCycle을 BeanPostProcessor라고 한다."
    >
    > * Initialization
    >   * @PostConstruct를 사용하여 정의 가능

    * Initialization

      * @PostConstruct를 사용하여 정의할 수 있다.

      ```java
      @Service
      public class BookService{
          
          @Autowired
          BookRepository bookRepository;
          
          public void printBookRepository(){
              System.out.println(bookRepository.getClass());
          }
          
          @PostConstruct
          public void setUp(){
              //빈이 만들어진 다음에 해야 할 일 작성
              //@Autowired를 사용해서 주입 받는 빈들이 다 주입됐다고 생각하고 이후 작업을 작성해야 한다.
              //애플리케이션 구동 중에 동작하기 때문에, BookServiceRunner와 동작하는 시각이 다르다.
          }
          
      }
      ```

      * implements InitializingBean 사용하여 정의할 수 있다.

  * [docs, Interface BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)

* AutowiredAnnotationBeanPostProcessor extends BeanPostProcessor

  * 스프링이 제공하는 @Autowired와 @Value, JSR-330의 @Inject를 지원하는 애노테이션 처리기
  * [docs, Interface AutowiredAnnotationBeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html)

### 의존성 주입 방법 : 해당 타입의 빈이 없거나 한 개인 경우

* 생성자

  ```java
  @Service
  public class BookService{
      
      BookRepository bookRepository;
      
      @Autowired
      public BookService(BookRepository bookRepository){
          this.bookRepository = bookRepository;
      }
      
  }
  ```

  ```java
  @Repository
  public class BookRepository{
      //이하 BookRepository 생략
  }
  ```

* Setter

  ```java
  @Service
  public class BookService{
  	
      BookRepository bookRepository;
  
      @Autowired(required = false)
      public void setBookRepository(BookRepository bookRepository) {
          this.bookRepository = bookRepository;
      }
      
  }
  ```

  * @Autowired(required = false)
    * 의존성 주입을 반드시 해야 하는 게 아닐 때
    * 혹은 Autowired를 이용하여 의존성 주입을 해야 하는데 빈을 찾지 못할 때
      * 해당 인스턴스는 빈으로 등록됐지만, 의존성 주입은 되지 않은 상태로 생성됨

* 필드

  ```java
  @Service
  public class BookService{
      
      @Autowired(required = false)
      BookRepository bookRepository;
      
  }
  ```

  * 필드를 이용해서 하는 의존성 주입은 생성자를 이용하는 것과 약간 다르다.
    * 생성자는 주입 받아야 하는 **빈이 없으면** 인스턴스를 생성하지 못한다.
    * Setter, 필드를 이용하는 의존성 주입은 주입 받아야 하는 **빈이 없어도** 인스턴스를 생성할 수 있다.

### 의존성 주입 방법 : 같은 타입의 빈이 여러 개인 경우

* @Primary

  * 여러 개의 빈 중에 해당 마크가 붙은 빈을 사용하도록 마킹한다.

  ```java
  @Service
  public class BookService{
      
      @Autowired
      BookRepository bookRepository;
      
      //Repository Class 출력 메소드
      public void printBookRepository(){
          System.out.println(bookRepository.getClass());
          //결과 : MyBookRepository
      }
      
  }
  ```

  ```java
  public interface BookRepository{
      
  }
  
  @Repository @Primary
  public class MyBookRepository implements BookRepository{
      
  }
  
  @Repository
  public class YourBookRepository implements BookRepository{
      
  }
  ```

  ```java
  @Component
  public class BookServiceRunner implements ApplicationRunner{
      //BookService를 주입 받아서
      //해당 서비스에 어떤 Repository 빈이 주입되는지 출력해보기
      
      @Autowired
      BookService bookService;
      
      @Override
      public void run(ApplicationArguments args) throws Exception {
          bookService.printBookRepository();
      }
      
  }
  ```

* 해당하는 타입의 빈 모두 주입 받기

  ```java
  @Service
  public class BookService{
      
      @Autowired
      List<BookRepository> bookRepository;
      
      public void printBookRepository(){
          this.bookRepository.forEach(System.out::println);
          //결과 : MyBookRepository, YourBookRepository
      }
      
  }
  ```

* @Qualifier

  * 설정된 ID를 가진 빈을 찾아 주입한다. (빈 이름으로 주입)
  * 빈 ID는 클래스 이름의 첫 글자를 소문자로 바꾼 것과 같다.

  ```java
  @Service
  public class BookService{
      
      @Autowired @Qualifier("yourBookRepository")
      BookRepository bookRepository;
      
      //Repository Class 출력 메소드
      public void printBookRepository(){
          System.out.println(bookRepository.getClass());
          //결과 : YourBookRepository
      }
      
  }
  ```