## 일치

JavaScript에서 주의해야 할 한가지는 `==`과 `===`는 다르다는 것이다. JavaScript는 프로그래밍 오류를 복원하려 하므로 `==`는 두 변수 사이에서 강제로 타입 변환을 시도합니다. 예를 들면 아래에 보이는 것처럼 숫자와 비교할 수 있게 문자열을 숫자로 변환합니다.:

```js
console.log(5 == "5"); // true   , TS Error
console.log(5 === "5"); // false , TS Error
```

그러나 JavaScript의 선택이 항상 이상적이지는 않습니다. 예를 들어 아래 예제에서 `""`과 `"0"`은 둘 다 문자열이고 명확하게 다르기 때문에 첫 번째 구문은 거짓입니다. 그러나 두 번째 경우에서 `0`과 빈 문자열 (`"""`)은 falsy이므로 (`false`처럼 행동) `==`에 대해서는 같습니다. `===`를 사용한다면 두 구문은 거짓입니다.

> 역자주: 0 == "" 비교는 ""을 숫자로 변환하면 0이고, 이를 비교하기 때문입니다.

```js
console.log("" == "0"); // false
console.log(0 == ""); // true

console.log("" === "0"); // false
console.log(0 === ""); // false
```

> TypeScript에서 `string == number`과 `string === number`은 컴파일할 때 오류여서 보통 이런 것들을 걱정할 필요가 없습니다. 

`==`과 `===`에 비슷하게 `!=`과 `!==`이 있습니다.

전문가 조언: 뒤에서 살펴볼 null 검사를 제외하고 항상 `===`과 `!==`을 사용하세요.

## 구조적 일치
두 객체를 구조적으로 비교하려고 한다면 `==`/`===`은 충분하지 ***않습니다***. 예) 

```js
console.log({a:123} == {a:123}); // False
console.log({a:123} === {a:123}); // False
```
이러한 검사를 하려면 [deep-equal](https://www.npmjs.com/package/deep-equal) npm 패키지를 사용하에요. 예) 

```js
import * as deepEqual from "deep-equal";

console.log(deepEqual({a:123},{a:123})); // True
```
