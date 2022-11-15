---
date: '2022-11-13'
title: 'number 타입'
categories: ['JavaScript']
summary: 'number 타입'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# number 타입

자바스크립트에서 문자열로부터 숫자를 파싱하는 방법으로 `parseInt`와 `parseFloat`가 있습니다.

```jsx
console.log(Number.parseInt('123')); // 123
console.log(Number.parseInt('123.456')); // 123
console.log(Number.parseInt('123abc')); // 123

console.log(Number.parseFloat('123')); // 123
console.log(Number.parseFloat('123.456')); // 123.456
console.log(Number.parseFloat('123abc')); // 123
console.log(Number.parseFloat('123.456.789')); // 123.456
```

`parseInt`는 정수를 파싱하는 것이기 때문에 `.`을 만나거나 숫자가 아닌 문자열을 만나면 파싱을 끝냅니다. `parseFloat`은 실수를 파싱합니다. 실수이기 때문에 `.`이 있는 부분도 파싱을 합니다. `parseInt`와 마찬가지로 문자열을 만났을때는 파싱을 끝냅니다.

숫자가 전혀 없는 문자열을 파싱할 때는 `NaN`이 출력이됩니다.

```jsx
const v = Number.parseInt('abc');
console.log(v); // NaN

console.log(v, Number.isNaN(v)); // NaN true
console.log(123, Number.isNaN(123)); // 123 false
```

`NaN`은 Not A Number의 약자이고, 말 그대로 숫자가 아니라는 뜻입니다. 그리고 `NaN`을 검사해주는 `isNaN`함수도 있습니다.

많은 프로그래밍 언어에서 `0`으로 나누는 경우에는 에러가 나지만, 자바스크립트에서는 에러가 나지 않고 `Infinity`라는 값이 출력됩니다.

```jsx
const v = 1 / 0;
console.log(v); // Infinity

console.log('Infinity', v === Infinity); // Infinity true
console.log('Number.isFinite', Number.isFinite(v)); // Number.isFinite false
```

`Infinity`는 무한이라는 뜻이고 자바스크립트에서는 무한대를 표현할 수가 있습니다. **`Infinity`는 전역변수로 노출되어있으며**, 값이 유한하지 검사하는 `isFinite`함수도 있습니다.

**자바스크립트의 number는 무조건 64비트 부동소수점 방식을 사용합니다.** 자바나 C언어에서 정수와 실수를 구분하는 것과 비교되는 부분이기도 합니다. 자바스크립트의 number는 단일 형태라서 사용하기 편하다는 장점이 있지만 메모리 최적화에는 불리하다는 단점도 있습니다. (32비트만 사용하고 싶어도 무조건 64비트를 사용) (메모리 최적화를 위해서는 [`ArrayBuffer`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)라는 것을 사용할 수 있습니다.)

```jsx
// 자바스크립트 number => 64 bit 부동소수점(floating point)
//   부호(sign) 1 bit, 지수부(exponent) 11 bits, 가수부(fraction) 52 bits
//   (-1)^부호 * 가수부 * 2^지수부
//
// but, 자바스크립트는 복잡한 인코딩 방식을 사용하여 53 bits precision을 갖습니다.
// 자세한 내용은 링크 참조 https://en.wikipedia.org/wiki/Double-precision_floating-point_format

// -(2^53 - 1) ~ (2^53 - 1)
// 9007199254740991, 약 16자리
console.log(Math.pow(2, 53) - 1); // 9007199254740991
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991

console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
```

지수부는 숫자의 크기를 점점 커지게 만들어 주고 가수부는 숫자의 정확도를 담당합니다. 이러한 정확도를 영어로 precision이라고 합니다.

부동 소수점 방식은 가수부가 52비트라고 했기 때문에 자바스크립트도 52비트의 precision을 갖는다고 생각할 수 있겠지만 사실 **자바스크립트에서는 좀 더 복잡한 인코딩 방식을 사용하여 53비트의 precision을 갖습니다.** 자세한 내용은 [링크](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)에서 확인하실 수 있습니다.

그래서 자바스크립트에서는 `-(2^53 - 1) ~ (2^53 - 1)` 범위에 있는 정수를 안전한 정수라고 표현합니다. 최소는 `MIN_SAFE_INTEGER`, 최대는 `MAX_SAFE_INTEGER` 입니다.

물론 자바스크립트에서는 `MAX_VALUE`와 같이 커다란 숫자를 표현할 수 있습니다. 하지만 이런 커다란 숫자를 가지고 연산을 했을 때는 그 안전성을 보장할 수가 없습니다. 하지만 `-(2^53 - 1) ~ (2^53 - 1)` 범위에 있는 숫자를 가지고 연산을 한다면 비교적 안전하다고 생각할 수 있습니다.

`isSafeInteger`라는 함수로 숫자가 안전한지 검사할 수 있습니다.

```jsx
// Number.MAX_SAFE_INTEGER = 9007199254740991
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false
console.log(9007199254740995 - 10); // (1) 9007199254740986
console.log(9007199254740995n - 10n); // 9007199254740985n

const a = 9007199254740995;
const b = 10;
const result = a - b;
console.log(
  Number.isSafeInteger(a),
  Number.isSafeInteger(b),
  Number.isSafeInteger(result),
); // false true true
console.log('9007199254740995 - 10 =', result); // 9007199254740995 - 10 = 9007199254740986

const a = 9007199254740991;
const b = 10;
const result = a - b;
console.log(
  Number.isSafeInteger(a),
  Number.isSafeInteger(b),
  Number.isSafeInteger(result),
); // true true true
console.log('9007199254740991 - 10 =', result); // 9007199254740991 - 10 = 9007199254740981

const a = 9007199254740991;
const b = 10;
const result = a + b;
console.log(
  Number.isSafeInteger(a),
  Number.isSafeInteger(b),
  Number.isSafeInteger(result),
); // true true false
console.log('9007199254740991 + 10 =', result); // 9007199254740991 + 10 = 9007199254741000
```

(1)의 `9007199254740995`는 안전하지 않은 숫자이고 이 안전하지 않은 숫자를 연산에 사용했을 때는 연산 결과가 정확하지 않을 수 있습니다. `9007199254740995`에서 `10`을 빼기 때문에 결과는`MAX_SAFE_INTEGER`보다 작아서 안전하지 않겠냐는 생각을 할 수 있습니다. 하지만 실제 결과는 `9007199254740986`로 예상했던 `9007199254740985`가 나오지 않습니다. 이렇게 안전하지 않은 숫자를 연산에 사용했을 때는 그 결과값의 정확성을 보장할 수 없습니다. 이것을 해결하는 방법은 끝에 `n`을 입력해서 `bigint`를 사용하는 겁니다. 또 다른 방법은 연산에 사용된 피연산자와 그 결과를 모두 검사하여 모두 안전한 숫자라고 나오면 그 연산 결과를 믿을 수 있습니다.

number가 부동소수점 방식을 사용하기 때문에 다음과 같은 결과가 나옵니다.

```jsx
console.log(0.1 + 0.2 === 0.3); // false
console.log(0.1 + 0.2); // 0.30000000000000004
```

부동소수점에서는 precision이 정해져 있기 때문에 정확도에 한계가 있습니다. 이는 자바스크립트의 문제는 아니고 다른 언어에서도 마찬가지 문제가 있습니다.

실수 연산에서는 `EPSILON`이라는 값을 이용해서 비슷한 값인지 검사할 수 있습니다.

```jsx
console.log(Number.EPSILON); // 2.220446049250313e-16

function isSimilar(x, y) {
  return Math.abs(x - y) < Number.EPSILON;
}

console.log(isSimilar(0.1 + 0.2, 0.3)); // true
```

`Number.EPSILON`은 매우 작은 숫자이고 53비트 precision을 고려해서 이 값보다 작은 값은 “무시 할만하다“라고 생각할 수 있습니다. `isSimilar`함수는 두 값을 입력받아서 그 차이가 `EPSILON`보다 작다면 “두 값은 비슷하다”라고 생각할 수 있습니다.

`Math`객체에는 수학 관련된 여러 함수가 있습니다.

```jsx
console.log(Math.random());
console.log(Math.max(30, 10, 55));
console.log(Math.pow(5, 3));

function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
console.log(getRandomInt(0, 10));
console.log(getRandomInt(0, 10));
console.log(getRandomInt(0, 10));
```

`random`이라는 함수는 0보다 크고 1보다 작은 숫자를(0 ≤ x < 1) 랜덤하게 반환해 줍니다. `max`는 여러 개의 숫자를 입력 받아서 이 중에서 가장 큰 숫자를 반환해 줍니다. 그리고 `pow`라는 함수는 첫 번째 매개변수의 숫자를 베이스(base)로 해서 두 번째 매개변수의 숫자만큼의 승수를 적용해 줍니다. `Math.floor`라는 함수는 소수점을 버리는 함수이고 `random`함수와 같이 이용해서 `getRandomInt`함수를 구현할 수 있습니다.

이 밖에 `Math`객체에는 수학과 관련된 다양한 함수가 존재합니다.
