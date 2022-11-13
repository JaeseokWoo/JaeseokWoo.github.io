---
date: '2022-11-13'
title: '8가지 기본 타입'
categories: ['JavaScript']
summary: '8가지 기본 타입'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# 8가지 기본 타입

자바스크립트에는 `number`, `bigint`, `string`, `boolean`, `object`, `symbol`, `undefined`, `null` 이렇게 8가지 기본 타입이 있습니다.

```jsx
// 코드1 자바스크립트의 8가지 기본 타입

const v1 = 12;
const v2 = 123456789123456789123456789n;
const v3 = 'ab';
const v4 = true;
console.log(typeof v1, typeof v2, typeof v3, typeof v4); // number bigint string boolean

const v5 = { name: 'abc' };
const v6 = Symbol('abc');
const v7 = undefined;
const v8 = null;
console.log(typeof v5, typeof v6, typeof v7, typeof v8); // object symbol undefined object
```

자바스크립트는 typeof 키워드를 사용하여 오른쪽에 있는 값의 타입을 문자열로 반환해 줍니다.

변수 v1은 숫자 타입이고, 변수 v2는 bigint라는 타입인데 큰 숫자를 나타낼 때 사용하는 타입입니다. 끝에 `n`을 입력하면 bigint가 됩니다. 변수 v3은 문자열 타입이고 변수 v4는 boolean 타입입니다.

변수 v5는 object 타입이고 변수 v6는 symbol 타입입니다. symbol 타입은 object에서 유니크한 속성 이름을 표현할 때 사용합니다. 변수 v7, v8은 각각 undefined와 null 타입입니다. 두 타입 모두 값이 없다는 것을 표현할 때 사용합니다. `undefined`는 값이 할당된 적이 없다는 것을 표현할 때 주로 사용합니다.

여기서 특이한 점은 `null`은 object 타입으로 나온다는 것입니다. object로 나오는 이유는 초기 자바스크립트의 `typeof`에 버그가 있었기 때문입니다. 이후에 버그를 수정하려는 시도가 있었지만 하위 호환성을 위해서 그냥 이 상태로 유지하기로 결정했다고 합니다.

8가지 기본 타입 외에 object로부터 파생된 function과 같은 타입도 있습니다.

```jsx
// 코드2 object로부터 파생된 function 타입

function f1() {}
console.log(typeof f1); // function

class MyClass {}
console.log(typeof MyClass); // function
```

자바스크립트의 **class는 함수를 기반으로 만들어졌기 때문에** class의 타입을 검사하면 function 타입으로 나옵니다.

`typeof`로 `null` 의 타입을 확인하면 object라고 나오는데, `Object`의 `toString`이라는 함수를 이용하면 null 타입을 구분할 수 있습니다.

```jsx
// 코드3 Object의 toString 함수로 세부적으로 타입 확인

console.log(Object.prototype.toString.call(null); // [object Null]

console.log(typeof []); // object
console.log(Object.prototype.toString.call([]); // [object Array]
```

배열 역시, typeof로 타입을 검사해보면 object라고 나오는데 `Object`의 `toString()`을 이용하면 배열도 구분해낼 수 있습니다.

`symbol`은 유일한 속성 이름을 만들 때 사용합니다.

```jsx
// 코드4 symbol 예시

const idSymbol = Symbol('id');
const obj = { id: 123 };
obj[idSymbol] = 456;
console.log(obj); // { id: 123, [Symbol(id)]: 456 }
```

`symbol`을 이용하면 이름 충돌 문제를 해결할 수 있습니다.

타입을 변환할 때는 `String`, `Number`, `BigInt`, `Boolean`과 같은 함수를 사용할 수 있습니다.

```jsx
// 코드5 타입을 변환해주는 함수

const v1 = String(123);
const v2 = String(new Date());
console.log(typeof v1, v1); // string 123
console.log(typeof v2, v2); // string

const v3 = Number('123');
const v4 = BigInt('123');
console.log(typeof v3, v3); // number 123
console.log(typeof v4, v4); // bigint 123n
```

new 키워드와 함께 사용하면 object로 만들어집니다.

```jsx
// 코드7 new 키워드와 함께 사용 시 object로 생성

console.log(typeof new Boolean(true)); // object
console.log(typeof new Number(1)); // object
console.log(typeof new String('abc')); // object

const s1 = new String('abc');
s1.id = 123;
console.log('value:', s1.valueOf()); // value: abc
console.log('id:', s1.id); // id: 123
```

new 키워드를 사용해서 문자열을 object로 만들고 id라는 속성값을 붙였습니다. 이렇게 추가로 속성을 붙여서 사용할게 아니라면 new 키워드를 사용할 필요는 없습니다.

boolean 타입으로 변환하는 코드를 알아보겠습니다.

```jsx
// 코드6 boolean 타입으로 변환하는 코드

const v1 = Boolean(123);
const v2 = Boolean(0);
console.log(typeof v1, v1); // boolean true
console.log(typeof v2, v2); // boolean false

const v3 = Boolean('abc');
const v4 = Boolean('');
console.log(typeof v3, v3); // boolean true
console.log(typeof v4, v4); // boolean false

const v11 = !!123;
const v12 = !!0;
const v13 = !!'abc';
const v14 = !!'';
console.log(typeof v11, v11); // boolean true
console.log(typeof v12, v12); // boolean false
console.log(typeof v13, v13); // boolean true
console.log(typeof v14, v14); // boolean false
```

문자열에서는 **문자열의 길이가 0보다 크면 `true`가 됩니다. 빈 문자열은 `false`가 됩니다.** Boolean이라는 함수를 사용하지 않고 `!!` (느낌표 두 개)로 표현하는 것도 똑같이 boolean으로 변환하는 효과가 있습니다. `!` (느낌표)는 Logical NOT을 의미합니다. NOT을 두 번 적용했기 때문에 원래 자기 자신의 boolean 값이 나오게됩니다.

자바스크립트에서 두 값이 같은지 비교하는 방법은 두 가지가 있습니다.

```jsx
// 코드8 값을 비교하는 두 가지 방법(=== vs ==)

console.log(123 === 123); // true
console.log('123' === '123'); // true
console.log('123' === 123); // false
console.log(0 === false); // false
console.log(123 === true); // false

console.log(123 == 123); // true
console.log('123' == '123'); // true
console.log('123' == 123); // true
console.log(0 == false); // true
console.log(123 == true); // false
```

등호를 세 개 사용하는 방식(`===`)과 두 개를 사용하는 방식(`==`)이 있습니다. **`===`를 사용하면 두 값의 타입과 값이 모두 같은지 검사를 합니다.** 하지만 **`==`를 사용했을 때는 두 값의 타입이 다르면 타입을 변환하면서까지 비교하려고 합니다.** `==`를 사용했을 때는 동작하는 로직이 다소 복잡하기 때문에 `===`를 사용할 것을 추천드립니다.
