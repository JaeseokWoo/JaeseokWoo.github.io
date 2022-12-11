---
date: '2022-12-11'
title: 'optional chaining'
categories: ['JavaScript']
summary: 'optional chaining'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# optional chaining

자바스크립트에서는 객체의 속성에 접근할 때 점을 찍어서 접근을 합니다. 하지만 객체가 존재하지 않는다면 런타임에서 에러가 발생할 겁니다.

```jsx
const person = null;
const name = person.name; // 에러 발생
```

에러가 발생하지 않도록 하기 위해서 앞에서 객체가 존재하는지 검사를 해주는 방법도 있습니다.

```jsx
const person = null;
const name = person && person.name;
```

하지만 optional chaining을 사용하면 간단하게 작성할 수 있습니다.

```jsx
const name = person?.name;
// === const name = person?.['name'];
```

객체의 속성에 접근하기 전에 물음표를 입력하면 됩니다. 이렇게 하면 자동으로 객체를 검사해줍니다.

앞에서 살펴봤던 optional chaining 코드는 다음과 같다고 볼 수 있습니다.

```jsx
// const name = person?.name;
const name = person === null || person === undefined ? undefined : person.name;
```

`null` 또는 `undefined`이면 `undefined`가 되고 그렇지 않다면 의도했던 코드가 사용이 되는 겁니다.

함수를 호출할 때도 optional chaining을 사용할 수 있습니다. 함수를 호출하기 전에 물음표와 점을 입력하면 됩니다.

```jsx
const person = {
  getName: () => 'abc',
};
const name = person.getName?.();
const age = person.getAge?.();

console.log(name); // abc
console.log(age); // undefined
```

마찬가지로 함수가 없다면 `undefined`가 사용이 됩니다.

함수를 매개변수로 받아오고 그 매개변수가 optional일 때도 optional chaining이 유용하게 사용될 수 있습니다.

```jsx
function loadData(onComplete) {
  console.log('loading...');
  onComplete?.();
}
loadData();
```

optional chaining은 배열의 아이템에 접근할 때도 사용될 수 있습니다.

```jsx
const person = { friends: null, mother: null };
const firstFriend = person.friends?.[0];
```

optional chaining은 검사하는 단계가 많을수록 유용합니다.

```jsx
const name1 =
  person &&
  person.friends &&
  person.friends[0] &&
  person.friends[0].mother &&
  person.friends[0].mother.name;

const name 2 = person?.friend?.[0]?.mother?.name;
```

optional chaining을 사용하지 않으면 작성해야될 코드가 많은데 optional chaining을 사용하면 간단하게 표현할 수가 있고 생산성과 가독성 측면에서 분명한 차이가 있습니다.

optional chaining은 nullish coalescing과 함께 사용하기 좋습니다.

```jsx
const person = {};
const name = person?.friends?.[0]?.mother?.name ?? 'default name';
```

optional chaining에서 중간에 `undefined`가 됐을 때 nullish coalescing으로 기본값이 사용되게 하기 좋습니다.
