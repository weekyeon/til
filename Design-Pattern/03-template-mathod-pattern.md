# 템플릿 메소드 패턴

* [가람, 자바 디자인 패턴의 이해 - Gof Design Pattern](https://www.inflearn.com/course/자바-디자인-패턴/dashboard)
* [Source Repo](https://github.com/weekyeon/design-pattern)



### 목차

* 템플릿 메소드 패턴 (Template Method Pattern)
* 실습
* 유지보수



### 템플릿 메소드 패턴 (Template Method Pattern)

* 일정한 프로세스를 가진 요구사항을 Template Method 패턴을 이용하여 구현할 수 있음
* 알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조 변경없이 알고리즘을 재정의하는 패턴
* 템플릿 메소드 패턴을 사용하는 경우
  * 구현하려는 알고리즘이 일정한 프로세스(여러 단계)가 있을 때
  * 구현하려는 알고리즘의 변경 가능성이 있을 때

![설계](https://github.com/weekyeon/til/blob/master/Design-Pattern/img/templatemethod1.png)

* 템플릿 메소드 구현 단계
  * `1` 알고리즘을 여러 단계로 나눔(operaion1,2,3())
  * `2` 나눠진 알고리즘의 단계를 메소드로 선언(templateMethod())
  * `3` 알고리즘을 수행할 템플릿 메소드 생성 후 나눠진 알고리즘 메소드들 호출(templateMethod())
  * `4` 하위 클래스(Concreate Class)에서 나눠진 메소드들 구현



### 실습

* 신작 게임의 접속을 구현해주세요.
  * requestConnection(String str) : String
* 유저가 게임 접속 시 다음을 고려해야 합니다.
  * 보안 과정 : 보안 관련 부분을 처리합니다.
    * doSecurity(String string) : String
  * 인증 과정 : User name과 password가 일치하는지 확인합니다.
    * authentication(String id, String password) : boolean
  * 권한 과정 : 접속자가 유료 회원인지 무료 회원인지, 게임 마스터인지 확인합니다.
    * authorization(String userName) : int
  * 접속 과정 : 접속자에게 커넥션 정보를 넘겨줍니다.
    * connection(String info) : String

```java
package exercise.library;

public abstract class AbstGameConnectHelper {

    //보안 단계
    protected abstract String doSecurity(String string);

    //인증 단계
    protected abstract boolean authentication(String id, String password);

    //권한 단계
    protected abstract int authorization(String userName);

    //접속 단계
    protected abstract String connection(String info);

    //템플릿 메소드
    public String requestConnection(String encodedInfo){

        //보안 과정 --암호화 된 String을 복호화(디코드)
        String decodedInfo = doSecurity(encodedInfo);

        //인증 과정 --반환된 것을 가지고 id, pwd 할당
        String id = "aaa";
        String password = "bbb";

        if(!authentication(id, password)){
            throw new Error("아이디와 암호가 일치하지 않습니다.");
        }

        //권한 과정 --
        String userName = "userName";
        int i = authorization(userName);

        switch (i){
            case 0:
                System.out.println("게임 매니저");
                break;
            case 1:
                System.out.println("유료 회원");
                break;
            case 2:
                System.out.println("무료 회원");
                break;
            case 3:
                System.out.println("권한 없음");
                break;
            default:
                //기타 사항 : 확장성을 위해 생성
                break;
        }
        return connection(decodedInfo);
    }
}
```

```java
package exercise.library;

public class DefaultGameConnectHelper extends AbstGameConnectHelper {
    //하위 클래스 구현

    @Override
    protected String doSecurity(String string) {
        System.out.println("디코드 과정");
        return string;
    }

    @Override
    protected boolean authentication(String id, String password) {
        System.out.println("id, pwd 확인 과정");
        return true;
    }

    @Override
    protected int authorization(String userName) {
        System.out.println("권한 확인 과정");
        return 0;
    }

    @Override
    protected String connection(String info) {
        System.out.println("마지막 접속 단계!");
        //접속 단계에서 필요한 정보 넘겨줌
        return info;
    }
}
```

```java
package exercise.main;

import exercise.library.AbstGameConnectHelper;
import exercise.library.DefaultGameConnectHelper;

public class Main {
    public static void main(String[] args) {
        AbstGameConnectHelper abstGameConnectHelper = new DefaultGameConnectHelper();
        abstGameConnectHelper.requestConnection("아이디 암호 등 접속 정보");
    }   
}
```



### 유지보수

* 보안 부분이 정부 정책에 의해서 강화되었습니다. 강화된 방식으로 코드를 변경해야 합니다.
* 밤 10시 이후에는 접속이 제한되도록 합니다.

```java
package exercise.library;

public class DefaultGameConnectHelper extends AbstGameConnectHelper {
    //하위 클래스 구현

    @Override
    protected String doSecurity(String string) {
        System.out.println("강화된 알고리즘을 이용한 디코드 과정");
        return string;
    }

    @Override
    protected boolean authentication(String id, String password) {
        System.out.println("id, pwd 확인 과정");
        return true;
    }

    @Override
    protected int authorization(String userName) {
        System.out.println("권한 확인 단계");
        //서버에서 유저 이름을 가지고 유저의 나이를 알 수 있다.
        //나이와 현재 시각을 확인하고 성인이 아니고 10시가 지났다면
        //권한이 없는 것으로 한다.
        return 0;
    }

    @Override
    protected String connection(String info) {
        System.out.println("마지막 접속 단계!");
        //접속 단계에서 필요한 정보 넘겨줌
        return info;
    }
}
```

```java
package exercise.library;

public abstract class AbstGameConnectHelper {

    //보안 단계
    protected abstract String doSecurity(String string);

    //인증 단계
    protected abstract boolean authentication(String id, String password);

    //권한 단계
    protected abstract int authorization(String userName);

    //접속 단계
    protected abstract String connection(String info);

    //템플릿 메소드
    public String requestConnection(String encodedInfo){

        //보안 과정 --암호화 된 String을 복호화(디코드)
        String decodedInfo = doSecurity(encodedInfo);

        //인증 과정 --반환된 것을 가지고 id, pwd 할당
        String id = "aaa";
        String password = "bbb";

        if(!authentication(id, password)){
            throw new Error("아이디와 암호가 일치하지 않습니다.");
        }

        //권한 과정 --
        String userName = "userName";
        int i = authorization(userName);

        switch (i){
            case -1:
                throw new Error("셧다운");
            case 0:
                System.out.println("게임 매니저");
                break;
            case 1:
                System.out.println("유료 회원");
                break;
            case 2:
                System.out.println("무료 회원");
                break;
            case 3:
                System.out.println("권한 없음");
                break;
            default:
                //기타 사항 : 확장성을 위해 생성
                break;
        }
        return connection(decodedInfo);
    }
}
```

