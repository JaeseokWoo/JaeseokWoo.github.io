---
date: '2023-01-06'
title: 'RxJS의 iif()'
categories: ['RxJS']
summary: 'RxJS의 iif()'
thumbnail: './images/rxjs_logo.png'
---

# RxJS의 iif() 사용 시 주의할 점

RxJS 라이브러리를 사용하며 프로젝트를 진행하면서 알게 된 iif() 함수의 사용에 주의할 점을 소개하려고 합니다.

<aside>
📚 이 포스트는 <a href="https://rxjs.dev/">RxJS</a>에 대한 기본적인 지식이 있다면, 쉽게 이해할 수 있습니다.

</aside>

<aside>
📝 먼저, RxJS의 iif()에 대해 간략히 설명하겠습니다.

RxJS의 iif() 함수는 조건부로 Observable을 생성하고 구독하는 함수입니다. if-else 문과 유사한 형태를 가지고 있어서, 첫 번째 매개변수로 전달된 조건식이 true인 경우 두 번째 매개변수로 전달된 Observable을 생성하고, false인 경우 세 번째 매개변수로 전달된 Observable을 생성합니다. 이를 통해 비동기 프로그래밍에서 조건 분기를 간단하게 처리할 수 있습니다.

</aside>

이제 RxJS의 iif()를 사용할 때 주의할 점을 살펴보겠습니다.

```jsx
const { of, mergeMap, iif } = require('rxjs');

const number$ = num => {
  console.log('number$:', num);
  return of(num);
};

const string$ = str => {
  console.log('string$:', str);
  return of(str);
};

of(1)
  .pipe(
    mergeMap(value =>
      iif(
        () => typeof value === 'number' && !Number.isNaN(value),
        number$(value),
        string$(value),
      ),
    ),
  )
  .subscribe({
    next: value => console.log(`next: ${value}`),
    error: error => console.log(`error: ${error}`),
    complete: () => console.log('complete'),
  });
```

위 코드를 실행하면 콘솔에는

```bash
number$: 1
string$: 1
next: 1
complete
```

의 순서로 출력됩니다. `mergeMap`의 인자인 `value`에 Number 타입인 1이 전달되면서, `iif()` 함수의 첫 번째 매개변수인 조건식이 실행되어 true로 평가가 됩니다. 따라서, `number$()` 함수를 호출하는 것이 예상되었지만, `string$()` 함수도 함께 호출되어 `"string$: 1"`이 출력됩니다. 이와 같이, `iif()` 함수의 첫 번째 매개변수에 전달된 조건식과 상관 없이 두 번째 매개변수로 전달된 Observable과 세 번째 매개변수로 전달된 Observable을 생성하기 전 모든 로직이 실행될 수 있으므로, 사용할 때, 주의가 필요하다는 것을 알 수 있습니다.

다음으로는 이러한 현상을 해결하기 위해, `iif()`함수 사용 시의 대안 방법에 대해서 소개해볼게요.

1. `iif()` 함수 대신 조건 연산자인 ternary operator(삼항 연산자)를 사용

```jsx
const { of } = require('rxjs');

const number$ = num => {
  console.log('number$:', num);
  return of(num);
};

const string$ = str => {
  console.log('string$:', str);
  return of(str);
};

of(1)
  .pipe(
    mergeMap(value =>
      typeof value === 'number' && !Number.isNaN(value)
        ? number$(value)
        : string$(value),
    ),
  )
  .subscribe({
    next: value => console.log(`next: ${value}`),
    error: error => console.log(`error: ${error}`),
    complete: () => console.log('complete'),
  });
```

위 코드를 실행하면 콘솔에는

```bash
number$: 1
next: 1
complete
```

의 순서로 출력됩니다. `iif()` 함수를 사용한 코드와는 달리, ternary operator를 사용한 코드에서는 첫 번째 매개변수로 전달된 조건식이 true이면 `number$()` 함수만 호출되고, false이면 `string$()` 함수만 호출됩니다. 따라서, 불필요한 함수 호출을 방지할 수 있습니다.

2. `iif()` 함수의 두 번째 매개변수와 세 번째 매개변수를 defer() 함수로 감싸기

```jsx
const { of, mergeMap, iif, defer } = require('rxjs');

const number$ = num => {
  console.log('number$:', num);
  return of(num);
};

const string$ = str => {
  console.log('string$:', str);
  return of(str);
};

of(1)
  .pipe(
    mergeMap(value =>
      iif(
        () => typeof value === 'number' && !Number.isNaN(value),
        defer(() => number$(value)),
        defer(() => string$(value)),
      ),
    ),
  )
  .subscribe({
    next: value => console.log(`next: ${value}`),
    error: error => console.log(`error: ${error}`),
    complete: () => console.log('complete'),
  });
```

위 코드를 실행하면 콘솔에는

```bash
number$: 1
next: 1
complete
```

의 순서로 출력됩니다.

## ✏️ 마무리 및 정리

RxJS의 **`iif()`** 함수를 사용하는 경우 발생할 수 있는 문제와 대안 방법에 대해 알아보았습니다. **`iif()`** 함수의 단점을 보완하기 위해 ternary operator와 `defer()` 함수를 활용할 수 있다는 것을 확인했습니다.
