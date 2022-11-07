---
date: '2022-11-08'
title: '자바스크립트의 객체와 배열'
categories: ['JavaScript']
summary: 'object, array'
thumbnail: './javascript-var-let-const.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

## 객체와 배열을 간편하게 생성하고 수정하기

### 단축 속성명

단축 속성명(shorthand property names)은 객체 리터럴 코드를 간편하게 작성할 목적으로 만들어진 문법입니다.

속성명을 사용하면 `코드 1`과 같이 간편하게 새로운 객체를 만들 수 있습니다.

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
