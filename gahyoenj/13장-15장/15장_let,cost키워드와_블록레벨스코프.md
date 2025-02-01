## var 키워드로 선언한 변수의 문제점

### 변수 중복 선언 허용

- var 키워드로 선언한 변수는 중복 선언이 가능

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 허용
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
var x = 100;
// 초기화문이 없는 변수 선언문은 무시됨
var y;

console.log(x); // 100
console.log(y); // 1
```

⇒ 동일한 변수 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값을 할당하면 의도치 않게 먼저 선언된 변수 값이 변경

### 함수 레벨 스코프

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정
- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됨

```jsx
var x = 1;

if (true) {
  // x는 전역 변수. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언
  var x = 10;
}

console.log(x); // 10
```

- 함수 레벨 스코프는 전역 변수를 남발할 가능성을 높임

### 변수 호이스팅

- var 키워드로 변수 선언하면 변수 호이스팅에 의해 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작

```jsx
console.log(foo); // undefined << foo 변수 암묵적 선언 & 초기화 (호이스팅)

foo = 123; // foo변수 값 할당

console.log(foo); // 123

var foo;
```

- 변수 선언문 이전에 변수를 참조하는 것은 에러를 발생시키지는 않지만 프로그램의 흐름상 맞지 않고 가독성을 떨어뜨리고 오류 발생 여지를 남김

## let 키워드

### 변수 중복 선언 금지

- let 키워드로 이름이 같은 변수를 중복 선언 하면 SyntaxError 발생

```jsx
// var 변수 = 중복 선언 허용
var foo = 123;
var foo = 456;
console.log(foo);

// let 변수 = 중복 선언 허용 X
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
console.log(bar);

// 선언 단계 에서 체크하므로 → 위에서 var 변수인 foo에 대한 출력문 실행이 진행 안됨
```

### 블록 레벨 스코프

- let 키워들 ㅗ선언한 변수는 모든 코드블록(함수, if 문, for문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따름

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

⇒ 전역에서 선언된 foo 변수와 코드 블록 내에서 선언된 foo 변수는 별개의 변수

⇒ bar 변수도 블록 레벨 스코프를 갖는 지역 변수이므로 전역에서는 bar 변수를 참조 할 수 없음

### 변수 호이스팅

- let 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

- var로 선언한 변수는 런타임 이전에 선언단계와 초기화 단계가 한번에 진행
  - 선언단계에서 자바스크립트 엔진에 변수의 존재의 알리고 즉시 초기화 단계에서 undefined로 변수를 초기화
- **let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행**
  - 선언 단계가 먼저 실행
  - 초기화 단계는 변수 선언문에 도달했을 때 실행
  - 초기화 이전에 변수에 접근하려고 하면 참조에러 발생
  - 스코프의 시작 지점 부터 초기화 시작 지점까지 변수를 참조 할 수 없는 구간 = **일시적 사각지대(TDZ)**

```jsx
console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
// 일시적 사각지대(TDZ)

let foo; // 변수 선언문에서 초기화 단계가 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```

```jsx
let foo = 1; // 전역 변수
{
  console.log(foo); // ReferenError : Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

⇒ 호이스팅이 발생하지 않는 다면 전역 변수 foo 값을 출력 해야함

⇒ 하지만 호이스팅이 발생하기 때문에 참조 에러가 발생

### 전역 객체와 let

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
- let 전역 변수는 보이지 않는 개념적인 블록 내에 존재

```jsx
// 브라우저 환경 가정
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아님
console.log(window.x); // undefinde
console.log(x); // 1
```

## const 키워드

- const 키워드는 상수를 선언하기 위해 사용

### 선언과 초기화

- **const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화 해야함**
- 그렇지 않으면 `SyntaxError: Missing intializer in const declaraion` 발생
- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며 변수 호이스팅이 발생하지 않는 것처럼 동작

### 재할당 금지

- const 키워드로 선언한 변수는 재할당이 금지됨

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 상수

- const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다
- 원시 값은 변경 불가능한 값이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문
- const 키워드를 상수를 표현하는데 사용함
- 상수 = 재할당이 금지된 변수
- const 키워드로 선언된 변수에 원시값을 할당하면 원시 값은 변경 할 수 없는 값, const 키워드는 재할당 금지이므로 할당된 값을 변경할 수 있는 방법이 없음 ⇒ 유지보수성 향상

```jsx
// TAX_RATE 라는 상수(constant)를 적용하므로써, 코드의 가독성이 증가한다.
const TAX_RATE = 0.1;

let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```

### const 키워드와 객체

- **const 키워드로 선언된 변수에 객체를 할당한 경우에는 값을 변경할 수 있음**
- 객체는 변경 가능한 값이므로 직접 변경 가능

```jsx
const person = {
  name: "Lee",
};

// 객체는 변경 가능한 값(mutable value) == 재할당 없이 변경 가능
person.name = "Kim";
console.log(person.name); // { name: "Kim" };
```

- const 키워드는 재할당을 금지할 뿐 불변을 의미하지는 않음
- 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능

## var vs. let vs. const

- 변수 선언에는 기본적으로 const 사용하고 let은 재할당이 필요한 경우 한정해 사용하는 것이 좋음
- ES6를 사용한다면 var 키워드는 사용x
- 재할당이 필요한 경우 한정해 let 키워드 사용. 이때 변수의 스코프는 최대한 좁게
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드 사용
- const 키워드는 재할당을 금지하므로 var, let 보다 안전
