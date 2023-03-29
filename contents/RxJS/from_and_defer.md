---
date: '2022-09-13'
title: 'RxJS의 from()과 defer()'
categories: ['RxJS']
summary: 'RxJS의 from()과 defer()'
thumbnail: './images/rxjs_logo.png'
---

# RxJS의 from()과 defer()

프로젝트를 진행하면서, 외부 API를 호출할 때 RxJS 라이브러리의 defer() 함수와 from() 함수의 동작 방식이 다르다는 것을 알게 되었습니다. 이에 두 함수의 차이점을 직접 실행하면서 비교해보았습니다.

<aside>
📚 이 포스트는 [RxJS](https://rxjs.dev/)와 [JavaScript의 Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)에 대한 기본적인 지식이 있다면, 쉽게 이해할 수 있습니다.

</aside>

실험 내용은 reject되는 Promise를 defer() 함수와 from() 함수를 이용하여 Observable로 변환한 후, retry 연산자로 reject된 Promise를 어떻게 처리하는지 실험하였습니다.

<aside>
📝 실험에 앞서, RxJS의 from(), defer(),  retry()에 관해 간단하게 알아보겠습니다.

- from(): 옵저버블로 변환할 수 있을 만한 객체들을 옵저버블로 변환해줍니다.
- defer(): 구독하는 시점에 팩토리 함수를 호출해 생성한 옵저버블을 구독합니다.
- retry(): 에러가 발생했을 때 인자로 설정한 정숫값만큼 소스 옵저버블 구독을 재시도하는 연산자입니다.
</aside>

먼저 defer()로 생성된 Observable를 확인해보도록 하겠습니다.

```jsx
const { retry, defer, catchError } = require('rxjs');

defer(
  () =>
    new Promise((resolve, reject) => {
      console.log('Promise created!!');

      console.log('Promise rejected!');
      reject('reject');
    }),
)
  .pipe(
    catchError(error => {
      console.log(`${error} 발생`);
      throw error;
    }),
    retry(2),
  )
  .subscribe({
    next: value => console.log(`defer next: ${value}`),
    error: error => console.log(`defer error: ${error}`),
    complete: () => console.log('defer complete'),
  });
```

해당 소스 코드를 실행해보면

```bash
Promise created!!
Promise rejected!
reject 발생
Promise created!!
Promise rejected!
reject 발생
Promise created!!
Promise rejected!
reject 발생
defer error: reject
```

Promise가 reject되면, retry 연산자는 다시 구독을 하면서 새로운 Promise를 생성하여 동작하는 것을 알 수 있습니다.

이제 from()으로 생성된 Observable이 어떻게 동작하는지 확인해보겠습니다.

```jsx
const { retry, from, catchError } = require('rxjs');

from(
  new Promise((resolve, reject) => {
    console.log('Promise created!!');

    console.log('Promise rejected!');
    reject('reject');
  }),
)
  .pipe(
    catchError(error => {
      console.log(`${error} 발생`);
      throw error;
    }),
    retry(2),
  )
  .subscribe({
    next: value => console.log(`defer next: ${value}`),
    error: error => console.log(`defer error: ${error}`),
    complete: () => console.log('defer complete'),
  });
```

해당 소스 코드를 실행해보면

```bash
Promise created!!
Promise rejected!
reject 발생
reject 발생
reject 발생
defer error: reject
```

from()으로 생성된 Promise Observable은 reject로 인해 에러가 발생하면 retry 연산자를 사용하여 재구독합니다. 그러나 defer() 함수와 달리 새로운 Promise를 생성하는 것이 아니라 이미 실행이 완료된(rejected) Promise를 그대로 재구독하는 것을 알 수 있습니다.

위 실험 결과, 에러가 발생했을 때 retry 연산자를 통해 defer() 함수는 매번 새로운 Promise를 생성하여 구독하고, from() 함수는 이미 완료된 Promise를 그대로 재구독하는 것을 알 수 있습니다.

이러한 특성 때문에, from() 함수는 이미 실행 중이거나 완료한 프로미스를 옵저버블로 만들 때 적합하고 defer() 함수는 프로미스 실행 시점이 구독하는 시점이어야 할 때 적합하고 상황에 따라 defer()함수와 from() 함수 중 적절한 생성 함수를 사용해야합니다.
