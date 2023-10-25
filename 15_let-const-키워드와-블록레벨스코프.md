# 15. let, const 키워드와 블록 레벨 스코프

## 15.1 var 키워드로 선언한 변수의 문제점

### 15.1.1 변수 중복 선언 허용

```jsx
var x = 1;
var y = 1;
var x = 100;
var y;

console.log(x); //100
console.log(y); //1
```

초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시되며, 에러는 발생하지 않는다.

### 15.1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 **함수의 코드 블록만을 지역 스코프**로 인정한다. 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록(for,if..) 내에서 선언해도 모두 전역 변수가 된다.

```jsx
var x = 1;

if (true) {
  var x = 10;
}

console.log(x); //10
```

### 15.1.3 변수 호이스팅

var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

```jsx
console.log(foo); //undefined
foo = 123;
console.log(foo); //123
var foo;
```

## 15.2 let 키워드

### 15.2.1 변수 중복 선언 금지

let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```jsx
let bar = 123;
let bar = 456; //SyntaxError:Identifier 'bar' has already been declared
```

### 15.2.2 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let foo = 1;
{
  let foo = 2;
  let bar = 3;
}
console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 15.2.3 변수 호이스팅

let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생한다. “선언 단계”와 “초기화 단계”가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 시점(변수 선언문)까지 변수를 참조할 수 없다. 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대(TDZ)**라고 부른다.

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
console.log(foo); // undefined
foo = 1;
console.log(foo); // 1
```

### 15.2.4 전역 객체와 let

var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

let 키워드로 선언한 전역변수는 전역 객체의 프로퍼티가 아니다. let 전역 변수는 보이지 않은 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.

```jsx
var x = 1; //  window.x
y = 2; // window.y
function foo() {} // window.foo

let z = 1;
console.log(window.z); // undefined
```

## 15.3 const 키워드

const 키워드는 상수를 선언하기 위해 사용한다.

### 15.3.1 선언과 초기화

**const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화 해야한다.**

```jsx
const foo; //SyntaxError: Missing initializer in const declaration
```

const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록레벨스코프를 가지며 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
}

console.log(foo); //ReferenceError: foo is not defined
```

### 15.3.2 재할당 금지

const 키워드로 선언한 변수는 **재할당이 금지 된다**.

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 15.3.3 상수

const 키워드로 선언한 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고, const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다 (스네이크\_케이스)

### 15.3.4 const 키워드와 객체

const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. 재할당을 금지할 뿐 “불변”을 의미하지 않는다. 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.
