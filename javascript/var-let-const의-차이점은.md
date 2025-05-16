# var-let-const의 차이점은?

JavaScript에서 변수를 선언할 수 있는 세 가지 키워드는 `var`, `let`, `const`입니다.

> "var는 절대 사용하지 말고, let과 const를 사용해야 한다."  
> 이 말은 왜 나왔을까요?

`var`와 `let`은 **변수 선언 키워드**, `const`는 **상수 선언 키워드**입니다.  
JavaScript 초창기에는 `var`만 있었지만, ES6(ES2015)에서 `let`과 `const`가 도입되었습니다.


## ✅ var와 let의 주요 차이점 3가지

1. **스코프(scope)**
2. **중복 선언(variable redeclaration)**
3. **호이스팅(hoisting)**


### 1. 스코프

```js
function main() {
  if (true) {
    var x = 'hi';
  }
  console.log(x); // ✅ hi
}

function main() {
  if (true) {
    let x = 'hi';
  }
  console.log(x); // ❌ ReferenceError
}
```
- var는 함수 스코프를 가지며, 블록 바깥에서도 접근 가능합니다.
- let은 블록 스코프를 가지며, 선언된 블록 바깥에서는 접근이 불가능합니다.

💡 추가 설명: var로 전역 변수를 선언하면 브라우저 환경에서는 window 객체의 속성으로 올라가 전역 충돌의 원인이 됩니다.

### 2. 중복 선언
```js
var x = '안녕하세요';
var x = 'hi';
console.log(x); // hi

let y = '안녕하세요';
let y = 'hi'; // ❌ SyntaxError
```
- var는 같은 이름으로 중복 선언이 가능 합니다.
- let은 같은 스코프 내에서 중복 선언이 불가능 합니다.

###  3. 호이스팅
호이스팅은 변수의 선언과 변수의 초기화 (할당)으로 분리해서 생각했을때,
선언만을 코드의 최상으로 끌어올려서 사용하고 있는 변수만 먼저 알리는 작업입니다
```js
console.log(num); // undefined
var num = 10;

console.log(num); // ❌ Cannot access 'num' before initialization
let num = 10;
```
- var는 선언과 초기화가 함께 호이스팅되어 undefined 출력이 됩니다.
- let은 호이스팅은 되지만, 초기화 전까지 접근이 불가능 합니다. (TDZ, Temporal Dead Zone 발생)

## ✅ const는 무엇이 다른가?
var 와 let을 먼저 비교한 이유가 있습니다.
<br/>
const는 let과 스코프, 호이스팅 동작은 같지만, const는 “재할당이 불가능”한 상수 선언이라는 점이 다릅니다.
```js
let a = 'hi';
a = 'hello'; // ✅ 가능

const b = 'hi';
b = 'hello'; // ❌ TypeError: Assignment to constant variable
```

###  객체인 경우 예외
```js
const obj = {
  a: 1,
  b: 2
};

obj.b = 4; // ✅ 가능
```
- const로 선언된 값이 원시값이 아닌 경우에는, 객체의 속성값을 바꾸는 것이 가능 하다는 것입니다.
- 하지만 변수 자체를 다른 객체로 할당은 것은 여전히 불가능 합니다.

## What-ziny-say
var, let, const 모두 변수를 선언하는 키워드 입니다. <br/>
최근 개발자들 사이에서는 var는 사용하지 말고 let과 const를 사용해야된다 는 말이 있는데요. <br/>
var는 함수 스코프를 가지고 있고, 중복 선언이 되고, 호이스팅 될 때 초기값을 undefined로 가지는 점에 예기치 못한 문제 발생 가능성이 높습니다. <br/>
그에 비에 let 또는 const는 블록 스코를 가지고 있어서, 좀 더 안전하게 사용이 가능합니다. <br/>
let 과 const 둘 다 중복 선이 불가능하고, 호이스팅으로 선언은 되지만 <br/>
TDZ가 발생해서 예기치 못한 문제의 발생 확률이 줄어든다는 장점이 있습니다. <br/>
let은 재선언만 불가능 할 뿐 값의 재할당이 가능하기 때문에, const보다는 위험도가 높은 것은 사실이나<br/>
무조건 const를 써야하는 것은 아니고, 변수의 특성에 맞게 let과 const를 골라서 사용해야 합니다. <br/>
실제로 저는 const를 우선으로 사용하고, 필요한 경우에만 let을 사용하고 있습니다.<br/>

그 외에도 var를 전역변수로 쓰면 window 객체로 올라가버려 전역 충돌의 원인이 되기도 하니 더 주의해야 합니다.
