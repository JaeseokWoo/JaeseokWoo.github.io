---
date: '2022-11-05'
title: '변수 선언 - const, let'
categories: ['JavaScript']
summary: 'const, let'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# 변수 선언

## const, let

### const, let은 블록 스코프

var는 함수 스코프였지만 const, let은 **블록(block) 스코프**입니다. 블록 스코프는 대부분의 언어에서 사용하므로 개발자에게 익숙한 개념입니다.

`코드 7`와 같이 if문의 블록 안에서 정의된 변수는 if문을 벗어나면 참조할 수 없으며, 사용하려고 하면 에러가 발생합니다.

```jsx
// 코드 7 - 블록 스코프에서는 블록을 벗어나면 변수를 사용할 수 없습니다.

if (true) {
  const i = 0;
}
console.log(i); // 참조 에러
```

이번에는 블록 스코프에서 같은 이름의 변수를 정의하는 경우를 확인해보겠습니다.

```jsx
// 코드 8 - 블록 스코프에서 같은 이름을 갖는 변수의 사용 예시

let foo = 'bar1'; // (1)
console.log(foo); // bar1
if (true) {
  let foo = 'bar2';
  console.log(foo); // bar2
}
console.log(foo); // bar1
```

마지막 줄의 foo변수는 같은 블록에서 정의된 변수 (1)를 참조하므로 `bar1`을 출력합니다.

### const, let의 호이스팅

<mark>const와 let으로 정의된 변수 또한, 호이스팅이 됩니다.</mark> 하지만, const, let은 변수를 정의하기 전에 사용하려고 하면 참조 에러가 발생합니다.

```jsx
// 코드 9 - const, let 변수는 변수가 정의된 시점보다 먼저 변수를 사용할 수 없습니다.

console.log(foo); // 참조 에러
const foo = 1;
```

실행 결과를 통해 const, let으로 정의된 변수는 호이스팅이 되지 않는다고 생각하지만, const, let으로 정의된 변수도 호이스팅이 일어납니다.
다만, 변수가 정의된 위치와 호이스팅된 위치 사이에서 변수를 사용하려고 하면 에러가 발생하고, 이 구간을 **임시적 사각지대(temporal dead zone)** 라고 부릅니다. <mark>var로 정의된 변수에는 임시적 사각지대가 없기 때문에 참조 에러가 발생하지 않는 것입니다.</mark>

### const는 변수를 재할당 불가능하게 만듭니다.

const로 정의된 변수는 재할당이 불가능합니다. 반대로, let, var로 정의된 변수는 재할당할 수 있습니다. 재할당 불가능한 변수는 프로그램의 복잡도를 상당히 낮춰주기 때문에 되도록이면 재할당이 불가능한 변수를 사용하는게 좋습니다.

```jsx
// 코드 10 - const로 정의된 변수만 재할당 불가능 합니다.

var foo = 'a';
foo = 'b'; // 에러 없음
let value = 'a';
value = 'b'; // 에러 없음
const bar = 'a';
bar = 'b'; // 에러 발생
```

다만, const로 정의된 객체의 내부 속성값은 수정 가능하다는 점을 주의해야 합니다.

```jsx
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

객체의 내부 속성값도 수정 불가능하게 만들고 싶다면 `immer`, `immutable.js` 등의 외부 패키지를 활용하는게 좋습니다. 이러한 외부 패키지는 객체를 수정하려고 할 때, 기존 객체는 변경하지 않고 새로운 객체를 생성합니다.

새로운 객체를 생성하는 편의 기능은 필요 없고 수정만 할 수 없도록 차단하고 싶다면, 자바스크립트 내장 함수를 이용하면 됩니다.

- [Object.preventExtensions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
- [Object.seal](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
- [Object.freeze](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
