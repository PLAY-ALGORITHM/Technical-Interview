
## DOM (돔) 
### 정의
- Document Object Model 그리고 문서 객체 모델이며 HTML 및 XML 문서를 위한 API이다.  
- 이 DOM이란 트리 구조로 되어있는 객체 모델로써, Javascript가 getElementbyid()를 같은 함수를 이용하여 HTML문서의 각 요소(li, head같은 태그들)들을 접근하고 사용할 수 있도록 하는 객체 모델이다. 브라우저마다 DOM을 구현하는 방식은 다르기에 DOM이라는 것이 구체적으로 정해저 있는 언어나 모델과 같은 것은 아니다. 다만 웹페이지를 객체로 표현한 모델을 의미할 뿐이다.

#### DOM 의 트리구조 예시
![enter image description here](https://miro.medium.com/max/486/1*LA6AXbzvC0IQ_d2H8v3NCw.gif)

#### 	브라우저가 돔을 이용해서 화면을 렌더링 하는 방법 예시 
![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/cLx0Fj/btqzEC7eCe7/pOWQArZDNwHiQrvrdTDqdk/img.png)
**첫째,**  브라우저는 html태그를 파싱 하여  **돔 트리를**  구성한다.
**동시에**  브라우저는 스타일시트에서 css를 파싱 하여  **스타일 규칙**들을 만들어낸다.
**둘째,**  위에서 언급한 돔 트리와 스타일 규칙 두 가지가 합쳐져서  **렌더 트리를**  만들어낸다.


## VIRTUAL DOM (가상 돔) 

## 정의 

> A virtual DOM is a lightweight JavaScript representation of the DOM used in declarative web frameworks such as React, Vue.js, and Elm. Updating the virtual DOM is comparatively faster than updating the actual DOM, since nothing has to be rendered onto the screen. *- Wikipedia -*
> 
> 가상돔이란 돔을 가볍게 만든 자바스크립트 표현이라고 할 수 있고 주로 React, Vue.js 그리고 Elm에 사용된다. 가상돔은 실제로 스크린에 랜더링을 하는것이 아니기에 돔을 직접 업데이트 하는것보다 상대적으로 빠르다.

## 필요성및 차이점 

### 돔의 문제

> SPA (Single Page Application)의 출현으로 가상 돔이 매우 필요해졌다. 

DOM은 트리구조로 되어있어서 이해하기 쉽다는 장점은 있으나, 이 구조는 거대해 질때 속도가 느려지고, DOM 업데이트에 잦은 오류를 야기한다. DOM을 분석하는데에도 힘든 단점이 있다. 

그런데 최근 모던 웹은 SPA(Single Page Application) 을 사용한다. 하나의 웹 페이지를 어플리케이션 처럼 구성하는 SPA에서는 HTML 문서 자체가 하나이며, Application이라 칭 할 만큼 여러 동적인 기능(실시간 기능)들이 들어가기 때문에 안그래도 리소스가 모두 합쳐진 HTML 문서를 지속적으로 재 렌더링 해주어야 하는 문제점이 발생하게 되었다. 

기존에는 화면의 변경사항을 돔을 직접 조작하여 브라우저에 반영하였다. 하지만, 이 방법의 가장 큰 단점은 돔 트리가 수정될 때마다 렌더 트리가 계속해서 실시간으로 갱신된다는 점이었다. 즉, 화면에서 10개의 수정사항이 발생하면 수정할 때마다 새로운 랜더 트리가 10번 수정되면서 새롭게 만들어지게 되는 것이다.

### 그래서 가상 돔이 출현 

 - 가상돔(Virtual DOM)은 실제 DOM 문서를 추상화한 개념으로, 변화가 많은 View를 실제 DOM에서 직접 처리하는 방식이 아닌 Virtual Dom과 메모리에서 미리 처리하고 저장한 후 실제 DOM과 동기화 하는 프로그래밍 개념입니다.
   
 - 해당 DOM을 컴포넌트 단위로 쪼개어 HTML 컴포넌트 조립품 처럼 다루는 개념이다. 이 조립품들이 VirtualDOM이라 할 수 있다.

- Vue, React는 가상 돔 방식을 채택했다. 


|돔|가상돔|
|--|--|
|업데이트가 느리다.|업데이트가 빠르다.|
|HTML을 직접적으로 업데이트 한다.| HTML을 직접적으로 업데이트 하지 않는다.|
|새로운 element가 업데이트 된 경우 새로운 DOM을 생성한다.|새로운 element가 업데이트 된 경우 새로운 가상돔 생성한 후 이전 돔과 비교하여 차이를 계산 이후 돔을 업데이트한다.|
|메모리 낭비가 심하다.|메모리 낭비가 덜하다.|

**업데이트가 빠른것은 가상돔이 돔보다 빠르다는 표현과는 다르다.* 
*VDOM의 주목적은 DOM의 직접적 변화의 수를 줄이는데 있고 이 변화가 느린것이다.* 

### 아래와 같은 코드
돔은 브라우저가 핸들하여 생성한다. 
``` html
<ul class="fruits">
    <li>Apple</li>
    <li>Orange</li>
    <li>Banana</li>
</ul>
```
### Virtual DOM 식으로 표현된 코드 
React, Vue 등의 모던 프레임워크가 실제 돔과 비슷한 가상돔을 메모리에 생성한다. 
```javascript
// Virtual DOM representation
{
  type: "ul",
  props: {
    "class": "fruits"
  },
  children: [
    {
      type: "li",
      props: null,
      children: [
        "Apple"
      ]
    },
    {
      type: "li",
      props: null,
      children: [
        "Orange"
      ]
    },
    {
      type: "li",
      props: null,
      children: [
        "Banana"
      ]
    }
  ]
}
```
[출처](https://dev.to/karthikraja34/what-is-virtual-dom-and-why-is-it-faster-14p9)



## 가상돔의 Three Steps
1. 어떠한 데이터의 변화가 일어났을때, 전체 UI가 가상 돔에서 재 랜더링이 된다.  
![enter image description here](https://www.edureka.co/blog/wp-content/uploads/2017/08/1dom.png)

2. 이전 돔과 새로운 가상돔의 차이가 계산된다. 
![enter image description here](https://www.edureka.co/blog/wp-content/uploads/2017/08/2dom.png)

3. 계산이 끝난 후, 돔은 변경된 부분만을 변화시킨다. 
![enter image description here](https://www.edureka.co/blog/wp-content/uploads/2017/08/3dom.png)


### REACT/Vue.js/Angular/Elm 가 사용하는 가상 돔
첫 번째로 가상 돔은 리액트에서 나온 개념이 아니다. 

하지만 리액트는 무료로 이 개념을 사용하고 사용자들에게 제공한다. 퍼포먼스와  메모리 관리가 효율적으로 가능하게 되었다. 가상 돔을 활용하면 이러한 불필요한 렌더링 횟수를 줄일 수 있다. 가상 돔을 활용한 대표적인 프런트 앤드 프레임워크가 리액트, 뷰, 앵귤러이다. 이러한 프레임워크들은 화면에 변화가 있을 때마다 실시간으로 돔 트리를 수정하지 않고 변경사항이 모두 반영된 가상 돔을 만들어낸다. 그 후 가상 돔을 이용해 **한 번만 돔수정을 하게 되고 이는 한 번만 렌더 트리를 만들어**내게 된다. 결과적으로 브라우저는 한번만 렌더링을 하게 됨으로써 **불필요한 렌더링 횟수**를 줄일 수 있게 되는 것이다.

![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bgW4xU/btqzFeLIjMG/yGgjkkr7mnMX9pyVRowywK/img.png)


< 추가 링크 >
https://bitsofco.de/understanding-the-virtual-dom/
