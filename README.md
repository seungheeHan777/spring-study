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

  2.  객체 인스턴스를 반환하는 메서드를 구성 클래스(ProjectConfig을)에 추가하고, @Bean으로 메서드에 주석을 추가

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

       Parrot p= context.getBean(Parrot.class); // 스프링 컨텍스트에서 Pattot 타입의 빈 참조를 가져옴


      ```

      AnnotationConfigApplicationContext에 ProjectConfig.class를 이용해서 컨텍스트안에 객체를 추가한다.
      이 구성 클래스를 사용하면 된다.

      이와 같은 방식으로 스프링 컨텍스트에 모든 종류의 객체를 추가할 수 있다. (string, integer...)

      *스프링 컨텍스트의 목적*은 스프링이 관리해야 할 인스턴스를 추가하는 것.
      모든 객체를 추가하지 않음.

  ##### 이제 동일한 타입의 빈을 스프링 컨텍스트에 여러 개 추가하는 방법을 알아보자.

  ```java

  @Configuration
  public class ProjectConfig {

      @Bean
    Parrot parrot1() {
       var p = new Parrot();
       p.setName("Koko");
       return p;
    }

    @Bean
    Parrot parrot2() {
       var p = new Parrot();
       p.setName("Miki");
       return p;
    }
    @Bean
    Parrot parrot3() {
       var p = new Parrot();
       p.setName("Riki");
       return p;
    }
    }

  ```

  이렇게 구성 클래스에 Parrot 타입 빈 3 개 선언.

  아직 컨텍스트에 빈을 가져온 상태는 아님.

  전에 코드처럼 빈을 불러오면 _에러 발생_

  ```java

   public static void main(String[] args) {
     var context = new AnnotationConfigApplicationContext(ProjectConfig.class);
     }

     Parrot p= context.getBean(Parrot.class); // Parrot 인스턴스 중 어떤 것인지 몰라서 예외 발생


  ```

  이런 모호성 문제를 해결하려면 빈 이름을 사용해 인스턴스 중 하나를 정확하게 참조해야 한다.

  예를 들어

  ```java

  Parrot p = context.getBean("parrot2",Parrot.class);

  ```

  빈에 다름 이름을 지정하는 방법으로는 아래와 같은 방법이 있다.

  - @Bean(name='')

  - @Bean(value='')

  - @Bean('')

  또는 기본키를 지정하는 방법도 가능하다.

  @Primary 를 이용한다.

---

#### 스테레오타입 애너테이션으로 스프링 컨텍스트에 빈 추가

스테레오타입 애너테이션을 사용하면 더 적은 코드로 스프링에 컨텍스트에 빈을 추가할 수 있다.

가장 기본적인 방법인 @Component를 사용해보자.

스테레오 타입 애너테이션을 사용하려면 스프링 컨텍스트에 추가할 인스턴스의 클래스 위에 이를 추가해야 한다. 이렇게 하는 것을 _'클래스를 컴포넌트로 표시했다'_ 고 한다.

수행해야 할 단계

1. @Component 애너테이션으로 스프링이 해당 컨텍스트에 인스턴스를 추가할 클래스를 표시

2. 구성 클래스 위에 @ComponentScan 애너테이션으로 표시한 클래스를 어디에서 찾을 수 있는지 스프링에 지시함

```java

@Component
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

이 상태로는 동작하지 않음.

기본적으로 스프링은 애너테이션으로 지정된 클래스를 검색하지 않기 때문에 구성 클래스에 @ComponentScan 애너테이션을 추가해야 함.
ComponentScan 으로 클래스를 찾을 위치를 스프링에 알려 줄 수 있음.

```java

  @Configuration
  @ComponentScan(basePackages="main")
  // basePackages 속성으로 스프링에 스테레오타입 애너테이션이 지정된 클래스를 찾을 위치를 알려줌.
  public class ProjectConfig {

  }

```

이제 스프링에 다음을 지정

1. 컨텍스트에 추가할 인스턴스의 클래스

2. 이런 클래스를 찾을 수 있는 위치

```java

public static void main(String[] args) {
        var context = new AnnotationConfigApplicationContext(ProjectConfig.class);

        Parrot p = context.getBean(Parrot.class);

        System.out.println(p);
        System.out.println(p.getName());
    }

```

이제 스프링 컨텍스트에 빈을 추가할 때 가장 자주 접하는 두가지를 비교해보자.

@Bean 애너테이션 사용

1. 스프링 컨텍스트에 추가할 인스턴스의 생성을 완전히 제어. @Bean이 달린 메서드 안에서 인스턴스를 생성하고 구성하는 것은 사용자 책임

2. 동일한 타입의 인스턴스를 스프링 컨텍스트에 더 추가.

3. 스프링 컨텍스트에 모든 객체 인스턴스를 추가할 수 있다.
   즉, 인스턴스를 정의한 클래스가 앱 내에서 정의되지 않아도 추가 가능

4. 생성하는 각 빈에 별도의 메서드 작성. 앱에 상용구 코드 추가. 이 때문에 스테레오타입 애너테이션을 선호

스테레오타입 애너테이션 사용

1. 프레임워크가 인스턴스를 생성한 후에만 제어 가능

2. 컨텍스트에 클래스의 인스턴스를 하나만 추가

3. 애플리케이션이 소유한 클래스의 빈을 생성하는 데만 사용함.

4. 스프링 컨텍스트에 빈을 추가해도 앱에 상용구 코드가 추가되지 않음. 일반적으로 앱에 속한 클래스에서는 이 방식 선호.

---

PostConstruct를 사용해 인스턴스 생성 후 관리

@Component를 사용하면 스프링이 클래스의 생성자를 호출 한 후에는 어떤 것도 할 수 없다. 따라서 명령을 실행하기 위해서는 PostConstruct 애너테이션을 사용한다.

---

#### 프로그래밍 방식으로 스프링 컨텍스트에 빈 추가

registerBean() 메서드를 이용해 특정 클래스를 호출 할 수 있다.

4개의 매개변수가 있다.

1. String beanName
2. Class<T> beanClass
3. Supplier<T> supplier
4. BeanDefinitionCustomizer

예제 2-8 참조

---

### 요약

- 스프링에서 가장 먼저 배울 것은 스프링 컨텍스트에 빈을 추가하는 것

- 스프링 컨텍스트에 빈을 추가하는 방법은 3 가지. @Bean, 스테레오타입 애너테이션, 프로그래밍 방식

- @Bean을 사용하면 어떤 종류의 객체 인스턴스도 빈으로 추가 가능, 같은 종류의 다수 인스턴스도 가능. 이 관점에서는 스테레오타입보다 유연하나, 개별 인스턴스에 대해 구성 클래스에서 별도의 메서드를 만들어야 하는 것이 더 많은 코드를 작성하게 된다.

- 스테레오타입 애너테이션을 사용하면 특정 애너테이션이 있는 애플리케이션 클래스만을 위한 빈을 생성할 수 있다. 코드를 덜 작성해 편하게 읽을 수 있다.

- registerBean() 메서드를 사용하면 스프링 컨텍스트에 빈을 추가하는 로직을 재정의해 구현함. 이 방식은 스프링 5 이상에서 가능하다.
