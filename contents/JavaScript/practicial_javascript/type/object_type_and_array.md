---
date: '2022-11-27'
title: 'object 타입, array'
categories: ['JavaScript']
summary: 'object 타입, array'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# object 타입, array

자바스크립트의 객체에 대해서 알아보겠습니다.

```jsx
const obj = {
  age: 21,
  name: 'mike',
};
const obj2 = new Object({
  age: 21,
  name: 'mike',
});
console.log(Object.keys(obj)); // [ 'age', 'name' ]
console.log(Object.values(obj)); // [ 21, 'mike' ]
console.log(Object.entries(obj)); // [ [ 'age', 21 ], [ 'name', 'mike' ] ]
```

객체를 생성할 때는 중괄호를 이용해서 생성할 수 있습니다. 다른 표현으로는 `new` 키워드와 `Object` 생성자를 이용하는 방법이 있습니다.

`Object` 생성자를 통해서 제공되는 함수를 알아보겠습니다. `keys`는 입력된 객체의 모든 키를 배열로 반환을 해줍니다. `values`는 모든 값을 배열로 반환을 해줍니다. `entries`는 key와 value를 튜플 형식으로 만들어서 배열로 반환을 해줍니다.

객체의 속성을 추가하거나 수정하거나 삭제하는 방법에 대해서 알아보겠습니다.

```jsx
const obj = {
  age: 21,
  name: 'mike',
};
obj.city = 'seoul';
obj.age = 30;
obj['name'] = 'Mike';
console.log(obj); // { age: 30, name: 'Mike', city: 'seoul' }

delete obj.city;
console.log(obj); // { age: 30, name: 'Mike' }

delete obj['name'];
console.log(obj); // { age: 30 }
```

기존에 없던 속성 이름에 어떤 값을 할당하면 속성이 추가가 됩니다. 그리고 기존 속성에 할당을 하면 기존 속성의 값이 변경됩니다. `delete` 키워드를 이용하면 속성을 삭제할 수 있습니다.

이번에는 `array`에 대해서 알아보겠습니다.

```jsx
const arr = [1, 2, 3];
const arr2 = new Array(1, 2, 3);
console.log(typeof arr === 'object'); // true
console.log(Object.keys(arr)); // [ '0', '1', '2' ]
console.log(Object.values(arr)); // [ 1, 2, 3 ]
```

`array`는 대괄호를 이용해서 생성할 수 있습니다. 다른 표현으로는 `Array` 생성자를 사용할 수 있습니다. 한 가지 짚고 넘어갈 점은 **`array`의 타입은 `object`입니다.** 그래서 `Object` 생성자의 함수를 이용할 수 있습니다.

배열의 아이템으로 루프를 도는 방법이 크게 두 가지가 있습니다. `forEach` 메서드를 사용하는 방법이 있고 `for of` 문을 사용하는 방법이 있습니다.

```jsx
const arr = [1, 2, 3];

arr.forEach(item => console.log(item));

for (const item of arr) {
  console.log(item);
}
```

`array`에는 자체적으로 갖고 있는 유용한 메서드가 많이 있습니다.

```jsx
const arr = [1, 2, 3];

console.log(arr.map(item => item + 1)); // [ 2, 3, 4 ]
console.log(arr.filter(item => item >= 2)); // [ 2, 3 ]
console.log(arr.reduce((acc, item) => acc + item, 0)); // 6

console.log(arr.some(item => item === 2)); // true
console.log(arr.every(item => item === 2)); // false
console.log(arr.includes(2)); // true
console.log(arr.find(item => item % 2 === 1)); // 1
console.log(arr.findIndex(item => item % 2 === 1)); // 0
```

`map`은 연산에 따라서 각 아이템의 값을 변경해서 새로운 배열로 만들어 줍니다. `filter`는 조건을 만족하는 아이템만 새로운 배열로 만들어 줍니다. `reduce`는 두 번째 매개변수에 있는 값에서 출발해서 모든 아이템에 대해서 첫 번째 매개변수의 연산을 수행해서 최종적으로 하나의 값을 반환해 줍니다. `acc`에는계속해서 누적된 값이 들어옵니다. `some`은 아이템 중 하나라도 조건에 만족하면 `true`를 반환합니다. `every`는 모든 아이템에 대해서 조건을 만족해야 `true`를 반환합니다. `includes`는 배열이 특정 아이템을 포함하고 있는지 여부를 반환합니다. `find`는 조건을 만족하는 첫 번째 아이템을 반환해 줍니다. `findIndex`는 `find`와 비슷하지만 인덱스를 반환해줍니다. 위의 메서드(`forEach`, `map`, `filter`, `reduce`, `some`, `every`, `includes`, `find`, `findIndex`)는 기존 배열은 건드리지 않습니다.

이번에는 `array`에 아이템을 추가하고 삭제, 그리고 수정하는 방법에 대해서 알아보겠습니다.

```jsx
const arr = [1, 2, 3];

arr.push(4);
console.log(arr.pop()); // 4
console.log(arr); // [1, 2, 3]

arr.splice(1, 1);
console.log(arr); // [1, 3]
arr.splice(1, 0, 10, 20, 30);
console.log(arr); // [ 1, 10, 20, 30, 3 ]
arr.splice(1, 3, 40, 50);
console.log(arr); // [ 1, 40, 50, 3]
```

`push`는 뒤에 아이템 추가하는 거고 `pop`은 뒤에 있는 아이템을 삭제해서 그 아이템을 반환해줍니다. `splice` 메서드를 이용하면 아이템을 삭제하고 추가하는 것을 동시에 할 수 있습니다. 첫 번째 매개변수는 인덱스를 가리킵니다. 그리고 두 번째 매개변수는 삭제할 개수를 나타냅니다. 그리고 세 번째 매개변수 부터는 추가할 아이템을 나열하는 겁니다.

정렬을 하기 위해서 `sort` 메서드를 이용할 수 있습니다. **정렬은 [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability)가 아닐 수 있습니다.**

```jsx
const arr = [1, 2, 3, 21, 20];

arr.sort();
console.log(arr); // [ 1, 2, 20, 21, 3 ]
arr.sort((a, b) => (a % 10) - (b % 10));
console.log(arr); // [ 20, 1, 21, 2, 3 ]
```

**매개변수에 아무것도 입력하지 않으면 문자열의 유니코드를 따라 정렬됩니다.** `sort`는 새로운 배열을 생성하지는 않고 기존 배열을 수정합니다. 정렬 조건을 변경하기 위해서 매개변수로 함수를 입력할 수 있습니다. **함수에서 음수를 반환하면 `a`를 `b`보다 낮은 순으로 정렬하겠다는 의미**가 되고 **`0`을 반환하면 `a`와 `b` 서로에 대해 정렬 순서를 변경하지 않겠다는 의미**입니다. **양수를 반환하면 `b`를 `a`보다 낮은 순서로 정렬 하겠다는 의미**가 됩니다.
