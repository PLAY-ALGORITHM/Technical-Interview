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

- Cats.java
```java
package diEx2;


public class Cats {

private MyCats myCats;
	
	//생성자를 통해 myCats를 인수로 받아와서 myCats를 초기화시켜줌
	public Cats(MyCats myCats){
		this.myCats = myCats;
	}
	
	public void getMyCatsInfo(){
		System.out.println("==============");
		System.out.println("야옹이 이름 : "+myCats.getName());
		System.out.println("야옹이 나이 : "+myCats.getAge());
		System.out.println("야옹이 취미 : "+myCats.getHobbys());
		System.out.println("==============");
	}
	
}

```

- MyCats.java
```java
package diEx2;

import java.util.ArrayList;

public class MyCats {

	private String name;
	private int age;
	private ArrayList<String> hobbys;

	//생성자를 통해 name, age, hobbys를 받아와 각각의 필드의 값들을 초기화 시켜줌
	public MyCats(String name, int age, ArrayList<String> hobbys) {
		this.name = name;
		this.age = age;
		this.hobbys = hobbys;
	}

	//정보 출력을 위한 getter
	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public ArrayList<String> getHobbys() {
		return hobbys;
	}

}

```

- 생성자를 이용해서 name, age, hobbys를 인자로 받아와 필드에 있는 name, age, hobbys를 초기화

- myCats를 인수로 받아와서 필드의 myCats값을 초기화

- MainClass.java
```java
package diEx2;

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
		//getBean()를 이용해서 Cats타입에서 catsInfo를 얻어와서 객체를 생성 
		// = 방법1 예제처럼 직접 생성이 아닌 외부에서 얻어옴(주입을 시켜줌)
		Cats catsInfo = ctx.getBean("catsInfo",Cats.class);
		
		// 첫번째 고양이의 정보를 호출
		catsInfo.getMyCatsInfo();
		
		ctx.close();
	}

}

```

- catsInfo라는 객체를 얻어온다.

- applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

-<beans xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans">

<!-- id가 cat1이고, diEx2패키지에 있는 MyCats클래스를 뜻하는 bean(객체) 생성 -->

<bean class="diEx2.MyCats" id="cat1">


-<constructor-arg>

<!-- 생성자를 이용 -->


<value>나비</value>

</constructor-arg>


<constructor-arg>

<value>2</value>

</constructor-arg>


-<constructor-arg>


<list>

<!-- 배열일 경우에 사용 -->


<value>잠자기</value>

<value>꾹꾹이</value>

</list>

</constructor-arg>

</bean>


<!-- id가 catsInfo이고, diEx2패키지에 있는 Cats클래스를 뜻하는 bean(객체) 생성 -->

<bean class="diEx2.Cats" id="catsInfo">

<!-- catsInfo라는 bean은 위에서 만든 cat1이라는 bean(객체)를 참조함 -->

<constructor-arg>

<ref bean="cat1"/>

</constructor-arg>

</bean>

</beans>
```

- 생성한 첫번째 객체를 보면 "이 객체의 이름(id)은 cat1이고, diEx2패키지에 있는 MyCats 클래스" 라고 보면 된다.
- MyCats 클래스에는 name, age, hobbys를 인자로 받아와서 필드값을 초기화 시켜주는 생성자가 있다. 
- 생성자에 값을 넘겨주기 위해서는 이전에 했던 property가 아닌 constructor-arg를 사용해서 값을 넘겨준다. (property는 setter()가 있어야 사용이 가능합니다.) 
- 생성자에서 받아오는 인자 순서대로 이름, 나이, 취미를 넣어주면 된다.

- 두번째 객체를 보면 "이 객체의 이름(id)은 catsInfo이고, diEx2패키지에 있는 Cats 클래스" 라고 보면 된다.
- Cats클래스에도 생성자가 있었다.
- myCats를 인자로 받아와서 필드에 있는 myCats의 값을 초기화 시켜주기 때문에 마찬가지로 contructor-arg를 이용하여 값을 넘겨준다. 
- <ref bean = "cat1"/> 위에서 만든 cat1이라는 bean(객체)를 참조하겠다고 작성했기 때문에 첫번째 객체인 cat1의 값들이 넘어가서 생성자를 통해 초기화 된다.


- 메인클래스를 다시 보면 getBean으로 MyCats를 참조하고 있는 catsInfo라는 객체를 가져와서 catsInfo에 있는 getMyCatsInfo() 메서드를 사용하여 고양이의 정보를 출력해준다.

### 결과값

==============
야옹이 이름 : 나비
야옹이 나이 : 2
야옹이 취미 : [잠자기, 꾹꾹이]
==============


- applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

-<beans xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans">

<!-- 두번째 고양이 -->

<bean class="diEx2.MyCats" id="cat2">

<constructor-arg value="호랑이"/>

<constructor-arg value="1"/>


<constructor-arg>


<list>

<value>우다다</value>

<value>박치기</value>

</list>

</constructor-arg>

</bean>

<!-- id가 catsInfo이고, diEx2패키지에 있는 Cats클래스를 뜻하는 bean(객체) 생성 -->



<bean class="diEx2.Cats" id="catsInfo">

<!-- catsInfo라는 bean은 위에서 만든 cat1이라는 bean(객체)를 참조함 -->



<constructor-arg>

<ref bean="cat1"/>

</constructor-arg>

</bean>

</beans>
```

- 두번째 고양이를 등록하는 과정이다. 
- applicationContext에서 cat2라는 bean을 생성하고 값을 설정해 주었다. 
- property와 같이 <contructor-arg>안에 바로 value 값을 넣어줘도 된다. 
- 아래에 아까 생성한 catsInfo라는 객체는 여전히 cat1을 참조하고 있다.(변경사항 없음)

- Cats클래스에서 두번째 고양이의 정보도 출력할 수 있게 set메서드를 추가해준다.

```java
package diEx2;


public class Cats {

private MyCats myCats;
	
	//생성자를 통해 myCats를 인수로 받아와서 myCats를 초기화시켜줌
	public Cats(MyCats myCats){
		this.myCats = myCats;
	}
	
	public void getMyCatsInfo(){
		System.out.println("==============");
		System.out.println("야옹이 이름 : "+myCats.getName());
		System.out.println("야옹이 나이 : "+myCats.getAge());
		System.out.println("야옹이 취미 : "+myCats.getHobbys());
		System.out.println("==============");
	}
	
	//**받아온 myCats값을 넣어줌**
	public void setMyCatsInfo(MyCats myCats){
		this.myCats = myCats;
	}

}

```

- setMyCatsInfo() 메서드에서 인자값으로 myCats를 받아와서 필드의 myCats에 값을 넣어준다. 그 후 메인 클래스에서 넘겨주면 된다.

- MainClass.java
```java
package diEx2;

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
		//getBean()를 이용해서 Cats타입에서 catsInfo를 얻어와서 객체를 생성 
		// = 방법1 예제처럼 직접 생성이 아닌 외부에서 얻어옴(주입을 시켜줌)
		Cats catsInfo = ctx.getBean("catsInfo",Cats.class);
		
		// 첫번째 고양이의 정보를 호출
		catsInfo.getMyCatsInfo();
		
		//**두번째 고양이의 정보를 호출**
		MyCats cat2 = ctx.getBean("cat2",MyCats.class);
		catsInfo.setMyCatsInfo(cat2);
		catsInfo.getMyCatsInfo();
		
		
		ctx.close();
		
	}

}
```

- 첫번째 고양이의 경우 catsInfo라는 객체에서 cat1을 참조하기 때문에 따로 set을 해주지 않아도 되지만 두번째 고양이의 객체는 참조하고 있지 않으니 직접 set을 해준다. 
 - getBean을 통해서 cat2 객체를 얻어와서 위에서 생성한 setMyCatsInfo에 인자값으로 넣어줍니다. 그럼 Cats클래스에서 받아온 인자값을 필드값에 넣어주겠죠? 그 후에 getMyCatsInfo() 메서드를 호출해서 cat2의 값이 들어간 정보를 출력해준다. 
- 모두 사용한 객체는 close()를 통해 닫아준다.


### 결과값

==============
야옹이 이름 : 나비
야옹이 나이 : 2
야옹이 취미 : [잠자기, 꾹꾹이]
==============
==============
야옹이 이름 : 호랑이
야옹이 나이 : 1
야옹이 취미 : [우다다, 박치기]
==============

### 총 정리

- property를 사용할때는 꼭 setter()가 있어야하고, constructor-arg를 사용할때는 생성자가 있어야 한다.  
----------------------------



