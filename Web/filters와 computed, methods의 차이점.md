## filters란?
- 텍스트 형식화를 적용 할 수 있는 역할을 수행한다.
- 중괄호 보간법과 v-bind 표현법을 이용할 때 사용한다.
- filters는 javascript 표현식인 pipe symbol과 함께 추가되어야 한다.

## computed란?
- 반응형 getter 와 같은 역할을 수행한다.
- 템플릿을 보다 직관적으로 표현하기 위해 사용된다.
- computed 내의 function() 은 자신이 참고하고 있는 data값이 재실행 될 필요가 없다면 저장된 값을 제공한다.
- data 속성이 변했을 때만 이를 감지하고 자동으로 다시 연산해준다.

## methods란?
- 함수를 저장할 수 있는 공간이다.
- 데이터 변경을 모두 캐치한다.
- methods 내의 function()은 자신이 참고하고 있는 data가 변경될 때는 물론이고, 자신의 계산식과는 전혀 상관없는 data2가 변경될 때도 실행된다. 

## filter의 사용방식
1. 필터 사용방법
```html
<!-- 중괄호 보간법 -->
{{ message | capitalize }}

<!-- v-bind 표현 -->
<div v-bind:id="rawId | formatId"></div>
```
2. 로컬 filter 정의
```javascript
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```
3. 전역 filter 정의
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

```html
<input v-model="message" placeholder="여기를 수정해보세요">
<p>메시지: {{ message }}</p>
```
4. filter chaining 정의
```html
{{ message | filterA | filterB }}
```
- filter의 함수는 항상 첫번째 전달인자로 표현식의 값(이전 체이닝의 결과) 를 받게 된다. 
- 위의 경우, 하나의 인수를 받는 filterA는 message 값을 받을 것이고, filterA가 message와 함께 실행된 결과가 filterB에 넘겨진다.

5. filter 함수 전달인자
```html
{{ message | filterA('arg1', arg2) }}
```
- filter 함수는 일반적인 javascript 함수이기 때문에 두개 이상의 전달 인자를 가질 수 있다.
- 위 코드의 filterA filter 함수는 3개의 전달 인자를 가진다. 
    1. 첫번째 전달 인자 : message
    2. 두번째 전달 인자 : 'arg1'
    3. 세번째 전달 인자 : arg2

## computed의 사용방식

## methods의 사용 방식

## computed와 methods의 차이
| computed | methods |
|--------|---------|
|함수로 정의하고 data 객체 등을 사용하여 계산된 값을 리턴해 줌 | 함수로 정의하고 data 객체 등을 사용하여 계산된 값을 리턴해 줌 |
|data 속성에 변화가 있을 때 자동으로 다시 연산(동일한 요청이 또 올 경우는 함수를 실행하지 않고 캐싱된 값만 리턴) | 캐싱이라는 개념이 없기 때문에 매번 재 렌더링(호출될 때마다 계속 함수를 실행) |

