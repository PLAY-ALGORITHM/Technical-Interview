# :wink: Spring의 DI란?
----------------------
## :question: DI란?

- DI는 말 그대로 의존성을 주입시켜준다- 입니다. 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입을 시켜주는 방식이다. 
----------------------
## Setter() 방식의 예제

### 예제[1]

![SPRING DI(1)](https://user-images.githubusercontent.com/73863771/106350980-59b1f100-631c-11eb-9b6e-7adfe2522598.png)

- 1번 방법
	1. A객체가 B와 C객체를 New 생성자를 통해서 직접 생성하는 방법
	2. 외부에서 생성 된 객체를 setter()나 생성자를 통해 사용하는 방법

- Main.java
 ```java
package model;

public class MainClass {
	public static void main(String[] args){

		Cats cats = new Cats(); // 객체를 직접 생성

		cats.setFirstCatName("보리");
		cats.setSecondName("나비");

		cats.setFirstCatAge(2);
		cats.setSecondCatAge(4);

		cats.catsName();
		cats.catsAge();
	}
}
 ```

 -Cats.java
 ```java
 package model;

 public class Cats{

	 private String firstCatName;
	 private int firstCatAge;
	 private String secondCatName;
	 private int secondCatAge;

	 public void catsName(){
		 System.out.println("catsName()");
		 System.out.println("첫번째 고양이 이름은 " + firstCatName + "입니다.");
		 System.out.println("두번째 고양이 이름은 " + secondCatAge + "살 입니다.");
	 }

	 public void setFirstCatName(String firstCatName){
		 this.firstCatName = firstCatName;
	 }

	 public void setFirstCatAge(int firstCatAge){
		 this.firstCatAge = firstCatAge;
	 }
	 
	 public void setSecondCatName(String secondCatName){
		 this.secondCatName = secondCatName;
	 }

	 public void setSecondCatAge(int secondCatAge){
		 this.secondCatAge = secondCatAge;
	 }
 }
 ```
 ### 예제[2]

![SPRING DI(2)](https://user-images.githubusercontent.com/73863771/106351428-713ea900-631f-11eb-9995-f2e568c47559.png)

- 2번 방법
	1. A객체에서 직접 생성하지 않고, 외부에서 생성된 객체를 setter( )혹은 생성자를 이용해서 사용합니다. 이런걸 "주입"한다고 하며 스프링에서 사용하는 방식(DI)
	2. A라는 객체에서 B, C객체를 사용(의존)할 때 A객체에서 직접 생성을 하는 것이 아닌 외부(IOC컨테이너)에서 생성된 B, C객체를 조립(주입)시켜 setter 혹은 생성자를 통해 사용

 -Cats.java
```java
package model;

public class Cats{

	public void catsName(String firstCatName, String secondCatName){
		System.out.println("catsName()");
		System.out.println("첫번째 고양이 이름은 " + firstCatName + "입니다.");
		System.out.println("두번째 고양이 이름은 " + secondCatName + "입니다.");
	}

	public void catsAge(int firstCatAge, int secondCatAge){
		System.out.println("catsAge()");
		System.out.println("첫번째 고양이 나이는 " + firstCatAge + "입니다.");
		System.out.println("두번째 고양이 나이는 " + secondCatAge + "입니다."); 
	}
}
```

-MyCats.java
```java
package model;

public class MyCats {

	Cats cats; 
	private String firstCatName;
	private int firstCatAge;
	private String secondCatName;
	private int secondCatAge;

	public void catsNameInfo() {
		cats.catsName(firstCatName, secondCatName);
	}

	public void catsAgeInfo() {
		cats.catsAge(firstCatAge, secondCatAge);
	}

	public void setCats(Cats cats) {
		this.cats = cats;
	}

	public void setFirstCatName(String firstCatName) {
		this.firstCatName = firstCatName;
	}

	public void setFirstCatAge(int firstCatAge) {
		this.firstCatAge = firstCatAge;
	}

	public void setSecondCatName(String secondCatName) {
		this.secondCatName = secondCatName;
	}

	public void setSecondCatAge(int secondCatAge) {
		this.secondCatAge = secondCatAge;
	}

}
```
### 예제[1] 과 예제 [2] 의 차이점

|  | 예제[1] | 예제[2] |
|--|------|------|
| 1 | Cats클래스에 setter와 실제 작동 하는 메서드를 작성 | Cats와 MyCats를 나누어 줌 |

- MainClass.java
```java
package diEx;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class MainClass {

	public static void main(String[] args) {
		
		 //bean을 설정한 xml파일이 있는 위치 지정
		String configLocation = "classpath:applicationContext.xml";
		
		//지정한 위치를 참고하여 설정파일을 얻어옴
		AbstractApplicationContext ctx = 
				new GenericXmlApplicationContext(configLocation);
		
		//설정파일에서 bean을 가져옴
		//getBean()를 이용해서 MyCats타입에서 myCats를 얻어와서 객체를 생성 
		// = 방법1 예제처럼 직접 생성이 아닌 외부에서 얻어옴(주입을 시켜줌)
		MyCats myCat = ctx.getBean("myCats",MyCats.class);
		
		//호출
		myCat.catsNameInfo();
		myCat.catsAgeInfo();
	}

}
```
- 예제[1] 처럼 직접 객체를 생성하지 않고 예제[2]는 GenericXmlApplicationContext를 사용하여 객체(bean)을 외부에서 얻어와서 호출(사용)

- applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
 
<!-- "diEx.Cats"클래스를 cats라는 id를 지정해서 객체(bean)을 생성 -->
<!-- "이 객체(bean)의 이름(id)은 cats입니다. 
        cats는 diEx패키지에 있는 Cats 클래스를 말합니다." 라고 선언 --> 
<bean id="cats" class="diEx.Cats" />
 
 
<!-- "diEx.MyCats"클래스를 myCats라는 id를 지정해서 객체(bean)을 생성 -->
<bean id="myCats" class="diEx.MyCats">
    <!-- diEx.MyCats라는 클래스에 있는 필드들의 값을 설정해줌 -->
    <property name="cats"><!-- 첫번째 property(필드) -->
        <ref bean="cats"/><!-- 이 property는 위에서 생성한 bean(객체)인 cats를 참조한다. -->
    </property>
    <property name="firstCatName" value="순덕" /><!-- MyCats의 필드의 이름과 값을 설정 -->
    <property name="secondCatName" value="나비" />
    <property name="firstCatAge" value="1" />
    <property name="secondCatAge" value="2" />
</bean>
</beans>

```

- applicationContext.xml에서 bean(객체)을 생성해 줌 

### 결과값

catsName()
첫번째 고양이 이름은 보리입니다.
두번째 고양이 이름은 나비입니다.
catsAge()
첫번째 고양이 나이는 2살입니다.
두번째 고양이 나이는 4살입니다.

### 예제[2] 의 Cats.java와 MyCats.java 차이점
|  | 예제[1] | 예제[2] |
|--|------|------|
| 1 | 실제 기능을 하는 메서드를 작성 | 필요한 필드들을 선언 후 setter를 만들어 줌 <br> catsNameInfo()와 catsAgeInfo() 메서드를 생성하여 직접 처리하지 않고, Cats를 클래스를 사용해서 처리|

### 총 정리

- DI(의존성 주입)을 적용한 두번째 예제는 직접 객체를 생성하지 않고, 외부(applicationContext.xml)에서 객체를 생성 후 사용할 객체에 주입을 시켜주어 사용합니다. 

 만약, 첫번째 방법처럼 직접 객체를 생성했을 경우, 고양이의 정보가 아닌 강아지의 정보를 보고 싶을때 메인 클래스의 소스를 싹 고쳐야 하지만 두번째 방법을 사용한다면 설정 파일만 바꿔주면 된다.

-----------------------

## 생성자를 사용한 예제

### 예제
