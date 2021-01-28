# Spring MVC

Model, View, Controller를 분리한 디자인 패턴

**Model**
- 어플리케이션의 data를 나타냄.
- 일반적으로 POJO로 구성됨
- Java Beans

 **View**
- 디스플레이 데이터 또는 프레젠테이션
- html ,jsp로 구성
- HTML output 생성

**Controller**
- FrontController : Dispatcher-Servlet
- View와 Model사이에서 사용자의 입력에 따른 적절한 결과를 처리하여 Model에 담아 View에 전달함.
- Dispatcher Servlet이 Controller 역할 담당함.
- @Controller 어노테이션 활용하여 Controller역할임을 명시
- @RequestMapping() 어노테이션으로 요청에 맞는 처리 가능

#
## pom.xml
모듈들을 가져오는 설정을 담당하는 파일
``` 
	<!-- Spring MVC -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>
		

```
## web.xml (WEB-INF하단에 위치)
웹서비스의 구동에 필요한 설정을 담당
``` 
<display-name>step08_webBasic</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
	</welcome-file-list>

	<!-- dispatcherservlet 설정과정 -->
	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

## dispatcher-servlet.xml (WEB-INF/lib하단에 위치)
스프링과 관련된 설정을 담당
bean을 활용한 DI 설정가능
redirect로 화면 이동하는 경우 dispatcher servlet이 관여하지 않음.
forward와 같이 request객체를 공유하여 화면을 이동하는 경우에만 dispatcher servlet이 관리.

```
<!-- controller의 패키지를 알려주고 디스패처가 controller를 찾는다. @Controller를 만나면 그 클래스가 Controller라는 것으로 인식함. -->
<mvc:annotation-driven/>
	<mvc:default-servlet-handler/>	
	<context:component-scan base-package="controller" />
	
	<!-- jsp위치에 대한 관리용 API 등록
		Spring Framework - id 값에 설정된 이름으로 이 객체를 활용
		코드상에서 개발자가 getBean() 하는 일은 없음
	 -->
	
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 디스패처가 뷰를 찾는 방법 -->
		<property name="prefix" value="/WEB-INF/" />
		<property name="suffix" value=".jsp" />
	</bean>
```

## Sub Controller

1. 애노테이션 베이스로 개발 절대 권장
			@Controller
		
2. get/post방식 요청 처리 또한 doGet or doPost메소드가 아닌 애노테이션 설정된 일반 메소드가 처리
			@RequestMapping("url명")
			@RequestMapping(value="url명", method=RequestMethod.GET)
			@RequestMapping(value="url명", method=RequestMethod.POST)
			
		
3. forward/redirect 웹페이지 이동 방식도 문자열로 지정 또는 단순 return만으로 처리
			1. forward 방식 사용한 API
				ModelAndView
					view지정 : setViewName("prefix와 surfix 제외한 파일명만")
					return이 forward 방식이동
			2. redirect 방식 
				반환 타입 - String
				return "redirect:file명.jsp"
								
4. request.setAttribute("key", value) 동일한 형식의 API 제공
	ModelAndView의 addObject()		
			
```java
@Controller
public class TestController {
	
	@RequestMapping(value="one", method=RequestMethod.GET)
	public ModelAndView m1() {
		System.out.println("m1() -------------");
		
		ModelAndView mv = new ModelAndView();
		
		//HttpServletRequest 객체에 setAttribute("key", "data")
		mv.addObject("key", "data"); //데이터 저장(model 저장)
		mv.setViewName("finalView"); //jsp 즉 실행 view 지정
		
		return mv;  //forward 방식으로 이동
	}
	
	@RequestMapping(value="two")
	public String m2() {
		System.out.println("m2() -------------");
		return "redirect:view.jsp"; 
	}
}

```

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http://cfile25.uf.tistory.com/image/9927F33B5AA920381357E1)
client가 요청을 하면, DispatcherServlet이 요청받음.
DispatcherServlet이 Controller에게 요청한뒤 ModelAndView를 이용하여 응답을 받음. ViewResolver와 View를 통해 사용자에게 페이지를 띄워줌.
#

![enter image description here](https://docs.spring.io/spring-framework/docs/4.3.30.RELEASE/spring-framework-reference/htmlsingle/images/mvc.png)

### Spring MVC 처리 순서

1. 클라이언트(Client)가 서버에 어떤 요청(Request)을 한다면 스프링에서 제공하는  **DispatcherServlet**  이라는 클래스(일종의 front controller)가 요청을 가로챈다.

(web.xml에 살펴보면 모든 url ( / )에 서블릿 매핑을하여 모든 요청을 DispatcherServlet이 가로체게 해둠(변경 가능))

2. 요청을 가로챈 DispatcherServlet은  **HandlerMapping**(URL 분석등..)에게  어떤 컨트롤러에게 요청을 위임하면 좋을지 물어본다.

(HandlerMapping은 servlet-context.xml에서 @Controller로 등록한 것들을 스캔해서 찾아놨기 때문에 어느 컨트롤러에게 요청을 위임해야할지 알고 있다.)

3. 요청에 매핑된 컨트롤러가 있다면  **@RequestMapping을 통하여 요청을 처리할 메서드**에 도달한다.

4. 컨트롤러에서는  해당 요청을 처리할 Service를 주입(DI)받아 비즈니스로직을 Service에게 위임한다.

5. Service에서는 요청에 필요한 작업 대부분(코딩)을 담당하며  데이터베이스에 접근이 필요하면 DAO를 주입받아 DB처리는 DAO에게 위임한다.

6. DAO는  mybatis(또는 hibernate등) 설정을 이용해서 SQL 쿼리를 날려 DB에 저장되어있는 정보를 받아 서비스에게 다시 돌려준다.

(이 때, 보통 Request와 함께 날아온 DTO 객체(@RequestParam, @RequestBody, ...)로 부터 DB 조회에 필요한 데이터를 받아와 쿼리를 만들어 보내고, 결과로 받은 Entity 객체를 가지고 Response에 필요한 DTO객체로 변환한다.)

7. 모든 비즈니스 로직을 끝낸 서비스가 결과물을 컨트롤러에게 넘긴다.

8. 결과물을 받은 컨트롤러는 필요에 따라 Model객체에 결과물 넣거나, 어떤 view(jsp)파일을 보여줄 것인지등의 정보를 담아 DispatcherServlet에게 보낸다.

9. DispatcherServlet은 ViewResolver에게 받은 뷰의 대한 정보를 넘긴다.

10. ViewResolver는 해당 JSP를 찾아서(응답할 View를 찾음) DispatcherServlet에게 알려준다.

(servlet-context.xml에서 suffix, prefix를 통해 /WEB-INF/views/index.jsp 이렇게 만들어주는 것도 ViewResolver)

11. DispatcherServlet은 응답할 View에게 Render를 지시하고 View는 응답 로직을 처리한다.

12. 결과적으로 DispatcherServlet이 클라이언트에게 렌더링된 View를 응답한다.

출처: [https://jeong-pro.tistory.com/96](https://jeong-pro.tistory.com/96) [기본기를 쌓는 정아마추어 코딩블로그]


