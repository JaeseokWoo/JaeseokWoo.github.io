---
date: '2022-11-05'
title: '자바스크립트의 const, let, var'
categories: ['JavaScript']
summary: 'conat, let, var'
thumbnail: './javascript-var-let-const.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard) 의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

## 변수를 정의하는 새로운 방법: const, let

ES5까지의 자바스크립트에서는 var를 이용해서 변수를 정의했고 그게 유일한 방법이었습니다.<br>
ES6에서는 const, let을 이용하는 새로운 변수 정의 방법이 생겼습니다.<br>
새로운 방법이 나온 이유는 기존 방식인 var로는 해결되지 않는 문제가 있었기 때문입니다.

### var가 가진 문제

#### var의 문제 1. 함수 스코프

var의 첫 번째 문제는 정의된 변수가 함수 스코프를 가진다는 것입니다.

> 스코프(scope)란?<br>
> 변수가 사용될 수 있는 영역을 말합니다.<br>
> 스코프는 변수가 정의된 위치에 의해 결정됩니다.<br>

https://developer.mozilla.org/ko/docs/Glossary/Scope

var는 함수 스코프이기 때문에 `코드 1`과 같이 함수를 벗어난 영역에서 사용하면 에러가 발생합니다.

```js
// 코드 1 - 스코프를 벗어나서 변수를 사용하면 에러 발생
function example() {
  var i = 1;
}
console.log(i); // 참조 에러
```

실행 결과

```shell
console.log(i); // 참조 에러
            ^
ReferenceError: i is not defined
```

var 변수를 함수가 아닌 프로그램의 가장 바깥에서 정의하면 전역변수가 되며, 특이한 점은 함수 안에서 var 키워드를 사용하지 않고 변수에 값을 할당하면 그 변수는 전역 변수가 된다는 점입니다.

```js
// 코드 2 - var 키워드 없이 변수를 정의하면 전역 변수가 됩니다.
function example1() {
  i = 1;
}
function exapmle2() {
  console.log(i);
}
example1();
example2(); // 1
```

실행 결과

```shell
1
```

이런 상황에서 명시적 에러가 발생하도록 하려면 파일 상단에 `use strict`를 선언하면 됩니다.

```js
// 코드 2-1
'use strict';

function example1() {
  i = 1;
}
function example2() {
  console.log(i);
}
example1();
example2();
```

실행 결과

```shell
  i = 1;
    ^

ReferenceError: i is not defined
```

var는 함수 스코프이기 때문에 for 반복문에서 정의된 변수가 반복문이 끝난 이후에도 계속 남는 문제점이 발생합니다.

```js
// 코드 3 - for 문을 벗어나도 변수가 사라지지 않습니다.
for (var i = 0; i < 10; i++) {
  console.log(i);
}
console.log(i); // 10
```

실행 결과

```shell
0
1
2
3
4
5
6
7
8
9
10
```

for문 뿐만 아니라 while문, switch문, if문 등 모든 코드는 같은 문제를 안고 있습니다.

이러한 var 변수의 스코프를 제한하기 위해 즉시 실행 함수를 사용하기도 합니다.

> 즉시 실행 함수<br>
> 함수를 정의하는 시점에 바로 실행되고 사라집니다.

https://developer.mozilla.org/ko/docs/Glossary/IIFE

var 변수는 함수 스코프이므로 즉시 실행 함수로 묶으면 변수의 스코프를 제한할 수 있지만, 즉시 실행 함수를 작성하기 번거롭고 가독성도 떨어진다는 문제점이 있습니다.

#### var의 문제 2. 호이스팅

> 호이스팅(hoisting)<br>
> 정의된 변수는 그 변수가 속한 스코프의 최상단으로 끌러올려진 것 같은 현상

https://developer.mozilla.org/ko/docs/Glossary/Hoisting

var로 정의된 변수는 호이스팅이 일어난다.

```js
// 코드 4 - 변수가 정의된 시점보다 먼저 변수 사용하기
console.log(myVar); // undefined
var myVar = 1;
```

실행 결과

```shell
undefined
```

변수를 정의하기 전에 사용했음에도 이 코드를 실행하면 에러가 발생하지 않으며, 특이한 점은 `1`이 아니라 `undefined`가 출력된다는 점입니다.

이것은 해당 변수의 정의가 위쪽으로 끌어올려졌기 때문이고, 코드가 `코드 5`처럼 변경됐다고 생각하면 이해하기 쉽습니다.

```js
// 코드 5 - 호이스팅의 결과
var myVar = undefined;
console.log(myVar); // undefined
myVar = 1;
```

변수의 정의만 끌어올려지고 값은 원래 정의했던 위치에서 할당됩니다.<br>
특이하게도 `코드 6`처럼 변수가 정의된 곳 위에서 값을 할당할 수도 있습니다.

```js
// 코드 6 - 변수가 정의된 시점보다 먼저 변수에 값을 할당하기
console.log(myVar); // undefined
myVar = 2;
console.log(myVar); // 2
var myVar = 1;
```

실행 결과

```shell
undefined
2
```

#### var의 문제 3. 재정의

var의 또 다른 문제는 한 번 정의된 변수를 재정의할 수 있습니다.

```js
var myVar = 1;
var myVar = 2;
```

변수를 정의한다는 것은 이전에 없던 변수를 생성한다는 의미로 통용됩니다.<br>
따라서 위의 코드가 에러 없이 사용될 수 있다는 것은 직관적이지 않으며 버그로 이어질 수 있습니다.<br>
뿐만 아니라, var는 재할당 가능한 변수로만 만들 수밖에 없다는 점입니다.<br>
상수처럼 쓸 값도 무조건 재할당 가능한 변수로 만들어서 사용할 수밖에 없습니다.

### var의 문제를 해결하는 const, let

#### const, let은 블록 스코프

var는 함수 스코프였지만 const, let은 블록(block) 스코프입니다.<br>
블록 스코프는 대부분의 언어에서 사용하므로 개발자에게 익숙한 개념입니다.<br>

`코드 7`와 같이 if문의 블록 안에서 정의된 변수는 if문을 벗어나면 참조할 수 없으며, 사용하려고 하면 에러가 발생합니다.<br>

```js
// 코드 7 - 블록 스코프에서는 블록을 벗어나면 변수를 사용할 수 없습니다.
if (true) {
  const i = 0;
}
console.log(i); // 참조 에러
```

실행 결과

```shell
console.log(i); // 참조 에러
            ^

ReferenceError: i is not defined
```

이번에는 블록 스코프에서 같은 이름의 변수를 정의하는 경우를 확인해보겠습니다.

```js
// 코드 8 - 블록 스코프에서 같은 이름을 갖는 변수의 사용 예시
let foo = 'bar1'; // (1)
console.log(foo); // bar1
if (true) {
  let foo = 'bar2';
  console.log(foo); // bar2
}
console.log(foo);
```

실행 결과

```shell
bar1
bar2
bar1
```

마지막 줄의 foo 변수는 같은 블록에서 정의된 변수 (1)를 참조하므로 bar1을 출력합니다.

#### const, let의 호이스팅

const와 let으로 정의된 변수 또한, 호이스팅이 됩니다.<br>
하지만, const, let은 변수를 정의하기 전에 사용하려고 하면 참조 에러가 발생합니다.

```js
// 코드 9 - const, let 변수는 변수가 정의된 시점보다 먼저 변수를 사용할 수 없습니다.

console.log(foo); // 참조 에러
const foo = 1;
```

실행 결과

```shell
console.log(foo); // 참조 에러
            ^

ReferenceError: Cannot access 'foo' before initialization
```

실행 결과를 통해 const, let으로 정의된 변수는 호이스팅이 되지 않는다고 생각하지만, const, let으로 정의된 변수도 호이스팅이 일어납니다.<br>
다만, 변수가 정의된 위치와 호이스팅된 위치 사이에서 변수를 사용하려고 하면 에러가 발생하고, 이 구간을 임시적 사각지대(temporal dead zone)라고 부릅니다.<br>
var로 정의된 변수에는 임시적 사각지대가 없기 때문에 참조 에러가 발생하지 않는 것입니다.

#### const는 변수를 재할당 불가능하게 만듭니다.

cons로 정의된 변수는 재할당이 불가능합니다.

반대로, let, var로 정의된 변수는 재할당할 수 있습니다.

재할당 불가능한 변수는 프로그램의 복잡도를 상당히 낮춰주기 때문에 되도록이면 재할당이 불가능한 변수를 사용하는게 좋습니다.

```js
// 코드 10 - const로 정의된 변수만 재할당 불가능 합니다.

var foo = 'a';
foo = 'b'; // 에러 없음
let value = 'a';
value = 'b'; // 에러 없음
const bar = 'a';
bar = 'b'; // 에러 발생
```

실행 결과

```shell
bar = 'b'; // 에러 발생
    ^

TypeError: Assignment to constant variable.
```

다만, const 정의된 객체의 내부 속성값은 수정 가능하다는 점을 주의해야 합니다.

```js
// 코드 11 - const로 정의해도 객체의 내부 속성값은 수정 가능합니다.
const bar = { prop1: 'a' };
bar.prop1 = 'b';
bar.prop2 = 123;
console.log(bar); // { prop1: 'b', prop2: 123 }
const arr = [10, 20];
arr[0] = 100;
arr.push(300);
console.log(arr); // [ 100, 20, 300 ]
```

실행 결과

```shell
{ prop1: 'b', prop2: 123 }
[ 100, 20, 300 ]
```

객체의 내부 속성값도 수정 불가능하게 만들고 싶다면 `immer`, `immutable.js` 등의 외부 패키지를 활용하는게 좋습니다.
이러한 외부 패키지는 객체를 수정하려고 할 때, 기존 객체는 변경하지 않고 새로운 객체를 생성합니다.

새로운 객체를 생성하는 편의 기능은 필요 없고 수정만 할 수 없도록 차단하고 싶다면, 자바스크립트 내장 함수를 이용하면 됩니다.

- [Object.preventExtensions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
- [Object.seal](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
- [Object.freeze](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
