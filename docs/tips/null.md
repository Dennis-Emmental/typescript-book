# Null은 나쁩니다
Javascript(와 확장해서 TypeScript)는 두 가지 기본 타입을 가지고 있습니다 : `null`과 `undefined`. 이 둘은 다른 것을 의미함을 *의도한* 것입니다:

* 뭔가 초기화되지 않았습니다 : `undefined`
* 뭔가 지금 사용 불가합니다 : `null` 

대부분의 다른 언어는 (보통 `null`이라고 불리는) 한 가지만 가지고 있습니다. 기본으로 JavaScript는 초기화되지 않은 변수 / 인자 / 속성을 `undefined`로 처리할 것이기 때문에 (선택할 수 없습니다) *사용 불가* 상태를 그것으로 사용할 것을 추천하고 `null`을 괴롭히지 마세요.

## 실제 토론
TypeScript 팀은 `null`을 사용하지 않습니다 : [TypeScript 코딩 가이드라인](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined)과 이는 아무 문제도 발생하지 않습니다. Douglas Crockford는 [`null`은 나쁜 아이디어](https://www.youtube.com/watch?v=PSGEjv3Tqo0&feature=youtu.be&t=9m21s)라고 생각합니다. 그리고 모두 `undefined`를 사용해야 합니다.

## `null` 스타일을 다루기 위한 코드 베이스
코드 베이스가 `null`을 주는 다른 API를 사용한다면 `== undefined`로 (`===` 대신) 검사하세요. 잠재적인 *잘못된* 값 때문에 이렇게 사용하는 것이 안전합니다.   
```ts
/// Image you are doing `foo == undefined` where foo can be one of:
console.log(undefined == undefined); // true
console.log(null == undefined); // true
console.log(0 == undefined); // false
console.log('' == undefined); // false
console.log(false == undefined); // false
```

## 추가 팁

### `undefined`의 명시적인 사용 제약하기
또한 TypeScript는 값과 별개로 구조체를 *문서화*하는 기회를 주기 때문에 다름과 같은 것 대신:
```ts
function foo(){
  // if Something
  return {a:1,b:2};
  // else
  return {a:1,b:undefined};
}
```
타입 주석을 사용할 수 있습니다:
```ts
function foo():{a:number,b?:number}{
  // if Something
  return {a:1,b:2};
  // else
  return {a:1};
}
```

### node 스타일의 콜백
노드 스타일의 콜백 함수는 (예를들어 `(err,somthingElse) => { /* something */ }`) 일반적으로 에러가 없다면 `err`을 `null`로 설정하여 호출됩니다. 어째든 일반적으로 참거짓 확인을 사용합니다: 

```ts
fs.readFile('someFile', 'utf8', (err,data) => {
  if (err) {
    // do something
  }
  // no error
});
```
자신의 API를 만들때 일관성을 유지하기 위해서 `null`을 사용하는 것은 괜찮습니다. 자신의 API에서 pomise를 사용할 경우 에러 값의 부재를 처리할 필요가 없습니다. (`.then` vs. `.catch`로 처리합니다)

### *유효성*을 나타내는 의미로 `undefined`를 사용하지 마세요

예를들어 다음과 같은 끔찍한 함수:

```ts
function toInt(str:string) {
  return str ? parseInt(str) : undefined;
}
```
다음과 같이 작성하는 것이 더 낫습니다:
```ts
function toInt(str: string): { valid: boolean, int?: number } {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  }
  else {
    return { valid: true, int };
  }
}
```
