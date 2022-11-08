---
date: '2022-11-08'
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
