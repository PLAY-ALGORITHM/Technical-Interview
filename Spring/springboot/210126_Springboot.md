:octocat: SpringBoot
=================================================

# Spring이란..?

Spring이라 불리는 이유 = 개발자들의 겨울이 끝났다!

![image](https://user-images.githubusercontent.com/61037197/105848608-916e1f80-6022-11eb-95f0-f75db2b0903a.png)![image !

- 자바 플랫폼을 기반으로 만들어진 오픈소스 프레임워크로 간단하게 스프링이라 불림.
- JAVA를 이용한 기술인 JSP, MyBatis, JPA등의 기술들을 더 편하게 사용하기 위해 만들어짐
- 중복 코드의 사용률을 줄여주고, 비즈니스 로직을 더 간단하게 해 줌
- 오픈 소스를 좀 더 효율적으로 가져다 쓰기 좋은 구조 
	- 다른 사람의 코드를 참조하여 쓰기 편리
- 특징 : 의존성 주입(DI, Dependency Injection)과 제어의 역전(IOC, Inversion Of Control)등



# Spring boot 란?
![image (1)](https://user-images.githubusercontent.com/61037197/105849233-7059fe80-6023-11eb-8d7b-5ad28b15ed4e.png)
![image (2)](https://user-images.githubusercontent.com/61037197/105848884-f4f84d00-6022-11eb-8d91-f243c07a7467.png)


- 자바 프로그램을 만들기 위한 노하우들이 다 뭉쳐있음
- 스프링 부트는 스프링 프레임워크라는 큰 틀에 속하는 도구 

<img width="590" alt="img9" src="https://user-images.githubusercontent.com/61037197/105848958-0fcac180-6023-11eb-8feb-1582dc2b30e9.png">

- spring과의 차이
    1. Embed Tomcat을 사용하기 때문에, (Spring Boot 내부에 Tomcat이 포함되어있다.) 따로 Tomcat을 설치하거나 매번 버전을 관리해 주어야 하는 수고로움을 덜어준다.
    2. starter을 통한 dependency 자동화
        - 과거 Spring framework에서는 각각의 dependency들의 호환되는 버전을 일일이 맞추어 주어야 했기 때문에 하나의 버전을 올리고자 하면 다른 dependeny에 까지 영향을 미쳐 version관리에 어려움이 겪음
        - 반면, Spring boot에서는 starter가 대부분의 dependency를 관리해주기 때문에 이러한 걱정을 많이 덜게 됨
    3. 번거로운 XML 설정을 하지 않아도 된다 
    4. jar file을 이용하여 자바 옵션만으로 손쉽게 배포가 가능
        - WAR 파일로 패키징 해야하는 웹 프로젝트와 달리, 내장 Tomcat을 지원하기 때문에 JAR파일로 패키징하여 웹 애플리케이션 실행이 가능
    5. Spring Actuaor제공 
        -  애플리케이션의 모니터링과 관리를 제공한다.
    


# 특징 자세히 

1. dependency
<img width="838" alt="img5" src="https://user-images.githubusercontent.com/61037197/105849053-34bf3480-6023-11eb-8be0-542dab503c6d.png">
<img width="841" alt="img6" src="https://user-images.githubusercontent.com/61037197/105849087-41438d00-6023-11eb-978e-190d762a1742.png">




2. Spring Boot Starter
- starter : 특정 목적을 달성하기 위한 의존성 그룹
- starter는 npm처럼 간편하게 dependency를 제공해주는데, 만약 우리가 JPA가 필요하다면 prom.xml(메이븐)이나 build.gradle에 'spring-boot-starter-data-jpa'만 추가해주면 spring boot가 그에 필요한 라이브러리들을 알아서 받아온다. 
- 즉, 스프링 부트에서는 스타터가 빌드에 필요한 의존성을 자동으로 관리
- 형태: spring-boot-starter*
    
> 'spring-boot-starter-data-jpa'
  'spring-boot-starter-security'
  'spring-boot-starterr-test'
  'spring-boot-starter-web'
  'spring-boot-starter-thymeleaf'

# 시스템 요구사항
Spring Boot 2.0.0RC2 기준
Java 8 or 9
Spring Frameworkd 5.0.4 RELEASE
Maven 3.2+
Gradle 4

# Servlet Containers (Servlet 3.0+ 호환 가능한 컨테이너)
Tomcat 8.5
Jetty 9.4
Undertow 1.4

# 스프링 부트 시작 전
Java 8버전 설치

Maven 설치

pom.xml 

실행가능한 Java main문 만들기

서버실행




# 정리 : 왜 스프링 부트를 사용하나?

 1. 첫 번째, 서버 구축이 간편!
- 최소한의 설정으로 스프링의 여러가지 라이브러리나 플랫폼을 사용할 수 있는 장점과 내장형 톰캣, 제티 등을 탑재하여 단독 실행가능 기능을 제공
- 과거 스프링 mvc 프레임 워크를 사용할때는 톰켓 라이브러리를 다운받아서, 경로를 확인하고, import 해준 뒤 설정을 해야하는 번거로움이 있었는데, 스프링부트에서는 웹서버 톰켓이 내장되어 있어서 따로 설정을 하지 않아도 서버를 실행할 수 있다.

2. 수많은 스프링 프레임워크가 한곳에 모여있다.
- 스프링부트 프레임워크라고 해서 하나의 프레임워크가 있는것 처럼 들리는데 그렇지 않다.
- 스프링 부트 프레임워크는 spring data, spring web service , spring mobile, spring mvc 등 다양한 스프링 프레임워크를 마우스 클릭 몇번으로 가져와서 사용할 수 있다.

3. 가볍고 쉽다
- spring mvc 프레임워크를 사용할 때는 여러 xml 파일을 작성해야하는 어려움이 있었고 또 굉장히 무거웠다.
 - 설정 파일을 작성하는 데 투자할 시간을 개발에 투자할 수 있게 설정 파일 작성을 할 필요가 없다. 



# 스프링 부트 단점
- 내장 톰켓 관리가 어려움. 규모가 큰 경우에는 외장 톰캣으로 연동해야 할 듯함. 
- 같은 서버 포트번호로 다르게 배포 시 (서로 다른 프로젝트) boot 버전을 맞춰야 한다. 



# 출처

우아한Tech
https://dayzen1258.tistory.com/entry/Spring-boot%EB%9E%80
https://jerryjerryjerry.tistory.com/62
https://abc1211.tistory.com/639 [길위의 개발자]
https://yoo-hyeok.tistory.com/117 [유혁의 엉터리 개발]
