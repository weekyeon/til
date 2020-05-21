# Spring Framework Introduction

* [백기선, 예제로 배우는 스프링 입문](https://www.inflearn.com/course/spring_revised_edition)

* [학습 자료, spring-petclinic](https://github.com/spring-projects/spring-petclinic)

* [Source Repo](https://github.com/weekyeon/spring-petclinic)



# AOP

### 정의

* 여러 곳에서 반복적으로 사용되는 동일한 코드를 한 곳으로 모으는 방법

### AOP 구현 방법

* 컴파일
  * A.java -----(AOP)-----> A.class
  * AspectJ가 제공
* 바이트코드 조작
  * A.java ---> A.class -----(AOP)-----> 메모리
  * Class Loader가 클래스를 읽어와서 메모리에 올릴 때 조작한다.
  * AspectJ가 제공
* Proxy 패턴
  * 스프링 AOP가 사용하는 방법
  * 디자인 패턴 중 하나를 사용해서 AOP와 같은 효과를 낸다.
* 참고 자료
  * [AspectJ](https://www.eclipse.org/aspectj/)
  * [Proxy Parttern](https://refactoring.guru/design-patterns/proxy)