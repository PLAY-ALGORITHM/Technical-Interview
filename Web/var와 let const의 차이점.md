
## 변수 선언 방식 var, let, const

**var, let, const의 차이점 정리** 

|  | 변수 재선언 | 변수 재할당(immutable) |
|--|--|--|
|var | O | O |
|let | X | O |
|const | X | X |

|  | Scope |
|--|--|
|var|Function-level-Scope|
|let | Block-level-Scope |
|const | Block-level-Scope |

 ### 사전지식
 **범위(Scope)** <br>
 선언된 변수가 미치는 범위를 뜻한다. Javascript는 함수 레벨 스코프(function-level-scope)를 따른다.
 
**Reference**
1. https://dev-taem.tistory.com/14

### var

 1. ES6 이전의 변수 선언 방식이다.  
 2. function 단위의 scope을 가진다.(function-level-scope)
 ```javascript
 //예시1
 var chicken = 'good';
 console.log(chicken); // good
 
 {
	chicken = 'nice'
 }
 console.log(chicken); //nice
 
 function testChicken() {
	var woongChicken = 'gag';
	console.log(woongChicken); // gag
 }
 
 //error: woongChicken is not defined
 console.log(woongChicken);
 ```
 
```javascript
//예시2
var hello = 'hello';
if(true) {
	var hello = 'hello in if';
}

console.log(hello); // hello in if	

// 'hello'라는 변수의 유효범위가 function 내라는 것을 알 수 있다.
// 즉, if절 내부에 hello 변수를 새로 선언했지만, var로 선언한 변수의 scope은 {}가 아닌 function이기 때문에 hello 변수가 block(블록,{}) 바깥에서도 변경된 것을 볼 수 있다.
```

 3. var는 아래와 같이 여러가지 문제점들이 있다.
 - var로 변수 선언 시 var 키워드 생략 가능
```javascript
chicken = 'nice';
console.log(chicken); // nice
```
 - var로 변수 선언 후 중복 선언 가능
 ```javascript
 var chicken = 'nice';
 console.log(chicken); // nice
 
 var chicken = 1;
 console.log(chicken); // 1
```
 - 변수가 선언도 되지 않았는데 참조 가능 (**변수 호이스팅**)
 ```javascript
 console.log(chicken); // undefined
 var chicken = 'nice'; // (할당)
 console.log(chicken); // nice
```
 - function 단위의 scope를 가지기 때문에 전역 변수를 남발할 수 있는 사태가 벌어지게 된다.
 
 4. 위와 같은 문제들을 해결하기 위해 ES6에서 const와 let이 나왔다.

 **Reference**
1. https://hianna.tistory.com/314

### const

 1. ES6에 const와 let이라는 변수 선언 방법이 추가되었다. 
 2. const는 'constance'의 약자이다. 즉, 한번 선언된 상수는 다시 재정의 할 수 없다.
 ```javascript
const hello='hello';
hello='change hello'; // error
// 상수로 선언한 hello의 값을 변경하려고 하면 에러가 발생한다. 
 ```
 3. block(블록) 단위의 scope를 가진다.(block-level-scope)
 ```javascript
 const hello='hello!';
{
  const hello='inner hello!'; 
  console.log(hello); // inner hello!
}
console.log(hello); // hello!
//괄호 안에 hello를 선언했지만, const의 scope은 괄호 블록 안이기 때문에 괄호 바깥에 hello 상수를 또 선언할 수 있다.
 ```
 
```javascript
  let chicken = 'nice'; //전역 변수
  
  {
	//ReferenceError: chicken is not defined
	console.log(chicken);
	let chicken = 'good'; // 지역 변수
	console.log(chicken); // good
  }
```


### let

 1. let으로 변수 선언하면 값을 재정의 할 수 있다.
 ```javascript
let hello='first hello';  // first hello
hello = 'changed hello';  // changed hello
  ```
 2. const와 마찬가지로 block(블록) 단위의 scope를 가진다.
  ```javascript
  let hello='first hello';  
{
  let hello = 'inner hello';  
  console.log(hello); // inner hello
}
console.log(hello); // first hello
   ```
 3. var처럼 같은 변수를 두 번 선언하는 것은 불가하다.
```javascript
let hello='first hello';  
let hello='second hello'; // error
```


#### 호이스팅

 ### 정의
**호이스팅(Hoisting):** hoisting이란 자바스크립트 함수는 실행되기 전에 함수 안에 필요한 변수값들을 모두 모아서 유효 범위의 최상단에 선언하는 것이다.
 - 자바스크립트는 실행시점에 변수와 함수에 대해 호이스팅을 수행한다.
 - 호이스팅은 실행 시점에 변수와 함수를 소스(코드)의 맨 위로 이동시키는 것을 의미한다.
 - 이러한 특성 때문에 var는 변수를 선언하기도 전에 변수를 참조할 수 있다.
```javascript
function test() {
	console.log(a)
	var a = 2
	console.log(a)
}
```
```javascript
//호이스팅 예시
function test() {
	var a // hoisting : 유효범위의 최상단에 선언됨
	console.log(a)
	var a = 2
	console.log(a)
}
// 실제로는 위의 코드처럼 코드가 실행된다.
```
 - 하지만 let과 const는 변수를 선언하기 이전에 참조할 수 없다.
```javascript
name = 'kyle';
let name;

console.log(name) 
// Uncaught ReferenceError: Cannot access 'name' before initialization
```
 - **let,const가 변수를 선언하기 전에 참조할 수 없다고 해서 호이스팅이 이루어지지 않는 것일까?**
 - 정답은 '그렇지 않다'이다. 이를 이해하기 위해서는 자바스크립트에서 변수를 할당할 때 var와 let,const가 어떤 차이가 있는지 이해할 필요가 있다.
 
  **Reference**
1. https://dev-taem.tistory.com/14
 
 
#### 자바스크립트의 변수 생성 3단계
 1. 선언 단계
  - 변수를 실행 컨텍스트의 변수 객체에 등록한다. 이 변수 객체는 Scope가 참조하는 대상이 된다.
  - Scope에 변수를 등록 후 변수를 위한 공간을 확보한다.
 2. 초기화 단계
  - 변수 객체에 등록된 변수를 위한 공간을 메모리에 확보한다. 이 단계에서 변수는 undefined로 초기화된다.
 3. 할당 단계
  - undefined로 초기화된 변수에 실제 값을 할당한다.
  
#### 자바스크립트에서 변수를 할당할 때 var와 let,const의 차이점
  
 - var로 선언된 변수는 **선언 단계와 초기화 단계가 한번에** 이루어진다.
 ![var1](https://user-images.githubusercontent.com/37354978/105271512-f49f2280-5bda-11eb-8e63-491a5b8a090c.JPG)
```javascript
// 스코프의 선두에서 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```
 - 반면에 let으로 선언된 변수는 **선언 단계와 초기화 단계가 분리되어** 진행된다.
![let,const1](https://user-images.githubusercontent.com/37354978/105271725-5e1f3100-5bdb-11eb-8900-7a3ea73d3d34.JPG)
```javascript
// 예시1
// 스코프의 선두에서 선언 단계가 실행된다.
// 아직 변수가 초기화(메모리 공간 확보와 undefined로 초기화)되지 않았다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

  ```javascript
 // 예시2
 console.log(chicken); // undefined
 var chicken = 'nice'; // (할당)
 console.log(chicken); // nice
 // 그래서 첫번째 console.log에 에러가 날 것 같은데 에러가 안나고 undefined가 찍힌 것이다.
```


## let과 const의 차이점 
 - let은 Mutable Data Type 이고, const(constant)는 Immutable Data Type이다.
 - let으로 변수 선언 후 값을 재할당 가능하지만, const로 선언 후 값을 재할당 할 수 없다.
```javascript
let chicken = 'nice';
//success
chicken = 'good';

const PIZZA = 'nice';
PIZZA = 'good';
// error: Assignment to constant variable.
```
 - let은 한 번 값이 할당되고나서 값이 계속해서 변할 수 있지만, const는 한 번 값을 할당하면 값이 절대로 바뀌지 않는다.
 - 코드를 작성할 때 값이 꼭 변경되어야 하는 경우를 제외하고는 **Immutable한 데이터 타입인 const의 사용**을 독려하고 있다.
 - 독려하는 이유는 다음과 같다.
 
 1. 보안

 2. Thread Safety
  
 3. 사람의 실수를 줄인다.
 
 **Reference**
1. https://poiemaweb.com/es6-block-scope
2. https://medium.com/sjk5766/var-let-const-특징-및-scope-335a078cec04

## 마지막 정리
**ES6를 사용 가능하다면 되도록이면 var 대신 let, const를 사용하자** <br>
**값을 재할당 한다면 let, 재할당을 안한다면 const를 사용하자**

