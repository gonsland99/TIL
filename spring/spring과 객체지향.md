# Spring과 객체지향
### Spring 역사
- 스프링 출시 이전 : EJB 방식으로 개발, 복잡하고 종속적인 문제
- EJB(Enterprise Java Bean)
 - 엔터프라이즈급 자바개발 단순화를 위한 개발모델
 - 장점 : 분산 트랜잭션
 - 단점 : 느림, 비쌈, 복잡
- 하이버네이트
  - 자바언어를 위한 ORM 프레임워크로 JPA를 표준인터페이스로 두고 구현
  - EJB 엔티티빈 기술을 대체
<br/>

### Spring 이란
- 객체지향적 자바코드를 용이하게 개발하기 위한 기능들을 제공하는 프레임워크
- 구성 : 스프링, 스프링부트, 데이터, 세션, 시큐리티, 클라우드, 배치, RestDocs등…
- 기술 : DI, AOP, 이벤트, MVC, 트랜잭션, JDBC
- 단점 : xml 설정이 복잡
<br/>

### Spring Boot 란
- 스프링의 설정등 더쉽고 용이하게 만들어진 프레임워크
- omcat같은 웹서버 내장되어 별도 설치 불필요
- 라이브러리와 연관된 라이브러리들도 같이 주입됨
<br/>

### 좋은 객체지향
- 유연한 변경을 위해 객체 설계 시 역할(interfaace)과 구현(구현 class)을 분리
- solid원칙을 기반으로 좋은 interface 설계 필요
<br/>

### SOLID 원칙
- 단일책임 원칙 SRP(Single Responsiblity Principle) : 한 클래스는 하나의 책임만 가진다
- 개방/폐쇄 원칙 OCP(Open/Closed Principle) : 소프트웨어 요소는 확장에는 열려있고 변경에는 닫혀 있어야함
  ``` java
  //DBConnection 인터페이스를 구현한 클래스들을 교체 시
  //코드 수정(변경)이 있으면 OCP원칙 위반
  DBConnection jdbc = new MysqlConnection();
  //DBConnection jdbc = new OracleConnection(); 
  ```
        
- 리스코프 치환 원칙 LSP(Liskov Substitution Principle) : 객체는 역할을 변경하지 않고 하위 타입 객체로 변경 가능 해야 함
- 인터페이스 분리 원칙 ISP(Interface Segregation Principle) : 특정 클라이언트를 위한 인터페이스 여러개가 범용 인터페이스 하나보다 좋음
- 의존관계 역전 원칙 DIP(Dependency Inversion Principle) : 추상화에 의존하고 구체화에 의존하면 안됨
  ``` java
  //DBConnection 인터페이스에는 의존하되
  //MysqlConnection 구체화된 객체에는 의존하면 DIP원칙 위반
  DBConnection jdbc = new MysqlConnection();
  ```
    
