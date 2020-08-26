# 기본 API 클래스 2

* 신용권, 『이것이 자바다 1,2』, 한빛미디어(2016)



### 목차

* 자바 API 도큐먼트
* java.util
* java.util.Objects
* java.util.Arrays
* java.util.StringTokenizer
* java.util.regex.Pattern
* java.util.Random
* java.util.Calendar / Date
* java.time

* java.text.Format



### java.util

* 자바 프로그램 개발에 조미료 같은 역할을 하는 클래스
* 대부분 컬렉션 클래스들
* Arrays, Claendar, Date, Objects, StringTokenizer, Random



### java.util.Objects

* Object의 유틸리티 클래스
* 객체 비교, 해시코드 생성, null 여부, 객체 문자열 리턴 등의 연산을 수행하는 정적 메소들로 구성

#### compare(T a, T b, Comparator&lt;T&gt; c)

* 리턴 타입 `int`
* 두 객체 a와 b를 Comparator를 사용해서 비교
* a < b : 음수 리턴 / a = b : 0 리턴 / a > b : 양수 리턴하도록 구현 클래스 만들어야 함

#### deepEquals(Object a, Object b)

* 리턴 타입 `boolean`
* 두 객체의 깊은 비교
* 배열의 항목까지 비교함

#### equals(Object a, Object b)

* 리턴 타입 `boolean`
* 두 객체의 얕은 비교
* 번지만 비교함

#### hash(Object... values)

* 리턴 타입 `int`
* 매개값이 저장된 배열의 해시코드 생성

#### hashCode(Object o)

* 리턴 타입 `int`
* 객체의 해시코드 생성

#### isNull(Object obj)

* 리턴 타입 `boolean`
* 객체가 null인지 조사

#### isNotNull(Object obj)

* 리턴 타입 `boolean`
* 객체가 null이 아닌지 조사

#### requireNonNull(T obj)

* 리턴 타입 `T`
* 객체가 null인 경우 예외 발생
* requireNonNull(T obj, String message)
  * 객체가 null인 경우, 주어진 예외 메시지를 포함하여 예외 발생
* requireNonNull(T obj, Supplier&lt;String&gt; messageSupplier)
  * 객체가 null인 경우, 람다식이 만든 예외 메시지 포함하여 예외 발생

#### toString(Object o)

* 리턴 타입 `String`
* 객체의 toString() 리턴값 리턴
* toString(Object o, String nullDefault)
  * 첫 번째 매개값이 null일 경우 두 번째 매개값 리턴



### java.util.Arrays

* 배열 조작 기능
  * 배열 조작, 배열의 복사/항목 정렬/항목 검색과 같은 기능
* 단순 배열 복사는 System.arraycopy() 메소드 사용 가능하지만, Arrays는 배열 조작 기능 제공
* Arrays 클래스의 모든 메소드는 정적 메소드
* [Java 8, Arrays](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)



### java.util.StringTokenizer

* 구분자를 기준으로 부분 문자열을 분리하기 위해 사용하는 클래스
  * String의 split() 메소드는 정규 표현식으로 구분함
  * StringTokenizer는 문자로 구분함
* 문자열이 한 종류의 구분자로 연결되어 있을 경우, StringTokenizer 클래스 사용하면 쉽게 분리 가능
* 구분자 생략 시 공백이 기본 구분자
* `StringTokenizer st = new StringTokenizer("문자열", "구분자");`



### java.util.regex.Pattern

* 문자열을 정규 표현식으로 검증하는 기능
* 해당 클래스의 정적 메소드인 `matches()` 메소드가 제공



### java.util.Random

* 난수를 얻어내기 위해 다양한 메소드 제공
* java.lang.Math.random() 메소드는 0.0에서 1 사이의 double 난수를 얻는 데만 사용한다면, Random 클래스는 boolean, int, long, float, double 난수를 얻을 수 있음
* Random 클래스는 종자값(seed) 설정 가능
  * 종자값, 난수를 만드는 알고리즘에 사용되는 값
  * 종자값이 같으면 같은 난수를 얻음



### java.util.Calendar / Date

* [Java 7, Calendar](https://docs.oracle.com/javase/7/docs/api/java/util/Calendar.html)
* [Java 7, Date](https://docs.oracle.com/javase/7/docs/api/java/util/Date.html)



### java.time

* 자바 8부터 날짜와 시간을 나타내는 여러 가지 API 추가
* [Java 8, java.time](https://docs.oracle.com/javase/8/docs/api/index.html?java/time/package-summary.html)



### java.text.Format

* [Java 7, Format](https://docs.oracle.com/javase/7/docs/api/java/text/Format.html)
* [Java 8, Format](https://docs.oracle.com/javase/8/docs/api/java/text/Format.html)

