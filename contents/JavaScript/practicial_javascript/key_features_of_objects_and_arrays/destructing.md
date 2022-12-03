---
date: '2022-12-04'
title: 'destructuring'
categories: ['JavaScript']
summary: 'destructuring'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# destructuring

## 객체와 배열의 속성값을 간편하게 가져오기

### 배열 비구조화

**배열 비구조화(array destructuring)** 는 배열의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다.

```jsx
// 코드 8 - 배열 비구조화를 사용한 간단한 코드

const arr = [1, 2];
const [a, b] = arr;
console.log(a); // 1
console.log(b); // 2
```

배열 비구조화 시 기본값을 정의할 수 있습니다. 배열의 속성값이 `undefined`라면 정의된 기본값이 할당되고, 그렇지 않다면 원래의 속성값이 할당됩니다.

```jsx
// 코드 - 9 배열 비구조화에서의 기본값

const arr = [1];
const [a = 10, b = 20] = arr;
console.log(a); // 1
console.log(b); // 20
```

첫 번째 변수의 속성값은 존재하기 때문에 기본값 `10`은 사용되지 않고 속성값이 그대로 할당됩니다. 두 번째 변수의 속성값은 `undefined`이므로 기본값 `20`이 할당됩니다.

배열 비구조화를 사용하면 두 변수의 값을 쉽게 교환할 수 있습니다.

```jsx
// 코드 - 10 배열 비구조화를 이용해서 두 변수의 값을 교환하기

let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

일반적으로 두 변수가 값을 교환하기 위해서는 제 3의 변수를 이용하는게 일반적입니다. 하지만 배열 비구조화를 사용하면 제 3의 변수없이 코드를 구현할 수 있습니다.

배열에서 일부 속성값을 무시하고 진행하고 싶다면 건너뛰는 개수 만큼 쉼표를 입력하면 됩니다.

```jsx
// 코드 11 - 쉼표를 이용해서 일부 속성값을 건너뛰기

const arr = [1, 2, 3];
const [a, , c] = arr;
console.log(a); // 1
console.log(b); // 3
```

첫 번째 속성값은 변수 a에 할당되고, 두 번째 속성값은 건너뛰고 세 번째 속성값이 변수 c에 할당됩니다.

쉼표 개수만큼을 제외한 나머지를 새로운 배열로 만들 수도 있습니다.

```jsx
// 코드 12 - 나머지 값을 별도의 배열로 만들기

const arr = [1, 2, 3];
const [first, ...rest1] = arr;
console.log(rest1); // [2, 3]
const [a, b, c, ...rest2] = arr;
console.log(rest2); // []
```

배열 비구조화 시 마지막에 ...와 함께 변수명을 입력하면 나머지 모든 속성값이 새로운 배열로 만들어집니다. 나머지 속성값이 존재하지 않으면 빈 배열이 만들어집니다.

### 객체 비구조화

**객체 비구조화(object destructuring)** 는 객체의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다.

```jsx
// 코드 13 - 객체 비구조화 간단한 예

const obj = { age: 21, name: 'mike' };
const { age, name } = obj;
console.log(age); // 21
console.log(name); // mike
```

객체 비구조화에서는 중괄호를 사용합니다. 배열 비구조화에서는 배열의 순서가 중요했지만 객체 비구조화에서는 순서는 무의미합니다. 하지만 배열 비구조화에서 왼쪽 변수의 이름은 임의로 결정할 수 있지만, 객체 비구조화에서는 기존 속성명을 그대로 사용해야 합니다.

```jsx
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

```jsx
// 코드 15 - 객체 비구조화에서 별칭 사용하기

const obj = { age: 21, name: 'mike' };
const { age: theAge, name } = obj;
console.log(theAge); // 21
console.log(age); // 참조 에러
```

속셩명 age의 값을 age변수에는 할당되지 않고 theAge변수에 할당합니다.

객체 비구조화에서도 기본값을 정의할 수 있습니다. 배열 비구조화처럼 속성값이 `undefined`인 경우에는 기본값이 들어갑니다.

```jsx
// 코드 16 - 객체 비구조화에서의 기본값

const obj = { age: undefined, name: null, grade: 'A' };
const { age = 0, name = 'noName', grade = 'F' } = obj;
console.log(age); // 0
console.log(name); // null
console.log(grade); // A
```

age는 `undefined`이므로 기본값 `0`이 들어갑니다. 속성값이 `null`이면 기본값은 들어가지 않습니다.

기본값을 정의하면서 별칭을 함께 사용할 수 있습니다.

```jsx
// 코드 17 - 기본값과 별칭 동시에 사용하기

const obj = { age: undefined, name: 'mike' };
const { age: theAge = 0, name } = obj;
console.log(theAge); // 0
```

기본값으로 함수의 반환값을 넣을 수 있습니다.

```jsx
// 코드 18 - 함수를 이용한 기본값

function getDefaultAge() {
  console.log('hello');
  return 0;
}
const obj1 = { age1: 21, grade1: 'A' };
const { age1 = getDefaultAge(), grade1 } = obj1; // hello 출력되지 않음
console.log(age1); // 21

const obj2 = { age2: undefined, grade2: 'B' };
const { age2 = getDefaultAge(), grade2 } = obj2; // hello 출력
aconsole.log(age2); // 0
```

여기서 중요한 점은 기본값이 사용될 때만 함수가 호출된다는 점입니다.

객체 비구조화에서도 사용되지 않는 나머지 속성들을 별도의 객체로 생성할 수 있습니다.

```jsx
// 코드 19 - 객체 비구조화에서 나머지 속성들을 별도의 객체로 생성하기

const obj = { age: 21, name: 'mike', grade: 'A' };
const { age, ...rest } = obj;
console.log(rest); // { name: 'mike', grade: 'A' }
```

for 문에서 객체를 원소로 갖는 배열을 순회할 때 객체 비구조화를 사용하면 편리합니다.

```jsx
// 코드 20 - for 문에서 객체 비구조화를 활용한 예

const people = [
  { age: 21, name: 'mike' },
  { age: 51, name: 'sara' },
];

for (const { age, name } of people) {
  // ...
}
```

### 비구조화 심화

비구조화는 객체와 배열이 중첩되어 있을 때도 사용할 수 있습니다.

```jsx
// 코드 21 - 중첩된 객체의 비구조화 사용 예

const obj = { name: 'mike', mother: { name: 'sara' } };
const {
  name,
  mother: { name: motherName }, // (1)
} = obj;
console.log(name); // mike
console.log(motherName); // sara
console.log(mother); // 참조 에러
```

(1) 세 개의 단어가 등장하지만, 비구조화의 결과로 motherName이라는 이름의 변수만 생성됩니다.

비구조화에서 기본값의 정의는 변수로 한정되지 않습니다.

```jsx
// 코드 22 - 기본값은 변수 단위가 아니라 패턴 단위로 적용됩니다.

const [{ prop: x } = { prop: 123 }] = []; // (1)
console.log(x); // 123
const [{ prop: y } = { prop: 123 }] = [{}]; // (2)
console.log(y); // undefined
```

(1) 코드에서 `{ prop: x }`는 배열의 첫 번째 원소를 가리키고, `{ prop: 123 }`은 그 기본값을 정의합니다. 따라서, 첫 번째 원소가 존재하지 않아서 기본값이 할당되고 결과적으로 변수 x에는 기본값에서 정의된 `123`이 들어갑니다.

(2) 배열의 첫 번째 원소가 존재하므로 기본값이 할당되지 않습니다. 그리고 첫 번째 원소에는 prop이라는 이름의 속성명이 존재하지 않으므로 `y`에는 `undefined`가 할당됩니다.

객체 비구조화에서도 계산된 속성명을 활용할 수 있습니다.

```jsx
// 코드 23 - 객체 비구조화에서 계산된 속성명 사용하기

const index = 1;
const { [`key${index}`]: valueOfTheIndex } = { key1: 123 };
console.log(valueOfTheIndex); // 123
```

객체 비구조화에서 계산된 속성명을 사용할 때에는 반드시 별칭을 입력해야합니다.

비구조화에서 별칭을 사용할 때 단순히 변수명만 입력할 수 있는 것은 아닙니다.

```jsx
// 코드 24 - 별칭을 이용해서 다른 객체와 배열의 속성값 할당

const obj = {};
const arr = [];
({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });
console.log(obj); // { prop: 123 }
console.log(arr); // [true]
```

객체 비구조화를 이용해서 `코드24`처럼 obj 객체의 prop이라는 속성과 배열의 첫 번째의 원소에 값을 할당할 수 있습니다.
