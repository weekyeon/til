# About Node.js

* [Egoing Lee, Node.js를 이용해 웹애플리케이션 만들기](https://inf.run/RQbs)
* [new_wisdom, [JavaScript] ES6/ES2015 문법 몇 가지](https://velog.io/@new_wisdom/JavaScript-ES6ES2015-%EB%AC%B8%EB%B2%95-%EB%AA%87-%EA%B0%80%EC%A7%80)
* [parksj3205, ES6 문법 맛보기 - 화살표 함수(arrow function)](https://velog.io/@parksj3205/2019-08-30-1208-%EC%9E%91%EC%84%B1%EB%90%A8)



### 목차

* JavaScript ES6 문법 - const VS let VS var
* JavaScript ES6 문법 - 템플릿 문자열(`, 백틱)
* JavaScript ES6 문법 - 화살표 함수(=>)
* Node.js
* NPM
* Express



### JavaScript ES6 문법 - const VS let VS var

```javascript
if(true){
    var a = 1;
    const b = 1;
}
console.log(a); // 1
console.log(b); // Error : Uncaught ReferenceError
```

* const & let은 Block Scope

* const는 상수
  * 선언한 값을 변경할 수 없음
  * 선언 시 초기화 필수
  * const에 객체를 넣고 객체 자체를 바꾸면 **Error !**
  * 단, 객체 안 요소 값은 변경 가능



### JavaScript ES6 문법 - 템플릿 문자열(`, 백틱)

* 기존에는 문자열 연결 시 '+'를 사용하여 연결
* ES6에서는 `(백틱)을 사용하여 문자열 선언, 변수는 ${}로 표현



### JavaScript ES6 문법 - 화살표 함수(=>)

* ES6 화살표 함수 이전의 함수 표현 법

```javascript
// 함수 표현식 : 변수를 먼저 선언 후 함수를 대입(익명함수)
const addFunc = function(x, y){ return x+y; }

// 함수 선언식 : function에 이름을 붙여 함께 선언
const addFunc2(x, y){ return x+y; }
```

* 화살표 함수
  * function 키워드 생략 가능
  * 함수의 매개 변수가 **한 개**인 경우 괄호( ) 생략 가능
  * 함수 바디가 **표현식 하나**라면 중괄호와 return문 생략 가능
  * **항상 익명함수**

```javascript
function f(){ }
const f = () => {};

const addFunc3 = function(){ return x+y; }
const addFunc4 = () => { return x+y; } //function 키워드 생략 가능

// EX2)
const func1 = function(num){
    for(let i=0; i<10; i++){num++;}
    return num;
}
const func2 = num => {
    for(let i=0; i<10; i++){num++;}
    return num;
}

const func3 = function(num){return `입력된 숫자는 ${num}입니다.`;}
const func4 = num => `입력된 숫자는 ${num}입니다.`; //중괄호, return문 생략 가능
```

* 화살표 함수 깊게 알아보기

  * **this**가 정적으로 묶임
    * this 키워드는 변수나 속성 이름이 X
    * this 키워드에는 값을 할당할 수 없음
    * 중첩함수가 메소드 형태로 호출되면 중첩함수의 this 값은 그 함수가 속한 객체
    * 중첩함수가 함수 형태로 호출되면 중첩함수의 this 값은 전역객체 또는 undefined(strict mode)
  * **객체 생성자**로 사용 불가

  ```javascript
  const Foo = () => {};
  const foo = new Foo(); // Error
  ```

  * Arguments 변수 사용 불가



### Node.js

* 



### NPM

* 



### Express

* Node.js Web F/W
* REST Server 구현 편리함



### Express-generator

* F/W에 필요한 package.json 및 기본 구조를 알아서 잡아줌

* 구조

  ```javascript
  ├── app.js
  ├── bin
  │   └── www
  ├── package.json
  ├── public
  │   ├── images
  │   ├── javascripts
  │   └── stylesheets
  │       └── style.css
  ├── routes
  │   ├── index.js
  │   └── users.js
  └── views
      ├── error.pug
      ├── index.pug
      └── layout.pug
  ```

  * app.js
    * 프로젝트의 핵심
    * 전반적인 프로젝트의 설정을 담고 있음
    * 미들웨어 관리
  * bin/www
    * http 모듈에 Express 모듈을 연결하여, http와 같은 port 및 서버 설정에 관한 내용을 담고 있음
    * 서버를 실행하는 스크립트
  * package.json
    * 의존성 관계, 프로젝트명, 버전 등 담고 있음
  * public
    * 정적 파일(img, js, css 등 Client에서 접근 가능한 파일) 관리
  * routes
    * MVC 패턴에서 Controller 역할
    * 서버의 모든 로직을 담고 있음
    * index.js 기반으로 라우팅 관리를 해주면 됨
  * views
    * MVC 패턴에서 View 역할