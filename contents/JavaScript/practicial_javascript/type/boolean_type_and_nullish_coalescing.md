---
date: '2022-11-20'
title: 'boolean 타입, nullish coalescing'
categories: ['JavaScript']
summary: 'boolean 타입, nullish coalescing1'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# boolean 타입, nullish coalescing

자바스크립트에서 Logical AND는 `&&`를 사용하면 되고 Logical OR는 `||`를 사용하면 됩니다.

```jsx
const c1 = true;
const c2 = false;
if (c1 && c2) {
  console.log('c1 && c2');
}
if (c1 || c2) {
  console.log('c1 || c2'); // c1 || c2
}
```

논리 연산에는 숫자나 문자열을 사용할 수도 있습니다. 숫자에서는 `0`이나 `NaN`이 `false`가 되고 그 외에는 `true`입니다. 문자열에서는 빈 문자열일 때만 `false` 이고 나머지는 `true`가 됩니다.

```jsx
const c1 = 123;
const c2 = 'abc';
if (c1 && c2) {
  console.log('c1 &&c2'); // c1 && c2
}
if (c1 || c2) {
  console.log('c1 || c2'); // c1 || c2
}
if (c1 && c2 && 0) {
  console.log('c1 && c2 && 0');
}
if (c1 && c2 && NaN) {
  console.log('c1 && c2 && NaN');
}
if (c1 && c2 && '') {
  console.log(`c1 && c2 && ''`);
}
```

**논리 연산자의 결과값은 마지막으로 평가된 값이 됩니다.**

```jsx
const c1 = 123;
const c2 = 'abc';
const v1 = c1 && c2;
const v2 = c1 && c2 && 0;
const v3 = c1 && 0 && c2;
console.log({ v1, v2, v3 }); // { v1: 'abc', v2: 0, v3: 0 }

const v4 = c1 || c2;
const v5 = '' || c2;
console.log({ v4, v5 }); // { v4: 123, v5: 'abc' }

const v6 = !!(c1 && 0 && c2);
const v7 = !!(c1 || c2);
console.log({ v6, v7 }); // { v6: false, v7: true }
```

AND 연산자에서는 앞에 있는 값이 `true`가 돼야 뒤에 있는 코드가 평가가 되기 때문에 if문 처럼 사용할 수도 있습니다.

```jsx
const c1 = 123;
const c2 = 0;
c1 && console.log('log1'); // log1
c2 && console.log('log2');
```

if문이 더 직관적이라서 if문을 선호하시는 분들도 있지만 작성하기 간편하다는 이유로 이 방식을 선호하시는 분들도 있습니다.

OR 연산자는 기본값을 입력하는 방법으로도 사용이 됩니다. OR 연산자 뒤에 기본값을 입력하는 것입니다.

```jsx
const price = 0;
const name = '';

const price2 = price || 1000;
const name2 = name || '이름을 입력해주세요';
console.log({ price2, name2 }); // {price2: 1000, name2: '이름을 입력해주세요'}
```

숫자에서는 `0` 또는 `NaN`이면 뒤에 있는 기본값이 사용이 되고, 문자열에서는 빈 문자열이면 기본값이 사용이 됩니다.

기본값을 입력하는 문법으로 nullish coalescing이라고 하는 문법이 있습니다. `??`기호와 뒤에는 기본값을 입력하면 됩니다.

```jsx
// nullish coalescing
const person = {};

const name = person.name ?? 'unknown';
// ==> const name = person.name === undefined || person.name === null ? 'unknown' : person.name;
console.log(name); // unknown
```

**nullish coalescing은 앞에 있는 값이 `undefined`이거나 `null`이면 기본값이 사용되는 것이고 그렇지 않으면 원래 값이 사용됩니다.**

nullish coalescing은 OR 연산자와 비슷하지만 한 가지 다른 점은 `undefined`와 `null` 만 확인한다는 것입니다. 그래서 상황에 따라 빈 문자열이나 `0`을 값으로 인정한다면 nullish coalescing을 사용하면 되고 아니라면 OR 연산자를 사용하면 됩니다.

```jsx
const product = { desc: '', price: 0 };

const descInput1 = product.desc ?? '상품 설명을 입력하세요';
const priceInput1 = product.price ?? 1000;
console.log({ descInput1, priceInput1 }); // { descInput1: '', priceInput1: 0 }

const descInput2 = product.desc || '상품 설명을 입력하세요';
const priceInput2 = product.price || 1000;
console.log({ descInput2, priceInput2 }); // { descInput2: '상품 설명을 입력하세요', priceInput2: 1000 }
```

nullish coalescing을 논리 연산자와 함께 사용할 때는 앞부분을 모두 괄호로 묶어줘야 합니다. 괄호를 묶지 않으면 `SyntaxError`가 발생합니다.

```jsx
const name = '';
const title = '';

const text1 = (name || title) ?? 'foo';
console.log(text1); // ''
const text2 = name || title ?? 'foo'; // SyntaxError 발생
console.log(text2);
```

논리 연산자와 nullish coalescing의 기본값으로 함수 호출을 넣으면 필요한 경우에만 호출이 됩니다.

```jsx
const name1 = 'mike';
const name2 = '';

function getDefaultName() {
  console.log('called getDefaultName');
  return 'default name';
}

console.log(name1 ?? getDefaultName()); // mike
console.log(name1 || getDefaultName()); // mike

console.log(name2 ?? getDefaultName()); // ''
console.log(name2 || getDefaultName());
/**
called getDefaultName
default name
 */
```
