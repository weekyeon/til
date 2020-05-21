# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# PSA

### 정의

* Portable Service Abstraction
* Servlet 애플리케이션을 만들고 있지만, Servlet을 전혀 사용하지 않는다.
  * Spring petclinic은 Servlet을 사용하지 않는다.
    * @GetMapping
    * @PostMapping

### Spring Web MVC

* 스프링이 제공하는 스프링 웹 MVC 추상화 계층이다.
  * Servlet을 Row Level로 사용하지 않아도 된다.
  * 편의성 증대

```java
@Controller
class OwnerController{
    
    @GetMapping("/owners/new")
    public String initCreationForm(Map<String, Object> model){
        Owner owner = new Owner();
        model.put("owner", owner);
        return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
    }
    
}
```

* @Controller
  * 요청을 맵핑할 수 있는 컨트롤러 역할 수행 클래스
  * 해당 클래스 안에 @GetMapping, @PostMapping 등으로 요청을 맵핑
    * *맵핑한다 : URL에 해당하는 요청이 들어왔을 때, 그 요청을 메소드가 처리하게끔 맵핑한다.*

### Spring Transaction

* 데이터에 데이터를 넣을 때, A를 하고 B를 하고 C까지 해야 **하나의 작업**으로 완료되는 경우
* ALL or Nothing
* [docs, Interface Platform TransactionManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html)
* @Transactional
  * 스프링이 제공하는 추상화 레벨 애노테이션
  * 해당 애노테이션이 붙은 메소드는 트랜잭션 처리가 된다.
    * 명시적인 코딩을 하지 않아도 됨

