# About Node.js

* [Egoing Lee, Node.js를 이용해 웹애플리케이션 만들기](https://inf.run/RQbs)



### 목차

* Node.js
* NPM
* Express



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