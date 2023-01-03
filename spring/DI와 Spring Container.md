# DI와 SpringContainer
### 제어의 역전 IoC Inversion of Control
- 프레임워크처럼 별도의 객체가 제어를 담당, 즉 외부에서 관리
- 스프링은 컨테이너가 빈관리를 하기 때문에 제어의 역전이라고 표현
- 프레임워크 : 코드를 제어하고 실행
- 라이브러리 : 내 코드가 직접 제어의 흐름을 담당
<br/>

### 의존관계 주입 DI Dependency Injection
- 정적인 클래스 의존관계 : import 코드만 보고 의존관계 쉽게 판단가능
- 실행 시점에 결정되는 동적인 객체 의존관계 : 별도의 객체에 정의되어 보이지 않음
<br/>

### 스프링 컨테이너
- 객체들을 Bean형태로 저장하여 관리하는 인터페이스, 빈이름은 중복이 안됨
- xml 또는 어노테이션 기반으로 생성가능
- 어노테이션 기반
  ``` java
  // 스프링 컨테이너 생성
  AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

  ac.getBean("빈이름", 빈타입.class);
  ```
- xml 기반
  ``` xml
  // 스프링 컨테이너 생성
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://
                 www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
      <constructor-arg name="memberRepository" ref="memberRepository" />
    </bean>
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository" />
  </beans>
  ```
  ``` java
  // 스프링 컨테이너 생성
  ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");

  ac.getBean("빈이름", 빈타입.class);
  ```
<br/>

### BeanFactory와 ApplicationContext
- BeanFactory를 상속받아 ApplicationContext 사용
- Application 제공 부가기능
  - ApplicationContext는 다양한 기능 상속받아 사용
  - MessageSource : 한국은 한국어, 영어권은 영어로 출력
  - EnvironmentCapable : 로컬, 개발, 운영등 환경별 처리
  - AppplicationEventPublisher : 이벤트 발행 구독하는 모델 지원
  - ReosourceLoader : 파일, 클래스패스등 리소스 조회
