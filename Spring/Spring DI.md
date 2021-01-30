# :wink: Spring의 DI란?
----------------------
## :question: DI란?

- DI는 말 그대로 의존성을 주입시켜준다- 입니다. 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입을 시켜주는 방식이다. 

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
### :question: 예제[1] 과 예제 [2] 의 차이점

|  | 예제[1] | 예제[2] |
|--|------|------|
| 1 | Cats클래스에 setter와 실제 작동 하는 메서드를 작성 | Cats와 MyCats를 나누어 줌 |

### :question: 예제[2] 의 Cats.java와 MyCats.java 차이점
|  | 예제[1] | 예제[2] |
|--|------|------|
| 1 | 실제 기능을 하는 메서드를 작성 | 필요한 필드들을 선언 후 setter를 만들어 줌 <br> catsNameInfo()와 catsAgeInfo() 메서드를 생성하여 직접 처리하지 않고, Cats를 클래스를 사용해서 처리|
