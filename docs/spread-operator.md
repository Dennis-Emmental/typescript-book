### 펼침(Spread) 연산자

펼침 연산자의 주 목적은 배열 또는 객체의 요소를 펼치는 것입니다. 이는 예제로 가장 잘 설명됩니다.

#### Apply
보통 사용법은 함수 인자에서 배열을 펼치는 것입니다. 이전에는 `Function.prototype.apply`를 사용할 필요가 있었습니다.:   

```ts
function foo(x, y, z) { }
var args = [0, 1, 2];
foo.apply(null, args);
```

이제 아래처럼 인자 앞에 `...`를 붙이는 것으로 쉽게 할 수 있습니다:

```ts
function foo(x, y, z) { }
var args = [0, 1, 2];
foo(...args);
```

여기에서 `args` 배열을 각 위치의 `인자`로 펼치고 있습니다.

#### 비구조화(Destructuring)
우리는 이미 *비구조화*에서 한 가지 사용법을 보았습니다:

```ts
var [x, y, ...remaining] = [1, 2, 3, 4];
console.log(x, y, remaining); // 1, 2, [3,4]
```
여기에서 동기는 비구조화시 단순히 배열의 남은 요소를 쉽게 접근하는 것입니다.

#### 배열 할당
펼침 연산자는 배열의 *확장된 버전*을 다른 배열에 쉽게 배치할 수 있게 합니다. 다음 예제에서 설명합니다:

```ts
var list = [1, 2];
list = [...list, 3, 4];
console.log(list); // [1,2,3,4]
```

#### 객체 펼침
한 객체를 다른 객체로 펼칠 수도 있습니다. 보통 사용법은 원본 수정없이 단순히 객체에 속성을 추가하는 것입니다.

```ts
const point2D = {x: 1, y: 2};
/** Create a new object by using all the point2D props along with z */
const point3D = {...point2D, z: 3};
```

다른 사용법은 얕은 확장을 하는 것입니다. 

```ts
const foo = {a: 1, b: 2};
const bar = {c: 1, d: 2};
/** Merge foo and bar */
const fooBar = {...foo, ...bar};
```

#### 정리
`apply`는 JavaScript에서는 불가피하게 사용할 것입니다. 그래서 `this` 인자에 추한 `null`이 없는 더 나은 문법을 갖는 것은 좋은 것입니다. 또한 배열을 다른 배열로 이동 (비구조화) 또는 이동하는 (할당) 전용 문법을 갖는 것은 배열의 부분에서 배열을 처리할 때 깔끔한 문법을 제공합니다.

[](https://github.com/Microsoft/TypeScript/pull/1931)
