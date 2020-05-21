# Spring Framework Core

* [백기선, 스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)

* [Spring Framework Repo](https://github.com/spring-projects/spring-framework)

* [Source Repo](https://github.com/weekyeon/spring-framework-core)



# @ComponentScan

### 주요 기능

* 어디부터 어디까지 **스캔**할 것인지 범위와 위치를 잘 알고 있어야 한다.
* 스캔 중에 어떤 애노테이션을 스캔하고 스캔하지 않을지 **필터** 또한 잘 알고 있어야 한다.

### @Component

* @Service
* @Repository
* @Controller
* @Configuration

### 동작원리

* 실제 스캐닝은 [ConfigurationClassPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ConfigurationClassPostProcessor.html)라는 [BeanFactoryPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html)에 의해 처리된다.



