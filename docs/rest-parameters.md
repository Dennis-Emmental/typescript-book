### 나머지 매개 변수
나머지 매개 변수는 (마지막 인자가 `...argumentName`으로 표시됨) 함수에서 여러 인자를 배열로 받습니다. 아래 예제로 설명합니다. 
Rest parameters (denoted by `...argumentName` for the last argument) allow you to quickly accept multiple arguments in your function and get them as an array. This is demonstrated in the below example.

```ts
function iTakeItAll(first, second, ...allOthers) {
    console.log(allOthers);
}
iTakeItAll('foo', 'bar'); // []
iTakeItAll('foo', 'bar', 'bas', 'qux'); // ['bas','qux']
```

나머지 매개 변수는 `function`/`()=>`/`class member` 같은 함수에서 사용할 수 있습니다.

