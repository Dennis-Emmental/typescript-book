## this

함수 안에서 `this` 키워드에 대한 접근은 실제로 함수가 호출되는 방식에 따라 다릅니다. 보통 이를 "호출 컨텍스트"라고 합니다.

예제가 여기 있습니다.:

```ts
function foo() {
  console.log(this);
}

foo(); // logs out the global e.g. `window` in browsers
let bar = {
  foo
}
bar.foo(); // Logs out `bar` as `foo` was called on `bar`
```

그래서 `this`의 사용법에 신경을 써야 합니다. 클래스 안에서 호출 컨텍스트와 `this`와 연결을 끊으려면 화살표 함수를 사용하세요, [뒤에 더 자세히][arrow]

[arrow]:../arrow-functions.md
