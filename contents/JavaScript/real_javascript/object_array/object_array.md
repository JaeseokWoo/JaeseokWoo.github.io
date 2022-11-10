---
date: '2022-11-09'
title: '자바스크립트의 객체와 배열'
categories: ['JavaScript']
summary: 'object, array'
thumbnail: './javascript-var-let-const.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

## 객체와 배열을 생성하고 수정하기

### 단축 속성명

단축 속성명(shorthand property names)은 객체 리터럴 코드를 간편하게 작성할 목적으로 만들어진 문법입니다.

단축 속성명을 사용하면 `코드 1`과 같이 간편하게 새로운 객체를 만들 수 있습니다.

```js
// 코드 1 - 단축 속성명을 사용해서 객체를 생성하기
const name = 'mike';
const obj = {
  age: 21,
  name,
  getName() {
    return this.name;
  }, // (1)
};
```

(1) 새로 만들려는 객체의 속성값 일부가 이미 변수로 존재하면 간단하게 변수 이름만 적어주면 됩니다.

(2) 속성값이 함수이면 function 키워드 없이 함수명만 적어도 됩니다.

이때 속성명은 함수명과 같아집니다.

### 계산된 속성명

계산된 속성명(computed property names)은 객체의 속성명을 동적으로 결정하기 위해 나온 문법입니다.

```js
// 코드 2 - 계산된 속성명을 사용하지 않는 코드와 사용하는 코드 비교
function makeObject1(key, value) {
  // (1)
  const obj = {};
  obj[key] = value;
  return obj;
}
function makeObject2(key, value) {
  // (2)
  return { [key]: value };
}
```

계산된 속성명을 사용하면 같은 함수를 (2)번처럼 간결하게 작성할 수 있습니다.

## 객체와 배열의 속성값 가져오기

### 전개 연산자

전개 연산자(spread operator)는 배열이나 객체의 모든 속성을 풀어놓을 때 사용하는 문법입니다.

```js
// 코드 3 - 전개 연산자를 이용해서 함수의 매개변수를 입력하기
Math.max(1, 3, 7, 9); // (1)
const numbers = [1, 3, 7, 9];
Math.max(...numbers); // (2)
```

(1)과 같은 방식으로는 동적으로 매개변수를 전달할 수 없습니다.

(2)와 같은 전개 연산자를 사용하면 동적으로 함수의 매개변수를 전달할 수 있습니다.

<hr>
동적으로 함수의 매개변수를 전달하는 방법

전개 연산자를 사용하지 않고도 동적으로 함수의 매개변수를 전달할 수 있습니다.

```js
// 코드 4 - apply 함수를 이용해서 동적으로 함수의 매개변수 입력하기
const numbers = [1, 3, 7, 9];
Math.max.apply(null, numbers);
```

this 바인딩이 필요하지 않기 때문에 `apply()`함수의 첫 번째 매개변수로 `null`을 입력하였습니다.

전개 연산자 방식보다 작성하기 번거롭고 가독성도 떨어지는 것을 알 수 있습니다.

<hr>

전개 연산자를 통해 배열이나 객체를 복사할 때도 유용합니다.

```js
// 코드 5 - 전개 연산자를 이용해서 배열과 객체 복사
const arr1 = [1, 2, 3];
const obj1 = { age: 23, name: 'mike' };
const arr2 = [...arr1];
const obj2 = { ...obj1 };
arr2.push(4);
obj2.age = 80;

console.log(arr1);
console.log(obj1);
console.log(arr2);
console.log(obj2);
```

실행 결과

```shell
[ 1, 2, 3 ]
{ age: 23, name: 'mike' }
[ 1, 2, 3, 4 ]
{ age: 80, name: 'mike' }
```

전개 연산자를 사용해서 새로운 객체와 배열을 생성했고, 새롭게 생성되었기 때문에 속성을 추가하거나 변경해도 원래의 객체에 연향으 주지 않습니다.

전개 연산자를 사용하면 서로 다른 두 배열이나 객체를 쉽게 합칠 수 있습니다.

```js
// 코드 6 - 전개 연산자를 이용해서 두 객체를 병합하기
const obj1 = { age: 21, name: 'mike' };
const obj2 = { hobby: 'soccer' };
const obj3 = { ...obj1, ...obj2 };
console.log(obj3);
```

실행 결과

```shell
{ age: 21, name: 'mike', hobby: 'soccer' }
```

ES5까지는 중복된 속성명을 사용하면 에러가 발생했지만, ES6부터는 중복된 속성명을 허용하기 때문에 중복된 속성명을 사용하면 최종 결과는 마지막 속성명의 값이 됩니다.

```js
// 코드 7 - 객체 리터럴에서 중복된 속성명 사용 가능
const obj1 = { x: 1, x: 2, y: 'a' };
const obj2 = { ...obj1, y: 'b' };
console.log(obj1);
console.log(obj2);
```

실행 결과

```shell
{ x: 2, y: 'a' }
{ x: 2, y: 'b' }
```

### 배열 비구조화

배열 비구조화(array destructuring)는 배열의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다.

```js
// 코드 8 - 배열 비구조화를 사용한 간단한 코드
const arr = [1, 2];
const [a, b] = arr;
console.log(a);
console.log(b);
```

실행 결과

```shell
1
2
```

배열 비구조화 시 기본값을 정의할 수 있습니다.
배열의 속성값이 `undefined`라면 정의된 기본값이 할당되고, 그렇지 않다면 원래의 속성값이 할당됩니다.

```js
// 코드 - 9 배열 비구조화에서의 기본값
const arr = [1];
const [a = 10, b = 20] = arr;
console.log(a);
console.log(b);
```

실행 결과

```shell
1
20
```

첫 번째 변수의 속성값은 존재하기 때문에 기본값 `10`은 사용되지 않고 속성값이 그대로 할당됩니다.

두 번째 변수의 속성값은 `undefined`이므로 기본값 20이 할당됩니다.

배열 비구조화를 사용하면 두 변수의 값을 쉽게 교환할 수 있습니다.

```js
// 코드 - 10 배열 비구조화를 이용해서 두 변수의 값을 교환하기
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

실행 결과

```shell
2
1
```

일반적으로 두 변수가 값을 교환하기 위해서는 제 3의 변수를 이용하는게 일반적입니다.

하지만 배열 비구조화를 사용하면 제 3의 변수없이 코드를 구현할 수 있습니다.

배열에서 일부 속성값을 무시하고 진행하고 싶다면 건너뛰는 개수 만큼 쉼표를 입력하면 됩니다.

```js
// 코드 11 - 쉼표를 이용해서 일부 속성값을 건너뛰기
const arr = [1, 2, 3];
const [a, , c] = arr;
console.log(a); // 1
console.log(b); // 3
```

첫 번째 속성값은 변수 a에 할당되고, 두 번째 속성값은 건너뛰고 세 번째 속성값이 변수 c에 할당됩니다.

쉼표 개수만큼을 제외한 나머지를 새로운 배열로 만들 수도 있습니다.

```js
// 코드 12 - 나머지 값을 별도의 배열로 만들기
const arr = [1, 2, 3];
const [first, ...rest1] = arr;
console.log(rest1); // [2, 3]
const [a, b, c, ...rest2] = arr;
console.log(rest2); // []
```

배열 비구조화 시 마지막에 ...와 함께 변수명을 입력하면 나머지 모든 속성값이 새로운 배열로 만들어집니다.

나머지 속성값이 존재하지 않으면 빈 배열이 만들어집니다.

### 객체 비구조화

객체 비구조화(object destructuring)는 객체의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다.

```js
// 코드 13 - 객체 비구조화 간단한 예
const obj = { age: 21, name: 'mike' };
const { age, name } = obj;
console.log(age); // 21
console.log(name); // mike
```

객체 비구조화에서는 중괄호를 사용합니다.

배열 비구조화에서는 배열의 순서가 중요했지만 객체 비구조화에서는 순서는 무의미합니다.

하지만 배열 비구조화에서 왼쪽 변수의 이름은 임의로 결정할 수 있지만, 객체 비구조화에서는 기존 속성명을 그대로 사용해야 합니다.

```js
// 코드 14 - 객체 비구조화에서는 순서는 상관없고 속성명이 중요하다
const obj = { age: 21, name: 'mike' };
const { name, age } = obj;
console.log(age); // 21
console.log(name); // mike

const { a, b } = obj;
console.log(a); // undefined
console.log(b); // undefined
```

존재하지 않는 속성명을 사용하면 `undefined`가 할당 됩니다.

객체 비구조화에서는 속성명과 다른 이름으로 변수를 생성할 수 있습니다. 이는 중복된 변수명을 피하거나 좀 더 구체적인 변수명을 만들 때 좋습니다.

```js
// 코드 15 - 객체 비구조화에서 별칭 사용하기
const obj = { age: 21, name: 'mike' };
const { age: theAge, name } = obj;
console.log(theAge); // 21
console.log(age); // 참조 에러
```

속셩명 `age`의 값을 `age`변수에는 할당되지 않고 `theAge`변수에 할당합니다.

객체 비구조화에서도 기본값을 정의할 수 있습니다. 배열 비구조하처럼 속성값이 `undefined`인 경우에는 기본값이 들어갑니다.

```js
// 코드 16 - 객체 비구조화에서의 기본값
const obj = { age: undefined, name: null, grade: 'A' };
const { age = 0, name = 'noName', grade = 'F' } = obj;
console.log(age); // 0
console.log(name); // null
console.log(grade); // A
```

`age`는 `undefined`이므로 기본값 0이 들어갑니다. 속성값이 `null`이면 기본값은 들어가지 않습니다.
