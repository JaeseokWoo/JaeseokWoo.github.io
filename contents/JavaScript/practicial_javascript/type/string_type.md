---
date: '2022-11-19'
title: 'string 타입'
categories: ['JavaScript']
summary: 'string 타입'
thumbnail: '../images/practicial_javascript.jpeg'
---

이 포스트는 [이재승님의 실전 자바스크립트](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)의 강의와 책 내용을 정리하기 위해 작성된 글 입니다.

# string 타입

자바스크립트에서는 3가지 방식으로 문자열을 만들 수 있습니다.

```jsx
const s1 = 'abc';
const s2 = 'abcd';
const s3 = `abcde`;
console.log(s1.length, s2.length, s3.length); // 3 4 5
```

작은 따옴표나 큰 따옴표를 이용할 수 있고 백틱(backtick)을 이용할 수도 있습니다.

여러 개의 변수를 조합해서 하나의 문자열을 만드는 방법을 알아보겠습니다.

```jsx
const name = 'mike';
const age = 23;
const text1 = 'name: ' + name + ', age: ' + age;
const text2 = `name: ${name}, age: ${age}`;
console.log(text1); // name: mike, age: 23
console.log(text2); // name: mike, age: 23
```

따옴표를 사용하는 경우 중간에 `+`기호를 사용해서 문자열을 연결할 수 있습니다. 백틱을 사용하면 한 쌍의 백틱 안에서 `$`기호와 중괄호를 사용해서 변수를 입력할 수도 있습니다. 보통은 백틱을 사용하는 방식인 **템플릿 리터럴(template literals)**이 훨씬 더 작성하기도 편하고 가독성에도 좋습니다.

줄바꿈을 포함하는 문자열을 만드는 방식을 알아보겠습니다.

```jsx
const text1 = '할 일 목록\n# 운동하기\n# 요리하기';
console.log(text1);
/**
할 일 목록
# 운동하기
# 요리하기
 */

const text2 = `할 일 목록
# 운동하기
# 요리하기
`;
console.log(text2);
/**
할 일 목록
# 운동하기
# 요리하기
 */
```

따옴표를 이용할 때는 `\n`을 입력합니다. 백틱을 이용할 때는 `\n`을 입력할 필요없고 코드에 입력한대로 줄바꿈이 됩니다.

문자열에 있는 문자를 추출할 때는 대괄호를 이용해서 인덱스를 입력하면 됩니다.

```jsx
const s1 = 'abcd';
const c1 = s1[1];
console.log(c1); // b
```

**자바스크립트 `string`은 불변(immutable)이기 때문에 값을 수정할 수 없습니다.**

```jsx
// 자바스크립트 string => 불변(immutable)
const s2 = 'abcd';
s2[1] = 'z';
console.log(s2); // abcd
```

문자열의 `replace`라는 메서드를 이용하면 문자열의 일부 내용을 다른 내용으로 변경할 수 있습니다.

```jsx
const input = 'This is my car. The car is mine';
const output = input.replace('car', 'bike');
console.log({ input, output }); // { input: 'This is my car. The car is mine', output: 'This is my bike. The car is mine' }
console.log(input.replace(/car/g, 'bike')); // This is my bike. The bike is mine
// replaceAll
console.log(input.replaceAll('car', 'bike')); // This is my bike. The bike is mine
```

이때도 마찬가지로 `string`은 불변이기 때문에 기존에 있던 문자열을 변경하는게 아니라 새로운 문자열을 만들어냅니다. 코드의 결과를 보면 기존 문자열은 건드리지 않고 새로운 문자열에서만 `car`가 `bike`로 변경됐습니다. 그런데 두 번째 `car`는 변경되지 않았고 처음에 만나는 `car`만 변경되었습니다. 모든 `car`를 변경하고 싶다면 정규표현식에서 `g(global)` 플래그를 입력해서 변경할 수 있고, 최근에 추가된 `replaceAll`이라는 메서드를 사용해서 변경할 수 있습니다.

<aside>

📚 Note that the `String.prototype.replaceAll()` method was added in ES2021/ES12.

`replaceAll()` 는 최근에 추가된 메서드이기 때문에 [Node.js에서는 v15+에서 사용할 수 있습니다.](https://nodejs.org/en/blog/release/v15.0.0/#v8-8-6-35415) 브라우저 같은 경우 [지원하는 브라우저의 호환성](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll#browser_compatibility)을 확인하시면 됩니다.

</aside>

문자열에 특정한 문자열이 있는지 검사해주는 메서드를 알아보겠습니다.

```jsx
const s1 = 'This is my car. The car is mine';
console.log(s1.includes('car')); // true
console.log(s1.includes('my car')); // true
console.log(s1.includes('my car', 10)); // false

console.log(s1.startsWith('This is')); // true
console.log(s1.startsWith('is')); // false

console.log(s1.endsWith('mine')); // true
console.log(s1.endsWith('is')); // false
```

`includes`는 입력된 문자열이 있는지 검사를 해줍니다. 두 번째 매개변수는 검사를 시작하는 위치를 나타냅니다. `startsWith`는 입력된 문자열로 시작하는지 검사를 합니다. 반대로 `endsWith`는 입력된 문자열로 끝나는지 검사합니다.

문자열의 일부분을 추출하는 방법을 알아보겠습니다.

```jsx
const s1 = 'This is my car. The car is mine';
console.log(s1.substr(0, 4)); // This
console.log(s1.substr(5, 2)); // is
console.log(s1.substr(16)); // The car is mine

let pos = s1.indexOf(' ');
console.log(s1.substr(0, pos)); // This
pos = s1.lastIndexOf(' ');
console.log(s1.substr(pos + 1)); // mine

console.log(s1.slice(5, 7)); // is
```

`substr`이라는 메서드의 첫 번째 매개변수는 시작 위치를 나타내고 두 번째 매개변수는 길이를 나타냅니다. 그리고 두 번째 매개변수를 입력하지 않으면 문자열의 맨 끝까지 반환을 해줍니다. `indexOf`를 사용하면 입력된 문자열의 위치를 왼쪽부터 시작해서 처음으로 만나는 위치를 반환을 해줍니다. `lastIndexOf`는 반대로 뒤에서부터 검사합니다. `slice`라는 메서드도 있습니다. `substr`과 거의 비슷하지만 두 번째 매개변수가 길이가 아니고 인덱스라는 점이 다릅니다.

<aside>

⚠️ Deprecated: [substr](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

`substr`메서드는 deprecated이기때문에 `slice`메서드나 [substring](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substring)메서드를 사용하길 바랍니다.

</aside>

문자열을 분할하고 합치는 방법을 알아보겠습니다.

```jsx
const s1 = 'This is my car. The car is mine';
console.log(s1.split(' ')); // ['This', 'is', 'my', 'car.', 'The', 'car', 'is', 'mine']

const arr = s1.split('.');
console.log(arr); // ['This is my car', ' The car is mine']
console.log(arr.map(item => item.trim())); // ['This is my car', 'The car is mine']

console.log(s1.split(' ').join()); // This,is,my,car.,The,car,is,mine
console.log(s1.split(' ').join('..')); // This..is..my..car...The..car..is..mine
```

`split`이라는 메서드를 사용하면 문자열을 분할할 수 있습니다. 입력된 문자열 기준으로 분할을 하며 결과는 배열 형태로 반환합니다. `trim`이라는 메서드는 문자열 앞뒤에 있는 공백을 제거해줍니다. 분할된 문자열을 합칠 때는 `join`이라는 메서드를 사용하면 됩니다. `split`의 결과는 배열이므로 `join`은 배열의 메서드입니다. 콤마가 기본값이며 입력한 문자열과 함께 합쳐집니다.

`padStart`와 `padEnd`를 사용하면 문자열을 일정 길이 이상으로 만들면서 빈 공간에 패딩 문자를 입력할 수 있습니다.

```jsx
console.log('12'.padStart(5, '0')); // 00012
console.log('123'.padStart(5, '0')); // 00123
console.log('123'.padStart(5, '*')); // **123
console.log('123'.padEnd(5, '*')); // 123**
```

`match`라는 메서드는 정규표현식과 함께 많이 사용되며 정규표현식에 해당하는 문자열을 추출할 수가 있습니다.

```jsx
const s1 = 'This is my car. The car is mine';
console.log(s1.match(/T[^\s-]*/g)); // [ 'This', 'The' ]
```
