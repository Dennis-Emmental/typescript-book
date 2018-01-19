## Null과 Undefined

사실 둘 다 다뤄야 할 것입니다. 그냥 `==` 비교로 확인하세요

```ts
/// bar가 다음 중 하나 일 수 있는 `foo.bar == undefined`를 한다고 상상하세요.:
console.log(undefined == undefined); // true
console.log(null == undefined); // true
console.log(0 == undefined); // false
console.log('' == undefined); // false
console.log(false == undefined); // false
```
`undefined` 또는 `null`인지 확인할 때 `== null`를 추천합니다. 보통 둘 사이의 구분을 원하지 않을 것입니다.

## undefined

`== null`을 사용해야 한다고 이야기한 것을 기억하세요. 물론 기억합니다. (바로 위에서 이야기 했기 때문에 ^) 최상위 수준 것에 대해서는 사용하지 마세요. strict 모드에서 `foo`를 사용하고 `foo`가 정의되어 있지 않으면 `ReferenceError` **예외**가 발생하고 전체 호출 스택이 풀려버립니다.

> strict 모드를 사용해야합니다 ... 그리고 사실 모듈을 사용하면 TS 컴파일러가 자동으로 추가합니다. ... 책의 뒷부분에 자세히 나오지만, 분명히 하지 않아도 됩니다. :)

그래서 *전역* 수준에서 변수가 정의되었는지 확인하려면 보통 `typeof`를 사용합니다.: 

```ts
if (typeof someglobal !== 'undefined') {
  // someglobal는 사용하기에 안전
  console.log(someglobal);
}
```
