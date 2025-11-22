---
title: Spring과 DispatcherServlet의 동작 흐름
date: 2025-03-22 00:00:00 +0900
description: Spring과 DispatcherServlet의 동작 흐름
categories: [Spring]
tags: [spring]     # TAG names should always be lowercase
pin: false
math: true
mermaid: true
image:
  path: /assets/img/20250322/spring_dispatcherservlet_flow/DispatcherServlet-Flow.jpg
---

최근 규모 있는 레거시한 스프링프레임워크를 접하면서, 최대한 적응하고자 스프링프레임워크에 대해서 공부하게 되었다. 단순히 Spring에서 Deafult로 제공하는 사용법만 익혀왔던 전과는 달리, 스프링프레임워크를 직접 구현하면서 왜 Spring이 Enterprise급 애플리케이션의 프레임워크라고 하는지,  스프링에 대한 철학을 조금이나마 이해하는 시간을 가졌다.



이번 포스팅에서는 스프링프레임워크의 FrontContrller인 DispatcherServlet의 흐름에 대해서 정리하고자 한다.

​    

## 1. 스프링 프레임워크란?

---

스프링 프레임워크는 기존 서블릿 기반 개발 방식에서 발생하던 반복적인 코드와 설정 작업을 줄이고, 효율적인 애플리케이션 개발을 지원하기 위해 등장했다.

핵심 개념은 IoC(제어의 역전)/DI(의존성 주입), 서비스 추상화(PSA), 관점 지향 프로그래밍(AOP)이다. 

1. **IoC(제어의 역전)/DI(의존성 주입)**

   스프링은 객체의 생성부터 소멸까지 전반적인 생명주기를 관리하고, 필요한 의존 관계를 직접 주입해 줌으로써 개발자가 더 효율적으로 개발할 수 있도록 한다. IoC와 DI를 통해 객체 관리의 부담을 프레임워크가 대신 맡아주기 때문에, 개발자는 핵심 로직에 집중할 수 있다.

   싱글톤 패턴은 자칫하면 안티패턴으로 이어질 수 있지만, 스프링에서 요구하는 방식대로 사용하면 쉽고 이상적으로 싱글톤 패턴을 사용할 수 있다.

2. **서비스 추상화(PSA)**

   PSA(Portable Service Abstraction)는 다양한 기술을 일관된 방식으로 사용할 수 있도록 스프링이 제공하는 추상화 계층이다. (그냥 잘 만들어진 인터페이스이다. 우리는 별다른 고민 없이 그냥 가져다 쓰면 된다.)

   트랜잭션을 적용할 때 JDBC는 DataSourceTransactionManager를, JPA는 JpaTransactionManager를 사용하는 등 기술마다 트랜잭션 처리 방식이 모두 다르다. 하지만 스프링은 PlatformTransactionManager라는 하나의 공통 인터페이스로 이들을 추상화했기 때문에, 개발자는 각 기술별 내부 구현을 직접 다룰 필요 없이 @Transactional 어노테이션 하나만 붙이면 쉽게 트랜잭션을 사용할 수 있다.

   또한 연결된 DB 종류(MySQL, Oracle 등)에 따라 발생하는 오류 메시지는 제각각이지만, 스프링은 이를 DataAccessException으로 추상화하여 일관된 방식의 예외 처리를 가능하게 한다. 이처럼 PSA는 기술에 대한 종속성을 줄이고, 개발자가 핵심 로직에 집중할 수 있도록 돕는다.

3. **AOP(관점 지향 프로그래밍)**

   AOP(Aspect-Oriented Programming)를 통해 로깅, 트랜잭션 처리 등과 같은 공통 관심사를 핵심 비즈니스 로직으로부터 명확히 분리할 수 있다. AOP는 메소드 실행 전/후에 공통 로직을 삽입하는 방식으로 동작한다. 하지만 사용자가 작성한 원본 코드를 프레임워크에서 직접 수정할 수 없기에, AOP와 같은 부가 기능을 실행하는 데 구조적 한계가 있다. 

   

   이를 해결하기 위해 스프링은 바이트코드를 조작한 프록시 객체를 생성하여, 실제 객체를 감싼 프록시를 통해 AOP 대상 메소드 호출을 가로챈다. 프록시는 메소드 흐름 자체를 가로채고 제어할 수 있기 때문에, 대상 메소드가 실행되기 전과 후에 원하는 공통 기능을 적용할 수 있다. 이러한 방식 덕분에 핵심 비즈니스 코드를 수정하지 않고도 필요한 공통 기능을 유연하게 관리할 수 있다.

​    

스프링은 이처럼 세 가지 핵심 개념을 바탕으로 코드의 중복을 줄이고, 애플리케이션의 구조 단순화 하며, 개발자가 핵심에만 집중할 수 있는 환경을 제공한다.

​    

## 2. Dispatcher-Servlet이란?
---

Spring의 DispatcherServlet은 기존 서블릿 아키텍처의 문제점을 개선한 Front Controller 패턴 기반의 Servlet 구현체이다. DispatcherServlet은 모든 HTTP 요청을 하나의 진입점에서 받아 URL 매핑을 통해 적절한 handler(컨트롤러)로 분기 처리한다.

기존 서블릿 방식에서는 기능별로 별도의 서블릿 객체를 등록해야 했으며, 이로 인해 다음과 같은 문제점이 있다.

1. **요청 매핑의 번거로움** : URL마다 서블릿을 직접 구현하거나, 내부 분기로 처리해야 함

2. **공통 처리 로직의 중복** : 인증, 로깅, 예외 처리 등을 각 서블릿마다 중복 구현

3. **요청 처리 과정의 일관성 부족** : 파라미터 추출 → 로직 실행 → 뷰 연결 흐름이 서블릿마다 제각각

4. **높은 결합도** : 비즈니스 로직과 View로직이 섞이기 쉬운 구조

5. **확장성 부족** : 공통 흐름 개입이 어렵다

​      

### 2.1 DispatcherServlet의 도입으로 인한 개선점

Spring MVC는 FrontController패턴의 DispatcherServlet을 도입하여, 다음과 같은 이점을 얻을 수 있었다.

1. **요청 매핑의 단순화** : URL을 메서드 및 HTTP Method 단위로 유연하게 매핑 가능

2. **공통 로직 처리** : 공통 기능을 일관되게 적용하기 쉬워짐

3. **요청 처리 과정의 일관성** : 모든 요청이 동일한 처리 흐름을 따름 

4. **명확한 역할 분리 (MVC 구조)** : 로직과 화면 처리가 명확히 구분되어, MVC 구조가 자연스럽게 구현됨

5. **확장 용이성** : OCP 원칙을 준수하며 각 컴포넌트를 확장하거나 교체하기 쉬움 

​      

Spring의 DispatcherServlet은 요청을 중앙에서 관리하며, 구조적 일관성과 유연한 확장을 가능하게 만들어 Spring MVC의 핵심 기반이 된다.

​    

## 3. DispatcherServlet의 동작 과정

스프링의 DispatcherServlet은 웹 애플리케이션의 핵심 진입점으로, ServletContainer로부터 사용자 요청을 전달받아 일관된 처리 흐름에 따라 동작한다.
각 요청은 DispatcherServlet 내부의 여러 컴포넌트를 거쳐 순차적으로 처리되며, 컴포넌트 간 책임이 명확하게 분리되어 있다.   

아래는 DispatcherServlet이 요청을 받아 주요 컴포넌트를 통해 처리하는 흐름이다.

![Process](/assets/img/20250322/spring_dispatcherservlet_flow/DispatcherServlet-Flow.jpg)

_**[1]**_ 클라이언트가 HTTP 요청을 보낸다.

_**[2]**_ HandlerMapping에게 Request URL을 넘겨, 해당 요청을 처리할 수 있는 적절한 handler(컨트롤러)를 찾는다.

_**[3]**_ HandlerAdapter목록에서 handler(컨트롤러)를 실행할 수 있는 적절한 Adapter를 찾는다.

_**[4]**_ DispatcherServlet은 찾은 HandlerAdapter에게 handler(컨트롤러)의 실행을 위임한다.

_**[5]**_ handler(컨트롤러)의 실제 로직을 수행하고, 그 결과를 반환한다.

_**[6]**_ 실행한 View에 대한 정보와 Model 값을 반환한다.

_**[7]**_ HandlerAdapter는 ModelAndView를 다시 DispatcherServlet에 반환한다.

_**[8]**_ DispatcherServlet은 ViewResolver를 통해 ModelAndView의 View 이름으로부터 실제 View 객체를 조회한다.

_**[9]**_ DispatcherServlet은 조회된 View 객체를 통해 화면을 렌더링한다.

_**[10]**_ 최종적으로 클라이언트에게 렌더링 된 결과를 보낸다

​      

## 4. DispatcherServlet Component
DispatcherServlet의 내부 컴포넌트는 인터페이스로 정의되어 있으며, 각 컴포넌트는 그 인터페이스를 구현한 구현체 형태로 동작한다. 개발자는 원하는 구현체를 추가하거나 교체하여 애플리케이션을 유연하게 확장할 수 있으며, 각 컴포넌트는 그 역할과 책임이 명확히 구분되어 있다.

예를 들어, 요청 URL을 핸들러와 매핑하는 HandlerMapping 인터페이스에는 어노테이션 기반의 매핑을 지원하는 RequestMappingHandlerMapping, 빈 이름 기반의 매핑을 지원하는 BeanNameUrlHandlerMapping 등 다양한 구현체가 존재한다.


Spring MVC에 사용되는 주요 컴포넌트들은 다음과 같다.

**HandlerMapping**

- 클라이언트의 요청 URL을 보고, 요청을 처리할 핸들러(컨트롤러)를 찾아준다.

**HandlerAdapter**

- 핸들러를 실제로 실행시키기 위한 어댑터 역할을 한다.

**ArgumentResolver**

- 핸들러가 필요로 하는 파라미터를 요청으로부터 분석하여 전달한다.

**ReturnValueHandler**

- 핸들러의 반환 값을 분석하여 ModelAndView와 같은 응답 형식으로 변환한다.

**ViewResolver**

- 핸들러가 반환한 뷰 이름(ViewName)을 이용해 실제 뷰 객체로 변환하여 화면에 렌더링한다.

   

컴포넌트화 된 구조 덕분에 개발자가 직접 커스텀한 구현체를 만들어 기존 전략을 대체하거나 추가할 수도 있다. 즉, 인터페이스를 통해 각 컴포넌트의 확장 포인트(Extension point)를 제공하기 때문에, 개발자는 핵심 비즈니스 로직을 건드리지 않고도 원하는 기능만 선택적으로 바꾸거나 확장할 수 있게 된다.

덕분에 개발자는 핵심 비즈니스 로직을 수정하지 않고도 원하는 동작을 확장할 수 있다. 스프링을 잘 사용한다면 OCP(개방-폐쇄 원칙)를 준수하는 애플리케이션을 설계할 수 있다. "그냥 필요한 것만 만들어서 끼워 넣으면 된다"는 이 구조 덕분에, 스프링 프레임워크는 복잡하지만, 이를 통해 유연한 애플리케이션을 만들 수 있다.