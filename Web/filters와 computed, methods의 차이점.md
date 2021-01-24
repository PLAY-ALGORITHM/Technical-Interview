# filters와 computed, methods의 차이점에 대해 알아보자.

## :question: filters란?
-----------

- 텍스트 형식화를 적용 할 수 있는 역할을 수행한다.
- 중괄호 보간법과 v-bind 표현법을 이용할 때 사용한다.
- filters는 javascript 표현식인 pipe symbol과 함께 추가되어야 한다.

## :question: computed란?
------------

- 반응형 getter 와 같은 역할을 수행한다.
- 템플릿을 보다 직관적으로 표현하기 위해 사용된다.
- computed 내의 function() 은 자신이 참고하고 있는 data값이 재실행 될 필요가 없다면 저장된 값을 제공한다.
- data 속성이 변했을 때만 이를 감지하고 자동으로 다시 연산해준다.

## :question: methods란?
----------------

- 함수를 저장할 수 있는 공간이다.
- 데이터 변경을 모두 캐치한다.
- methods 내의 function()은 자신이 참고하고 있는 data가 변경될 때는 물론이고, 자신의 계산식과는 전혀 상관없는 data2가 변경될 때도 실행된다. 

## :zap: filter의 사용방식
-------------

### :balloon: 필터 사용방법

```html
<!-- 중괄호 보간법 -->
{{ message | capitalize }}

<!-- v-bind 표현 -->
<div v-bind:id="rawId | formatId"></div>
```
### :balloon: 로컬 filter 정의

```javascript
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```
### :balloon: 전역 filter 정의

```javascript
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```
- Vue 인스턴스 생성 전 (new Vue() 전)에 전역으로 필터를 정의할 수 있다.
- 전역 filter의 이름이 로컬 filter와 동일한 경우 로컬 filter가 선호된다.

### :balloon: filter chaining 정의

```html
{{ message | filterA | filterB }}
```
- filter의 함수는 항상 첫번째 전달인자로 표현식의 값(이전 체이닝의 결과) 를 받게 된다. 
- 위의 경우, 하나의 인수를 받는 filterA는 message 값을 받을 것이고, filterA가 message와 함께 실행된 결과가 filterB에 넘겨진다.

### :balloon: filter 함수 전달인자

```html
{{ message | filterA('arg1', arg2) }}
```
- filter 함수는 일반적인 javascript 함수이기 때문에 두개 이상의 전달 인자를 가질 수 있다.
- 위 코드의 filterA filter 함수는 3개의 전달 인자를 가진다. 
    1. 첫번째 전달 인자 : message
    2. 두번째 전달 인자 : 'arg1'
    3. 세번째 전달 인자 : arg2

- 위의 예시보다 이해가 쉽게 설명해 보겠다.
```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'hello1',
  },
  filters : {
    msg2 : function(v){
        return v+'hello2!'
    },
    msg3 : function(v,a,b){
        return v+'hello3!'+a+b;
    }
  }
})
```
```html
<div id="app-2">
      <span v-bind:title="message|msg2|msg3('bye1','bye2')">
             {{message|msg2|msg3('bye1','bye2')}}
        </span>
   </div>
```

## :zap: computed의 사용방식
--------------

- 템플릿 내에 표현식을 넣으면 매우 편리하다. 하지만 간단한 연산일 때만 이러한 방식을 이용하는 것이 좋다. 너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지 보수가 어렵다.

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
위의 템플릿을 보면 message를 역순으로 표기한다는 것을 알 수 있다. 하지만 하나하나 따져서 이해해야 하므로, 복잡해진다면 시간이 오래 걸리고 비효율적이게 된다.

### :balloon: 기본 예제

```html
<div id="example">
  <p>원본 메시지: "{{ message }}"</p>
  <p>역순으로 표시한 메시지: "{{ reversedMessage }}"</p>
</div>
```
```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: '안녕하세요'
  },
  computed: {
    // 계산된 getter
    reversedMessage: function () {
      // `this` 는 vm 인스턴스를 가리킵니다.
      return this.message.split('').reverse().join('')
    }
  }
})
```
#### :bulb: 위 예제의 결과창

![화면 캡처 2021-01-24 144007](https://user-images.githubusercontent.com/73863771/105622148-28d04880-5e52-11eb-9e8a-8f53b5908999.png)

- 이 예제에서는 computed 속성인 reversedMessage를 선언했다. 우리가 작성한 함수는 vm.reversedMessage 속성에 대한 gatter 함수로 사용된다.

```javascript
console.log(vm.reversedMessage) // => '요세하녕안'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```
- 콘솔에 직접 확인해 볼 수 있다. 
- vm.reversedMessage 의 값은 항상 vm.message 의 값에 의존한다.

### :balloon: computed 속성의 setter 함수

- computed 속성은 기본적으로 getter 함수만 가지고 있지만, 필요한 경우 setter 함수를 만들어 쓸 수 있다.

```javascript
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
- 이제 vm.fullname = 'John Doe'를 실행하면 설정자가 호출되고 vm.firstName과 vm.lastName 이 그에 따라 업데이트 된다.

- 또 다른 예시이다.
```html
 <div id = "app">
   <input type = "text" v-model="info">
        이름 : {{name}}, 나이 : {{age}} 
    </div>
```
```javascript
  let vm = new Vue({
            el : "#app",
            data : {
                name : "아이유",
                age : 29
            },
            computed : {
                info : {
                    set : function(v) {
                        v = v.split(" ");
                        this.name = v[0];
                        this.age = v[1];
                    },
                    get : function() {
                        return this.name + " " + this.age;
                    },
                },
            }
        });
```

## :zap: methods의 사용 방식
-----------

- 함수들을 저장하고 있는 곳이 바로 methods 이다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<div id="app">
  <p>{{ title }}</p>
  <p>{{ howAreYou() }}</p>
</div>
```
```javascript
new Vue({ 
  el: "#app", 
  data: {
    title: "안녕 친구들!"
  },
  methods: {
   howAreYou: function () {
     return "기분이 어때?"
    }
  }
})
```
- 위의 코드를 보면 method 내에 howAreYou() 라는 함수를 선언했다.
- {{howAreYou()}} 로 지정해준 곳에 함수의 결과물이 출력된 것을 확인할 수 있다.

#### :bulb: 위 예제의 결과창

![화면 캡처 2021-01-24 152231](https://user-images.githubusercontent.com/73863771/105622841-1c4eee80-5e58-11eb-81eb-aa433182c3d5.png)


## :zap: computed와 methods의 차이
---------------

| computed | methods |
|--------|---------|
|함수로 정의하고 data 객체 등을 사용하여 계산된 값을 리턴해 줌 | 함수로 정의하고 data 객체 등을 사용하여 계산된 값을 리턴해 줌 |
|data 속성에 변화가 있을 때 자동으로 다시 연산(동일한 요청이 또 올 경우는 함수를 실행하지 않고 캐싱된 값만 리턴) | 캐싱이라는 개념이 없기 때문에 매번 재 렌더링(호출될 때마다 계속 함수를 실행) |

### :balloon: computed와 method의 차이점 <예시 1>

- 두 방식 모두 같은 결과를 얻을 수 있다.
```html
<p>뒤집힌 메시지: "{{ reversedMessage1() }}"</p>
```
```javascript
// 컴포넌트 내부
methods: {
  reversedMessage1: function () {
    return this.message.split('').reverse().join('')
  }
}
```
```html
<p>뒤집힌 메시지: "{{ reversedMessage2 }}"</p>
```
```javascript
// 컴포넌트 내부
computed: {
  reversedMessage2: function () {
    return this.message.split('').reverse().join('')
  }
}
```
- computed 속성 대신 메소드와 같은 함수를 정의 가능하다. 위의 표에서 말한 것처럼 최종 결과에 대한 두 가지 접근 방식은 서로 동일하다. 다만, 차이가 나는 부분은 **computed 속성은 종속 대상을 따라 저장(캐싱)된다는 것**이다. 
- computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행한다. 즉 message 가 변경되지 않는 한, computed 속성인 reversedMessage 를 여러번 요청해도 계산을 다시 하지 않고 계산되어 있던 결과를 즉시 반환한다.


- 이에 반해 메소드를 호출할 시에는 렌더링을 다시 할 때마다 **매번** 함수를  실행한다.

### :balloon: computed와 method의 차이점 <예시 2>

```javascript
    window.onload = function(){
        var vm1 = new Vue({
            el : '#test1',
            data : {
                a1 : 100,
                a2 : 200
        },
            methods : {
            test_method : function(){
                console.log('test method')
                return this.a1 + this.a2
            },
            setValue : function(){
                this.a1 = 1000
                this.a2 = 2000
            },
            getRandomMethod : function(){
                return Math.random()
            }
        },
            computed : {
                test_computed : function(){
                    console.log('test computed')
                    return this.a1 + this.a2
                },
                getRandomComputed : function(){
                    return Math.random()
                }
            }
        })
    }
```
```html
 <div id = 'test1'>
        <h3>a1 : {{a1}}</h3>
        <h3>a2 : {{a2}}</h3>
        <h3>a1 + a2 : {{a1 + a2}}</h3>
        <h3>test method : {{test_method()}}</h3>
        <h3>test method : {{test_method()}}</h3>
        <h3>test method : {{test_method()}}</h3>
        <h3>test computed : {{test_computed}}</h3>
        <h3>test computed : {{test_computed}}</h3>
        <h3>test computed : {{test_computed}}</h3>

        <button type = 'button' @click = 'setValue'>값 변경</button>

        <h3>get random method : {{getRandomMethod()}}</h3>
        <h3>get random method : {{getRandomMethod()}}</h3>
        <h3>get random method : {{getRandomMethod()}}</h3>

        <h3>get random computed : {{getRandomComputed}}</h3>
        <h3>get random computed : {{getRandomComputed}}</h3>
        <h3>get random computed : {{getRandomComputed}}</h3>
    </div>
```
#### :bulb: 위 예제의 결과창[1]

![화면 캡처 2021-01-24 200859](https://user-images.githubusercontent.com/73863771/105628415-1c62e480-5e80-11eb-86c5-6a323eac97bc.png)

#### :bulb: 위 예제의 결과창[2]

![화면 캡처 2021-01-24 182958](https://user-images.githubusercontent.com/73863771/105626469-19adc280-5e73-11eb-8e2e-44b13cfda1f6.png)

#### :bulb: 위 예제의 결과창[3]

![화면 캡처 2021-01-24 183202](https://user-images.githubusercontent.com/73863771/105626474-292d0b80-5e73-11eb-9845-57585a04c69b.png)


- **Reference**

https://levelup.gitconnected.com/how-to-use-vuejs-filters-to-write-better-code-4d038bc15e7d
https://vuejs.org/v2/guide/computed.html
https://negabaro.github.io/archive/vuejs-computed__watch__methods
https://webkimsora.tistory.com/53
http://itnovice1.blogspot.com/2019/01/vue-computed-watch-methods.html
https://vueschool.io/lessons/vuejs-computed-properties?friend=vuejs
https://m.blog.naver.com/PostView.nhn?blogId=bkcaller&logNo=221461605896&proxyReferer=https:%2F%2Fwww.google.com%2F
https://sodocumentation.net/ko/vue-js/topic/1878/%EB%A7%9E%EC%B6%A4-%ED%95%84%ED%84%B0
https://goodteacher.tistory.com/200
https://blog.naver.com/ineedsky/221600126643