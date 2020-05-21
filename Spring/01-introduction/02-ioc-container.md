# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# IoC Container

* 빈(Bean)을 만들고, 빈 사이의 의존성을 엮어주며, 컨테이너가 가지고 있는 빈들을 제공해주는 역할
* 모든 클래스가 빈으로 등록되어 있는 것은 아니다.
* 무엇을 보고 Bean인지 알 수 있을까?
  * class 왼쪽에 **녹색 콩**이 있으면 Bean으로 등록되어 있는 것을 뜻한다.
  * Annotation @Bean을 이용해서 Bean으로 등록할 수 있다.
* 의존성 주입은 Bean끼리만 가능하다.
  * 즉, 스프링 IoC 컨테이너 안에 있는 빈들끼리만 의존성 주입이 가능하다.
* ApplicationContext 안에 모든 빈이 있다.
  * 즉, ApplicationContext가 해당 타입의 Bean을 찾아서 의존성 주입을 해준다.
* 참고 자료
  * [Understanding Application Context](https://github.com/spring-guides/understanding/tree/master/application-context)
  * [docs, Interface ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)
  * [docs, Interface BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)



