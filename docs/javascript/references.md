## 참조 (Reference)

고정된 값 (literal) 외에 JavaScript에서 객체는 (함수, 배열, 정규 표현식 등을 포함하여 )참조이다. 이는 다음을 의미합니다.

### 모든 참조가 변경됩니다

```js
var foo = {};
var bar = foo; // bar는 같은 객체의 참조입니다

foo.baz = 123;
console.log(bar.baz); // 123
```

### 동등함은 참조에 대한 것입니다

```js
var foo = {};
var bar = foo; // bar는 참조
var baz = {}; // baz는 `foo`와 다른 *새로운 객체*

console.log(foo === bar); // true
console.log(foo === baz); // false
```
