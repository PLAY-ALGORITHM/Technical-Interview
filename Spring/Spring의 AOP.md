# :wink: Spring의 AOP란?
----------------------
## :zap: **AOP 등장 배경**
- 객체지향 프로그래밍(OOP: Object-Oriented Programming) 에선 공통된 기능을 재사용하는 방법으로 상속이나 위임을 사용합니다.
- 하지만 전체 어플리케이션에서 여기저기에서 사용되는 부가기능들을 상속이나 위임으로 처리하기에는 깔끔하게 모듈화가 어렵습니다.
- 그래서 이 문제를 해결하기 위해 AOP가 등장하게 된 것입니다.

## :question: AOP란?

- **Aspect Oriented Programming**의 약자로 **관점 지향 프로그래밍**이라고 불립니다.
- 관점 지향은 쉽게 말해 **어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것입니다**. 
- 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말합니다.  
- Spring은 자체적으로 AOP를 지원하고 있기 때문에 트랜잭션이나 로깅, 보안과 같이 여러 비즈니스 모듈에서 공통적으로 필요로 하는 공통 관심 사항을 핵심 로직과 분리시켜 각 모듈에 적용할 수 있습니다.

 ```java
흩어진 코드를 한곳으로 모아

class A {
	method a( ) {
		AAAA
		오늘은 7월 4일 미국 독립 기념일이래요.
		BBBB
	}
	
	method b() {
		AAAA
		저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
		BBBB
	}	
}

class B {
	method c( ) {
		AAAA
		점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
		BBBB
	}
}

모아놓은 AAAA와 BBBB

class A {
	method a( ) {
		오늘은 7월 4일 미국 독립 기념일이래요.
	}
	
	method b() {
		저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
	}	
}

class B {
	method c( ) {
		점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
	}
}

class AAAABBBB {
	method aaaabbbb(JoinPoint point) {
		AAAA
		point.execute()
		BBBB
	}
}
 ```
	### **AOP하면   '공통된 기능을 재사용' 하는 기법!**
	
## :question: 핵심기능 관점과 부가기능 관점


- **핵심 기능 관점**은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 됩니다.  
- **부가 기능 관점**은 핵심 기능 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있습니다.
### **< 핵심 기능 관점 >**

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile4.uf.tistory.com/image/273B244458496A0A1B7AC5)

### **< 부가 기능 관점 >**

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile3.uf.tistory.com/image/2473C33D58496A2A0F6DF9)

## :question: OOP vs AOP 요약
-   OOP : 비지니스 로직의 모듈화
    -   모듈화의 핵심 단위는 비지니스 로직 (**핵심 로직** - 구매와 판매)
-   AOP : 인프라 혹은 부가기능의 모듈화
    -   대표적 예 : 로깅, 트랜잭션, 보안 등
    -   각각의 모듈들의 주 목적 외에 필요한 부가적인 기능들을 수행하는 **공통로직** (핵심이 아닌 공통으로 사용되는 로직)

## :question: 핵심 로직과 공통 로직 적용 시점

- 공통 로직 반영 시점은 각 핵심 로직들이 실행 전, 실행 후 적용된다.

- before : 핵심로직 실행전, 전처리
- after : 핵심로직 실행후, 후처리
- throwing : 예외 발생 - 핵심로직 실행중 예외 발생
- returning : 핵심 로직 정상 실행되면서 반환시 - 후처리
- around : before,after,throwing,returning 포괄적인 설정 가능

## :zap: **AOP 장점**

-   어플리케이션 전체에 흩어진 공통 기능이 하나의 장소에서 관리된다는 점
-   다른 서비스 모듈들이 본인의 목적에만 충실하고 그외 사항들은 신경쓰지 않아도 된다는 점

## :zap: **다양한 AOP 구현 방법**
- 컴파일  : A.java --- (AOP) ---> A.class (AspectJ)
- 클래스 로드시 (바이트코드 조작) : A.java -> A.class --- (AOP) --> 메모리
- 프록시 패턴 : (Spring AOP가 사용하는 방법)
(프록시 패턴 내용 참고 : Real-World Analogy  https://refactoring.guru/design-patterns/proxy)

## :zap: **AOP 용어**
아래 용어들은 Spring에서만 사용되는 용어들이 아닌 **AOP 프레임워크 전체에서 사용되는 공용어** 입니다.

**타겟 (Target)**  
부가기능을 부여할 대상을 얘기합니다.  
여기선 핵심기능을 담당하는 getBoards 혹은 getUsers를 하는 Service 들을 얘기합니다.  
  
**애스펙트 (Aspect)**  
객체지향 모듈을 오프젝트라 부르는것과 비슷하게 부가기능 모듈을 애스펙트라고 부르며, **핵심기능에 부가되어 의미를 갖는** 특별한 모듈이라 생각하시면 됩니다.  
애스펙트는 부가될 기능을 정의한 **어드바이스 (Advice)**와 어드바이스를 어디에 적용할지를 결정하는 **포인트컷(PointCut)**을 함께 갖고 있습니다.  
참고로 AOP(Aspect Oriented Programming)라는 뜻 자체가 애플리케이션의 핵심적인 기능에서 부가적인 기능을 분리해서 애스팩트라는 독특한 모듈로 만들어서 설계하고 개발하는 방법을 얘기합니다.  
  
**어드바이스 (Advice)**  
실질적으로 부가기능을 담은 구현체를 얘기합니다.  
어드바이스의 경우 타겟 오프젝트에 종속되지 않기 때문에 순수하게 **부가기능에만 집중**할 수 있습니다.  
어드바이스는 애스펙트가 '무엇'을 '언제' 할지를 정의하고 있습니다.  
  
**포인트컷 (PointCut)**  
부가기능이 적용될 대상(메소드)를 선정하는 방법을 얘기합니다.  
즉, 어드바이스를 적용할 조인포인트를 선별하는 기능을 정의한 모듈을 애기합니다.  
  
**조인포인트 (JoinPoint)**  
어드바이스가 적용될 수 있는 위치를 얘기합니다.  
다른 AOP 프레임워크와 달리 Spring에서는 **메소드 조인포인트만 제공**하고 있습니다.  (무조건 메소드 단위로만 지정)  
따라서 Spring 프레임워크 내에서 조인포인트라 하면 **메소드**를 가리킨다고 생각하셔도 됩니다.  
  
**프록시 (Proxy)**  
타겟을 감싸서 타겟의 요청을 대신 받아주는 랩핑(Wrapping) 오브젝트입니다.  
호출자 (클라이언트)에서 타겟을 호출하게 되면 타겟이 아닌 타겟을 감싸고 있는 프록시가 호출되어, 타겟 메소드 실행전에 선처리(Before), 타겟 메소드 실행 후, 후처리(After)를 실행시키도록 구성되어있습니다.

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile29.uf.tistory.com/image/2715394658496A61010951)
**AOP에서 프록시는 호출을 가로챈 후, 어드바이스에 등록된 기능을 수행 후 타겟 메소드를 호출합니다.**

**인트로덕션 (Introduction)**  
타겟 클래스에 코드 변경없이 신규 메소드나 멤버변수를 추가하는 기능을 얘기합니다.  
자세한 설명은 [자바지기님의 포스팅](http://www.javajigi.net/pages/viewpage.action?pageId=1084) 참고  
  
**위빙 (Weaving)**  
지정된 객체에 애스팩트를 적용해서 새로운 프록시 객체를 생성하는 과정을 얘기합니다.  
예를 들면 A라는 객체에 트랜잭션 애스팩트가 지정되어 있다면, A라는 객체가 실행되기전 커넥션을 오픈하고 실행이 끝나면 커넥션을 종료하는 기능이 추가된 프록시 객체가 생성되고, 이 프록시 객체가 앞으로 A 객체가 호출되는 시점에서 사용됩니다. 이때의 프록시객체가 생성되는 과정을 **위빙**이라 생각하시면 됩니다.  
컴파일 타임, 클래스로드 타임, 런타임과 같은 시점에서 실행되지만, Spring AOP는 런타임에서 프록시 객체가 생성 됩니다.


## :question:  **Spring AOP 주요 개념**
----------------
**AOP**에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미다. 이때, 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을  **횡단 관심사 (Crosscutting Concerns)** 라 부른다.  
  
위와 같이 흩어진 관심사를 **Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지**입니다.  
     <center>![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile26.uf.tistory.com/image/994AA3335C1B8C9D28D24B ) 
</center>

-   **Aspect** : 위에서 설명한 **횡단 관심사를 모듈화 한 것**. **주로 부가기능을 모듈화함.**
-   **Target** :  **어떤 대상에 부가 기능을  부여할 것인가?**  -> Aspect를 적용하는 곳 (클래스, 메서드 .. )
-   **Advice** : **어떤 부가기능? Before, AfterReturning, After Throwing, After, Around** 
-   **JointPoint** : **어디에 적용할 것인가?** **메서드 레벨만 지원** 
-   **PointCut** : **실제 advice가 적용될 지점, Spring AOP에서는 advice가 적용될 메서드를 선정**
  -   **weaving** : **AOP를 끼워주는 시기  -> 런타임시에만 가능** -> Spring Container가 객체 생성을 관리해주기 때문

|  | Spring AOP | AspectJ
|--|--|--|
| 목표 | 간단한 AOP 기능 제공 | 완벽한 기능 제공 |
| join point | 메서드 레벨만 지원 | 생성자, 필드, 메서드 등 다양하게 지원 |
| weaving | 런타임시에만 가능 | 런타임은 제공하지 않음. compile-time, post-compile, load-time 제공  |
| 대상 |Spring Container가 관리하는 Bean에만 가능  | 모든 JAVA Object에 가능 |



## :question:  AOP Advice 종류
| Advice 종류 |XML스키마 기반의 POJO 클래스를 이용  |@Aspect애노테이션 기반  |설명|
|--|--|--|--|
|Before|\< aop:before \>|@Before|target 객체의 메소드 호출시 호출전에 실행|
|AfterReturning|\<aop:after-returning\>|@AfterReturning|target 객체의 메소드가 예외없이 실행된 후 호출|
|AfterThrowing|\<aop:after-throwing\>|@AfterThrowing|target객체의 메소드가 실행하는 중 예외가 발생한 경우에 호출|
|After|\<aop:after\>|@After|target 객체의 메소드를 정상 또는 예외 발생 유무와 상관없이 실행<br> try의 finally와 흡사|
|Around|\<aop:around\>|@Around|target객체의 메소드 실행 전, 후 또는 예외 발생 시점에 모두 실행해야할 로직을 담아야 할 경우|

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile26.uf.tistory.com/image/2379DB3E58496AA7079CEF)

@Around는 **어드바이스**입니다.  
앞서 설명드린것 처럼 어드바이스는 애스펙트가 "무엇을", "언제" 할지를 의미하고 있습니다.  
여기서 "무엇"은 calculatePerformanceTime() 메소드를 나타냅니다.  
그리고 "언제"는 @Around가 되는데, 이 언제 라는 시점의 경우 @Around만 존재하지 않고 총 5가지의 타입이 존재합니다.
  
-   @Before (이전)
    -   어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
-   @After (이후)
    -   타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료 되면 어드바이스 기능을 수행
-   @AfterReturning (정상적 반환 이후)
    -   타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
-   @AfterThrowing (예외 발생 이후)
    -   타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
-   @Around (메소드 실행 전후)
    -   어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출전과 후에 어드바이스 기능을 수행

여기서 주의하실 점은 @Around의 경우 **반드시 proceed() 메소드가 호출**되어야 한다는 것입니다.  
proceed() 메소드는 타겟 메소드를 지칭하기 때문에 proceed 메소드를 실행시켜야만 타겟 메소드가 수행이 된다는것을 잊으시면 안됩니다.

다음으로 알아볼것은 @Around 다음에 작성된  `"execution(* com.blogcode.board.BoardService.getBoards(..))"`  입니다.  
어드바이스의 value로 들어간 이 문자열을  **포인트컷 표현식**  이라고 합니다.  
포인트컷 표현식은 2가지로 나눠지는데,  `execution`을  **지정자**라고 부르며  
`(* com.blogcode.board.BoardService.getBoards(..))`는  **타겟 명세**라고 합니다.  
포인트컷 지정자는 execution을 포함하여 총 9가지가 있습니다.  

-   args()
    -   메소드의 인자가 타겟 명세에 포함된 타입일 경우
    -   ex) args(java.io.Serializable) : 하나의 파라미터를 갖고, 그 인자가 Serializable 타입인 모든 메소드
-   @args()
    -   메소드의 인자가 타겟 명세에 포함된 어노테이션 타입을 갖는 경우
    -   ex) @args(com.blogcode.session.User) : 하나의 파라미터를 갖고, 그 인자의 타입이 @User 어노테이션을 갖는 모든 메소드 (@User User user 같이 인자 선언된 메소드)
-   execution()
    -   접근제한자, 리턴타입, 인자타입, 클래스/인터페이스, 메소드명, 파라미터타입, 예외타입 등을 전부 조합가능한 가장 세심한 지정자
    -   이전 예제와 같이 풀패키지에 메소드명까지 직접 지정할 수도 있으며, 아래와 같이 특정 타입내의 모든 메소드를 지정할 수도 있다.
    -   ex) execution(* com.blogcode.service.AccountService.*(..) : AccountService 인터페이스의 모든 메소드
-   within()
    -   execution 지정자에서 클래스/인터페이스까지만 적용된 경우
    -   즉, 클래스 혹은 인터페이스 단위까지만 범위 지정이 가능하다.
    -   ex) within(com.blogcode.service.*) : service 패키지 아래의 클래스와 인터페이스가 가진 모든 메소드
    -   ex) within(com.blogcode.service.._) : service 아래의 모든 *_하위패키지까지** 포함한 클래스와 인터페이스가 가진 메소드
-   @within()
    -   주어진 어노테이션을 사용하는 타입으로 선언된 메소드
-   this()
    -   타겟 메소드가 지정된 빈 타입의 인스턴스인 경우
-   target()
    -   this와 유사하지만 빈 타입이 아닌 타입의 인스턴스인 경우
-   @target()
    -   타겟 메소드를 실행하는 객체의 클래스가 타겟 명세에 지정된 타입의 어노테이션이 있는 경우
-   @annotation
    -   타겟 메소드에 특정 어노테이션이 지정된 경우
    -   ex) @annotation(org.springframework.transaction.annotation.Transactional) : Transactional 어노테이션이 지정된 메소드 전부

다양한 지정자가 등장하였지만 실제로 execution과 @annotation을 주로 사용하는 것으로 알고 있습니다.
## :zap: **Spring AOP 기본 실습**

### :balloon: 실습 _ Step04_AOP  pom.xml
  
```xml
<properties>

		<!-- Generic properties -->
		<java.version>1.8</java.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- Spring -->
		<spring-framework.version>4.3.30.RELEASE</spring-framework.version>

	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>
		
		<!-- AOP 적용시에 필요한 추가 library 
			- byte code를 실시간 동적으로 자동 생성해주는 기능의 library
		-->	
		<dependency>
			<groupId>cglib</groupId>
			<artifactId>cglib</artifactId>
			<version>2.2.2</version>
		</dependency>
		
		<!-- AOP 기능의 framework
			- spring이 AOP 기술을 활용해서 spring스럽게 활용
		 -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.7.3</version>
		</dependency>
		
	</dependencies>
```
### :balloon: 실습 _ Step04_AOP  aop.xml
 
```xml
	<!-- aop 사용을 위한 필수 설정 -->
	<aop:aspectj-autoproxy />
	
	<!-- 핵심(biz, core)과 공통(aspect)을 스프링 빈으로 등록 -->
	<bean id="biz" class="step02.biz.aop.Car" />
	
	<bean id="common" class="step02.common.aop.NoticeAspect" />
	
	<!-- 핵심로직 실행 전, 후 공통 로직 적용
		핵심의 어떤 메소드들에 공통 로직 적용할지도 결정을 해야함
		어떤 핵심 로직의 메소드에 어떤 공통로직의 메소드를 정해진 시점에 적용
		spring은 aspectj라는 framework 활용
		 - 기능을 메소드에만 적용 + 표현법은 그대로 사용
		  
	 -->
	 
	 <aop:config>
	 	<aop:pointcut id="bizLogic" expression="execution(* step02.biz.aop.Car.buy*(..))"/>
	 	
	 	<aop:aspect ref="common">
	 		<aop:before method="noticBuyStart" pointcut-ref="bizLogic"/>
	 		<aop:after method="noticBuyEnd" pointcut-ref="bizLogic"/>
	 		
<!-- <aop:before method="noticeBuyStart" pointcut="execution(* step02.biz.Car.buy*(..))" />
	 		<aop:after method="noticBuyEnd" pointcut="execution(* step02.biz.Car.buy*(..))" /> -->
	 	</aop:aspect>
	 </aop:config>
```

### :balloon: 실습 _ Step04_AOP  NoticeAspect.java

```java
package step02.common.aop;

//공통 기능의 클래스
public class NoticeAspect {
	//구매 전 처리 공통 로직
	public void noticBuyStart() {
		System.out.println("[공통 1] 구매를 시작 하실 건가요?");
	}
	
	//구매 후 처리 공통 로직
	public void noticBuyEnd() {
		System.out.println("[공통 2] 구매를 완료 하셨습니다");
	}
	
}
```
### :balloon: 실습 _ Step04_AOP  Car.java
```java
package step02.biz.aop;

//구매, 판매 로직의 핵심 기능이라 가정
public class Car {

	public void buy() {
		System.out.println("자동차 구매"); 					//biz logic
	}
	
	public void buyMoney(int money) {
		System.out.println("자동차 구입한 금액 " + money); 		//biz logic
	}


	public void buy2() {
		System.out.println("자동차 구매");
	}
	
	public void buyMoney2(int money) {
		System.out.println("자동차 구입한 금액 " + money);
	}
	
	public String buyReturn() {
		return "자동차 구매 성공";
	}
	
	//판매 금액이 1000만원 이하인 경우에 예외 발생
	public void saleMoney(int money) throws Exception {
		if(money <= 1000) {
			throw new Exception("너무 싸!!!");
		}
	}
}
```
### :balloon: 실습 _ Step04_AOP  Step02Test.java
```java
package running;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import step02.biz.aop.Car;

public class Step02Test {

	public static void main(String[] args) {
		
		ApplicationContext context = new ClassPathXmlApplicationContext("aop.xml");
		Car car = context.getBean("biz", Car.class);
		
		System.out.println("*** 경우의 수 1 : Car의 각 메소드에 모든 로직을 구현한 경우 ***");
		car.buy();
		car.buyMoney(10);

		System.out.println("*** 경우의 수 2 : Car의 각 메소드에서 별도로 구분해서 구현한 공통 로직 클래스의 메소드를 직접 호출한 경우 ***");
		car.buy2();
		car.buyMoney2(10);
	}
}

```
### :balloon: 실습 _ Step04_AOP  Step02Test.java 실행결과
![step02Testjava](https://user-images.githubusercontent.com/37354978/106272809-fde05d00-6274-11eb-8128-adfafbefba9b.PNG)

## :zap: **Spring AOP Schema 기반 실습-throwing,returning**
 \<aop:after-throwing\> \<aop:after-returning\>
### :balloon: 실습 _ Step05_AOPSchema  playdata.xml

```xml
<aop:aspectj-autoproxy />
	
	<bean id="car" class="step01.biz.schema.Car" />
	<bean id="common" class="step01.common.schema.NoticeAspect" />
	
	<aop:config>
		<!-- sale~ 하는 메소드만 예외처리 설정 -->
		<aop:pointcut  id="bizLogic" expression="execution(* step01.biz.schema.Car.sale*(int))"/>

		<aop:aspect ref="common">
			<!-- public void noticException(Exception e) {} --> 
			<aop:after-throwing method="noticException" throwing="e" pointcut-ref="bizLogic" />
			
			<!-- public void noticReturnValue(Object value)  -->
			<aop:after-returning method="noticReturnValue" returning="value" pointcut="execution(* step01.biz.schema.Car.buyReturn(..))"/> 
		</aop:aspect>
	</aop:config>
```
### :balloon: 실습 _ Step05_AOPSchema  NoticeAspect.java

```java
package step01.common.schema;

//공통 기능의 클래스
public class NoticeAspect {

	// 반환시 공통로직
	/* 설계 - 메소드가 반환하는 로직을 포함한 경우에 공통적으로 처리하고자 하는 로직
	 *  parameter로 핵심 로직이 반하는 값을 받아서 처리
	 *  핵심로직이 반환하는 타입의 수는 무한대를 수용 가능하게 하려면 어떤 타입으로 parameter로 처리 
	 */
	public void noticReturnValue(Object value) {
		System.out.println("공통이 넘긴 반환값 - " + value);
	}
	
	// 예외 발생시 공통로직
	/* 핵심로직 실행시 발생되는 예외를 일괄적으로 받아서 처리
	 * 설계 - parameter로 발생되는 예외를 받아서 처리
	 */
	public void noticException(Exception e) {
		System.out.println("통일된 방식으로 예외 처리 로직 : " + e.getMessage()); //예외가 갖는 메세지를 반환
	}
}
```
### :balloon: 실습 _ Step05_AOPSchema  Car.java
```java
package step01.biz.schema;

//구매, 판매 로직의 핵심 기능이라 가정
public class Car {

	public void buy() {
		System.out.println("자동차 구매"); 					//biz logic
	}
	
	public void buyMoney(int money) {
		System.out.println("자동차 구입한 금액 " + money); 		//biz logic
	}


	public void buy2() {
		System.out.println("자동차 구매");
	}
	
	public void buyMoney2(int money) {
		System.out.println("자동차 구입한 금액 " + money);
	}
	
	public String buyReturn() {
		return "자동차 구매 성공";
	}
	
	//판매 금액이 1000만원 이하인 경우에 예외 발생
	public void saleMoney(int money) throws Exception {
		if(money <= 1000) {
			throw new Exception("너무 싸!!!"); //getMessage()로 내용 확인
		}
	}
}

```
### :balloon: 실습 _ Step05_AOPSchema  Test.java
```java
package running;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import step01.biz.schema.Car;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("playdata.xml");
		
		Car car = context.getBean("car", Car.class);
		car.buy();
		
		String data = car.buyReturn();
		System.out.println("리턴 받은 데이터 : " + data);
		
		try {
			car.saleMoney(500);
		} catch (Exception e) {	
		}	
	}
}
```
### :balloon: 실습 _ Step05_AOPSchema  Test.java 실행 결과
![step05](https://user-images.githubusercontent.com/37354978/106275706-9aa4f980-6279-11eb-9176-980a02dca280.PNG)

## :zap: **Spring AOP Schema 기반 실습-around**
\<aop:around\>

### :balloon: 실습 _ Step05_AOPSchema  playdataaround.xml
```xml
<aop:aspectj-autoproxy />
	
	<bean id="car" class="step01.biz.schema.Car" />
	<bean id="common" class="step02.common.schema.around.AroundAspect" />
	
	<aop:config>
		<aop:aspect ref="common">
			<aop:around method="aroundAspect" pointcut="within(step01.biz.schema.*)"/>
		</aop:aspect>
	</aop:config>
```
### :balloon: 실습 _ Step05_AOPSchema  AroundAspect.java
```java
package step02.common.schema.around;

import org.aspectj.lang.ProceedingJoinPoint;

public class AroundAspect {
	//메소드가 before(전처리) / after&after returning(후처리) / after throwing -> 이 모든 것을 한번에 구현 가능
	//return type=Object : 혹 return 데이터가 있는 핵심 메소드라면 공통 해당 로직 실행 후에도 반환값은 핵심 로직을 
	//요청했던 곳으로 넘겨줘야 하기 때문에 Object 타입으로 리턴 설정
	
	public Object aroundAspect(ProceedingJoinPoint point) {
			
		//전처리 - before
		System.out.println("[공통 1] 구매를 시작 하실 건가요?");
		
		Object bizReturnValue = null;
		
		//핵심로직 실행 & 예외처리
		try {
			bizReturnValue = point.proceed(); //메소드명이 무엇이든 무조건 핵심 메소드 호출하는 절대 코드
		} catch (Throwable e) {
			System.out.println("예외 발생시 실행");
		}  
		
		//후처리 - after/ after returning
		System.out.println("[공통 2] 구매를 완료 하셨습니다");
		
		//핵심로직 실행 결과 리턴
		return bizReturnValue;
	}
}
```
### :balloon: 실습 _ Step05_AOPSchema  Test.java
```java
package running;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import step01.biz.schema.Car;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("playdataaround.xml");
		
		Car car = context.getBean("car", Car.class);
		car.buy();
		
		String data = car.buyReturn();
		System.out.println("리턴 받은 데이터 : " + data);
		
		try {
			car.saleMoney(500);
		} catch (Exception e) {	
		}	
	}
}
```
### :balloon: 실습 _ Step05_AOPSchema  Test.java 실행 결과
![step05_shema2](https://user-images.githubusercontent.com/37354978/106276625-35520800-627b-11eb-92bf-770168c7d16d.PNG)

## :zap: **Spring AOP Annotation 실습 **
@Before, 	@After, @AfterReturning, @AfterThrowing
### :balloon: 실습 _ Step06_AOPAnnotation  playdata.xml

```xml
	<aop:aspectj-autoproxy />
	<context:annotation-config />
	
	<context:component-scan base-package="step01.biz.annotation" />
	<context:component-scan base-package="step01.common.annotation" />
```
### :balloon: 실습 _ Step06_AOPAnnotation  NoticeAspect.java
```java
package step01.common.annotation;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class NoticeAspect {
	
	@Before("execution(* step01.biz.annotation.Car.buy*(..))")
	public void noticBuyStart() {
		System.out.println("[공통 1] 구매를 시작 하실 건가요?");
	}
	
	@After("execution(* step01.biz.annotation.Car.buy*(..))")
	public void noticBuyEnd() {
		System.out.println("[공통 2] 구매를 완료 하셨습니다");
	}
	
	@AfterReturning(pointcut="execution(* step01.biz.annotation.Car.buyReturn(..))", returning="value")
	public void noticReturnValue(Object value) {
		System.out.println("공통이 넘긴 반환값 - " + value);
	}
	
	@AfterThrowing(pointcut="execution(* step01.biz.annotation.Car.sale*(int))", throwing="e")
	public void noticException(Exception e) {
		System.out.println("통일된 방식으로 예외 처리 로직 : " + e.getMessage());
	}
}
```
### :balloon: 실습 _ Step06_AOPAnnotation  Test.java
```java
package running;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import step01.biz.annotation.Car;

public class Test {

	public static void main(String[] args) {
		//? 컨테이너 생성
		ApplicationContext context = new ClassPathXmlApplicationContext("playdata.xml");
		
		//? Car 빈객체 생성
		Car car = context.getBean("biz", Car.class); //? 이 코드 수정 불가
		
		System.out.println("== before ==");
		System.out.println("== after ==");
		car.buy();
		
		System.out.println("== exception ==");
		
		try {
			car.saleMoney(500);
		} catch(Exception e) {
			
		}
		
		System.out.println("=============");
		String v = car.buyReturn();
		System.out.println("반환된 데이터값 : " + v);

	}

}

```
### :balloon: 실습 _ Step06_AOPAnnotation  Test.java 실행 결과

![step06_AOPAnno](https://user-images.githubusercontent.com/37354978/106278543-3a648680-627e-11eb-845c-2a03ba56dd58.PNG)
 
## :zap: **Spring AOP Annotation 정리 **
**@Component**  , **@Aspect**  public class Xxxx() { ... } 클래스 위에 선언

**@Component("biz")**  = 

```xml
<!-- 핵심(biz, core)과 공통(aspect)을 스프링 빈으로 등록 --> 
<bean id="biz" class="step02.biz.aop.Car" />  

<!-- 핵심로직 실행 전, 후 공통 로직 적용 핵심의 어떤 메소드들에 공통 로직 적용할지도 결정을 해야함 어떤 핵심 로직의 메소드에 어떤 공통로직의 메소드를 정해진 시점에 적용 spring은 aspectj라는 framework 활용 - 기능을 메소드에만 적용 + 표현법은 그대로 사용 -->  
```
**@Component("common")**  = 
```xml
<bean id="common" class="step02.common.aop.NoticeAspect" />  
```
**@Aspect** =
```xml
<aop:config>  
<aop:pointcut id="bizLogic" expression="execution(* step02.biz.aop.Car.buy*(..))"/> 
<aop:aspect ref="common">  
	<aop:before method="noticBuyStart" pointcut-ref="bizLogic"/>  
	<aop:after method="noticBuyEnd" pointcut-ref="bizLogic"/> 
</aop:aspect>
```

@Aspect : public class Xxxx() {... }  바로 위에 선언
@Around("within(model.domain.annotation.*)")  :   public Object aroundAspect(ProceedingJoinPoint point) { ..... }  바로 위에 선언

**@Aspect**   
**@Around("within(model.domain.annotation.\*)")**     = 
```xml
	<aop:config>
		<aop:aspect ref="common">
			<aop:around method="aroundAspect" pointcut="within(model.domain.annotation.*)"/>
		</aop:aspect>
	</aop:config>
```

**Reference**
- https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop
- [10분 테크톡] 제이의 Spring AOP https://www.youtube.com/watch?v=Hm0w_9ngDpM
- https://jojoldu.tistory.com/71
- https://engkimbs.tistory.com/746
- 예제로 배우는 스프링 입문, 9. AOP https://www.youtube.com/watch?v=GeLBZ-Fe38s
