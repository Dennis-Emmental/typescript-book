## 숫자
프로그래밍 언어에서 숫자를 다룰 때마다 그 언어가 숫자를 다루는 특징을 잘 알아야 합니다. JavaScript에서 숫자에 대해서 알아야 할 몇 가지 중요한 정보가 있습니다.  

### 핵심 타입
JavaScript는 단 하나의 숫자 타입이 있습니다. 배정밀도(double-precision) 64-bit `Number`입니다. 아래에 권장하는 방법과 함께 제약 사항을 논의합니다.

### 소수
다른 언어에서 double / float에 익숙하다면, 이진 부동 소수점 숫자가 정확하게 소수로 변화되지 *않는* 것을 알 것입니다. JavaScript에 있는 사소한 (그리고 유명한) 예는 다음과 같습니다.:

```js
console.log(.1 + .2); // 0.30000000000000004
```

> 진정한 소수 계산은 아래에 언급된 `big.js`을 사용하세요.

### 정수
내장된 숫자 타입으로 표현되는 정수 제한은 `Number.MAX_SAFE_INTEGER`과 `Number.MIN_SAFE_INTEGER`입니다.

```js
console.log({max: Number.MAX_SAFE_INTEGER, min: Number.MIN_SAFE_INTEGER});
// {max: 9007199254740991, min: -9007199254740991}
```

이 문맥에서 **안전**은 *반올림 오류가 없는* 값이라는 사실을 나타냅니다.

안전하지 않은 값은 안전한 값으로부터 `+1 / -1` 떨어져 있고, 더하기 / 빼기의 결과를 안전하지 않은 값으로 반올림할 것입니다.

```js
console.log(Number.MAX_SAFE_INTEGER + 20 === Number.MAX_SAFE_INTEGER + 1); // true!
console.log(Number.MIN_SAFE_INTEGER - 20 === Number.MIN_SAFE_INTEGER - 1); // true!
```

> 역자 주: 위 코드 제가 테스트해보니 false로 나옵니다. `console.log(Number.MAX_SAFE_INTEGER + 2 === Number.MAX_SAFE_INTEGER + 1);`가 true로 나옵니다. 

안정성을 검사하려면 ES6 `Number.isSafeInteger`을 사용하세요.:

```js
// Safe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true

// Unsafe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false

// Because it might have been rounded to it due to overflow
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 10)); // false
```

> 임의 정밀도 정수 계산은 아래 언급되는 `big.js`를 사용하세요.

### big.js
재무 계산을 (예를 들면 GST 계산, 센트 추가하는 돈 등) 하려면 다음을 목적으로 하는 [big.js](https://github.com/MikeMcl/big.js/)같은 라이브러리를 사용하세요.
* 완벽한 소수 계산.
* 범위를 벗어난 정수값에 대한 안전함

설치는 매우 쉽습니다.:
```bash
npm install big.js @types/big.js
```

빠른 사용 예제:

```js
import { Big } from 'big.js';

export const foo = new Big('111.11111111111111111111');
export const bar = foo.plus(new Big('0.00000000000000000001'));

// number 값으로:
const x: number = Number(bar.toString()); // 정밀도를 잃어 버린다
```

> 예를 들면 차트, 화면 그리기 등 UI / 성능 집중적인 목적의 계산에는 이 라이브러리를 사용하지 마세요.

### NaN
몇 가지 숫자 계산에서 유효한 값이 없으면, JavaScript는 특별한 값인 `NaN`을 반환합니다. 고전적인 예는 가상의 숫자입니다.

```js
console.log(Math.sqrt(-1)); // NaN
```

참고: `NaN` 값에 대한 일치 검사는 제대로 동작하지 않습니다. 대신 `Number.isNaN`을 사용하세요.:

```js
// 이렇게 하지 마세요
console.log(NaN === NaN); // false!!

// 이렇게 하세요
console.log(Number.isNaN(NaN)); // true
```

### Infinity
숫자에서 범위를 벗어나는 값은 고정된 `Number.MAX_VALUE`과 `-Number.MAX_VALUE` 값으로 표현됩니다.

```js
console.log(Number.MAX_VALUE);  // 1.7976931348623157e+308
console.log(-Number.MAX_VALUE); // -1.7976931348623157e+308
```

정밀도가 변하지 않은 범위 밖의 값은 이 제한에 고정됩니다. 예를 들면: 

```js
console.log(Number.MAX_VALUE + 1 == Number.MAX_VALUE);   // true!
console.log(-Number.MAX_VALUE - 1 == -Number.MAX_VALUE); // true!
```

정밀도가 변하는 범위 밖의 값은 특별한 값인 `Infinity`/`-Infinity`으로 결정됩니다. 예를 들면:

```js
console.log(Number.MAX_VALUE + 10**1000);  // Infinity
console.log(-Number.MAX_VALUE - 10**1000); // -Infinity
```

물론 이 특별한 문한 값은 이 값이 필요한 산술 연산에서도 나타납니다. 예를 들면:

```js
console.log( 1 / 0); // Infinity
console.log(-1 / 0); // -Infinity
```

이 `Infinity` 값을 직접 또는 아래에 보는 것처럼 `Number` 클래스의 정적 멤버로 사용할 수 있습니다. 

```js
console.log(Number.POSITIVE_INFINITY === Infinity);  // true
console.log(Number.NEGATIVE_INFINITY === -Infinity); // true
```

다행히 비교 연산자는 (`<` / `>`) 무한 값에서 안정적으로 잘 동작합니다.:

```js
console.log( Infinity >  1); // true
console.log(-Infinity < -1); // true
```

### 무한소

Number에서 표현 가능한 가장 작은 0이 아닌 값은 `Number.MIN_VALUE`로 사용할 수 있습니다. 

```js
console.log(Number.MIN_VALUE);  // 5e-324
```

`MIN_VALUE` 보다 작은 값은 0으로 바뀝니다.

```js
console.log(Number.MIN_VALUE / 10);  // 0
```

> 더 직관적: 유사하게 `Number.MAX_VALUE`보다 큰 값은 무한으로, `Number.MIN_VALUE`보다 작은 값은 0으로 바뀝니다.
