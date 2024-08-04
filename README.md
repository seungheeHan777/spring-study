# spring 공부하기

---

## spring 교과서를 보면서 학습하기

정리하면서 요약하기

## 목차

1. [1장](#1장)
2. [2장](#2장)
3. [3장](#사용법)

---

## 1장

스프링 프레임워크란?

자바 애플리케이션 프레임워크(application framework)

스프링 생태계

스프링은 복잡해서 생태계와 비슷한 구조이다.

그 중 일부분을 말하자면,

1. 스프링 코어(Spring Core)

   기본 기능을 포함하는 스프링의 기반 부분 중 하나.
   이 기능 중에 하나가 스프링 컨텍스트(Spring context)이다.

   스프링 컨텍스트는 스프링이 앱의 인스턴스를 관리하게 하는 기능이다.

   다른 기능으로는 스프링 애스펙트(Spring aspects)가 있다.

   애스펙트는 스프링이 앱에서 정의한 메소드를 가로채고, 조작한다.

   또 스프링 코어로 스프링 표현 언어(Spring Expression Language,SpEL)이 있다.

   특정 언어를 사용해 스프링 구성 내용을 작성한다.

이런 기능들을 사용해 스프링 코어가 앱을 통합하는데 사용되는 메커니즘을 담고 있다.

2. 스프링 모델-뷰-컨트롤러(MVC)

   HTTP 요청을 처리하는 웹 애플리케이션을 개발하도록 하는 스프링 프레임워크이다.

3. 스프링 데이터 액세스(Spring Data Access)

   스프링 기본 부분 중 하나, SQL 데이터베이스에 연결해 앱 영속성 계층을 구현하는데 사용하는 기본 도구 제공

4. 스프링 테스팅

   스프링 애플리케이션 테스트하는데 필요한 도구

#### 스프링 코어의 이해: 스프링 기초

스프링은 제어 역전(Inversion of Control,IOC)원칙을 기반으로 작동한다.

제어 역전은 앱이 실행을 제어하는 대신, 다른 소프트웨어(스프링 프레임워크)에게 제어 권한을 넘긴다.

구성(configuration)을 이용해 앱 로직을 정의하도록 작성된 코드 관리 방법을 프레임워크에 지시한다.

제어 역전은 앱이 자체 코드로 실행을 제어하거나 의존성을 사용하지 못하고, 프레임워크가 제어한다는 의미이다.

#### 스프링 부트

프레임워크의 모든 구성을 사용자가 직접 설정 대신 스프링 부트가 필요에 따라 정의하는 기본 구성을 제공

알려진 규칙은 따르고, 코드를 덜 작성함.

## 2장

컨텍스트(context)는 스프링 앱에서 애플리케이션 컨텍스트라고 한다.

컨텍스트를 프레임워크가 관리하는 모든 객체 인스턴스를 추가하는 **앱의 메모리 공간**

스프링이 객체를 알려면 스프링 컨텍스트에 객체를 추가해야한다. 또한 객체 간의 관계를 설정할 수 있다.

객체 인스턴스를 빈(bean) 이라고 한다.

---

메이븐 프로젝트에서는 외부 의존성을 pom.xml 파일에서 관리하고, 추가한다.

모든 의존성은 <dependencies></dependencies> 사이에 작성한다.

각 의존성은 <dependency></dependency> 태그를 이용해서 추가한다.

```xml
<dependencies>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context</artifactId>
<version>6.1.6</version>
</dependency>
</dependencies>
```

이런 식으로 작성한다.

#### 스프링 컨텍스트에 빈 추가

스프링 컨텍스트에 빈을 추가하는 방법은 여러 가지가 있다.

- @Bean 애너테이션 사용
- 스테레오타임(stereotype) 애터테이션 사용
- 프로그래밍 방식

먼저 스프링 컨텍스트 인스턴스를 생성한다.

AnnotationConfigApplicationContext 클래스를 이용해서 생성한다.

```java
var context = new AnnotationConfigApplicationContext();
```

이제 생성한 스프링 컨텍스트에 객체 인스턴스를 추가해야한다.

만약 Parrot 클래서의 인스턴스를 넣는다고 생각해보자.

```java
public class Parrot {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

이런 Parrot 클래스에서

```java
Parrot p = new Parrot();
```

인스턴스를 생성하면, Parrot 인스턴스는 생성되었지만 스프링 컨텍스트 외부에 존재하는 것이다.

우리는 이 인스턴스를 컨텍스트 안에 넣어서 관리하고 싶다.

- @Bean

  1.  (@Configuration 애너테이션이 지정된) 프로젝트 구성 클래스를 정의한다.

      ```java
      @Configuration
      public class ProjectConfig {

      }
      ```

      이렇게 @Configuration으로 지정된 ProjectConfig을 정의한다.

  2.  객체 인스턴스를 반환하는 메서드를 구성 클래스에 추가하고, @Bean으로 메서드에 주석을 추가

      ```java
      @Bean
      Parrot parrot() {
         var p = new Parrot();
         p.setName("Koko");
         return p;
      }
      ```

      @Bean 으로 메서드에 추가한다.

  3.  클래스를 사용한다.

      ```java

       public static void main(String[] args) {
       var context = new AnnotationConfigApplicationContext(ProjectConfig.class);
       }

      ```

      AnnotationConfigApplicationContext에 ProjectConfig.class를 이용해서 컨텍스트안에 객체를 추가한다.
      이 구성 클래스를 사용하면 된다.
