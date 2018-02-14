### const

ES6 / TypeScript에서 제공하는 `const`는 매우 환영받는 부가 기능입니다. 변수를 변경하지 못하게 합니다. 런타인 관점뿐만 아니라 문서로도 중요합니다. 사용하려면 그저 `var`를 `const`로 바꿔주세요.: 

```ts
const foo = 123;
```

> 예를 들어 변수 + 행동 지정자처럼 사용자가 `let constant foo` 같이 더 입력하게 만드는 다른 언어보다 (개인적인 생각으로) 문법은 더 좋습니다. 

`const`는 가독성과 유지 보수성을 위해 좋은 실천법이며 *마법의 문자* 사용을 피합니다. 예를 들면

```ts
// Low readability
if (x > 10) {
}

// Better!
const maxRows = 10;
if (x > maxRows) {
}
```

#### const 선언은 반드시 초기화되어야 합니다.
다음은 컴파일 에러입니다.:

```ts
const foo; // ERROR: const declarations must be initialized
```

#### 대입에서 왼쪽은 상수가 될 수 없습니다.
생성한 뒤에 상수는 바꿀 수 없습니다. 그래서 새로운 값으로 대입하려고 한다면 컴파일 에러입니다.: 

```ts
const foo = 123;
foo = 456; // ERROR: Left-hand side of an assignment expression cannot be a constant
```

#### 블록 범위
[`let`](./let.md)에서 본 것같이 `const`는 블록 범위입니다.:

```ts
const foo = 123;
if (true) {
    const foo = 456; // `if` 블록에 제한되는 새로운 변수로 가능
}
```

#### 깊은 불변성
변수의 *참조*를 보호하는 한 `const`는 객체 표현식에서도 동작합니다.

```ts
const foo = { bar: 123 };
foo = { bar: 456 }; // ERROR : Left hand side of an assignment expression cannot be a constant
```

그러나 아래 보이는 것처럼 객체의 하위 속성을 변경하는 것은 허락합니다.

```ts
const foo = { bar: 123 };
foo.bar = 456; // Allowed!
console.log(foo); // { bar: 456 }
```

이런 이유로 기본 타입 또는 불변 자료 구조에서 `const`를 사용하는 것을 추천합니다.
