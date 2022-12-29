# 싱글톤
### 웹 어플리케이션의 특성
- 클라이언트의 동일한 기능 요청 시 주소값이 다른 객체 생성 > 메모리 자원 낭비
 ``` java
 void pureContainer() {
   // AppConfig 객체 생성 
   AppConfig appConfig = new AppConfig();
   // 여러 고객들이 동일한 기능 요청 시 주소값이 다른 객체 생성 
   MemberService memberService1 = appConfig.memberService();
   MemberService memberService2 = appConfig.memberService();

   //memberService1 != memberService2
   assertThat(memberService1).isNotSameAs(memberService2);
 }
 ```
<br/><hr/>

### 싱글톤
- 동일한 기능 요청 시 동일한 주소값의 하나의객체를 참조 > 메모리 효율적 관리
  ``` java
  public class SingletonService {
    private static final SingletonService instance = new SingletonService();
    //해당 인스턴스메서드를 통해서만 객체 생성 가능
    public static SingletonService getInstance() {
     return instance;
    }
    // 생성자를 private으로 선언하여 외부에서 객체직접생성을 막음
    private SingletonService() { }
  }
  ```
- 싱글톤 단점
  - 코드량이 추가됨
  - DIP, OCP 원칙을 위반
  - 내부속성을 변경하기 힘듬, 유연성 비효율적   

<br/><hr/>

### 싱글톤(스프링) 컨테이너
- 스프링 컨테이너에 등록된 빈들은 싱글톤으로 자동관리
 ``` java
  @Test
  @DisplayName("스프링 컨테이너와 싱글톤")
  void springContainer() {
    // spring container에 등록된 클래스 호출
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    // 주소값이 같은 객체를 호출
    MemberService memberService1 = ac.getBean("memberService", MemberService.class);
    MemberService memberService2 = ac.getBean("memberService", MemberService.class);
    //참조값이 같은 것을 검증
    System.out.println("memberService1 = " + memberService1);
    System.out.println("memberService2 = " + memberService2);
    assertThat(memberService1).isSameAs(memberService2);
  }

 ```
<br/><hr/>

### 싱글톤 방식의 주의점(상태유지)
- 하나의 객체를 참조하는 패턴이기 때문에 변수로 값을 저장해버리면 원하는 값이 안나오는 현상발생
 ``` java
 public class StatefulService {
   private int price;
   public void order(String name, int price) {
     System.out.println("name = " + name + " price = " + price);
     this.price = price; // 생성 시 price값이 계속 업데이트되어 저장되는 문제 발생
     //return price; 하여 객체를 생성한 페이지에서 변수로 값을 저장하여 사용하는게 효율적
   }
   public int getPrice() {
    return price;
   }
 }
 ```
 ``` java
 public class StatefulServiceTest {
  @Test
  void statefulServiceSingleton() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
    StatefulService statefulService1 = ac.getBean("statefulService", StatefulService.class);
    StatefulService statefulService2 = ac.getBean("statefulService", StatefulService.class);
    //ThreadA: A사용자 10000원 주문, B사용자 20000원 주문
    statefulService1.order("userA", 10000);
    statefulService2.order("userB", 20000);
    //ThreadA: 사용자A 주문 금액 조회 시 10000원을 기대했지만, 기대와 다르게 20000원 출력
    int price = statefulService1.getPrice();
    Assertions.assertThat(statefulService1.getPrice()).isEqualTo(20000);
  }
  static class TestConfig {
    @Bean
    public StatefulService statefulService() {
    return new StatefulService();
    }
  }
 }
 ```
<br/><hr/>

### @Configuration 과 싱글톤
- AppConfig의 Bean에 등록된 메서드 호출 시 다른 주소값의 memberRepository객체가 생성되는 문제 발생
- 싱글톤으로 유지하기 위해선 @Configuration 어노테이션을 이용해 spring container가 관리하도록 설정 
  ``` java
  @Configuration
  public class AppConfig {
    @Bean
    public MemberService memberService() {
     return new MemberServiceImpl(memberRepository());
    }
    @Bean
    public OrderService orderService() {
     return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    @Bean
    public MemberRepository memberRepository() {
      return new MemoryMemberRepository();
    }
    ...
  }
  ```
 

