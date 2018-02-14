### let

JavaScript에서 `var` 변수는 *함수 범위*입니다. 이는 변수가 *블록 범위*인 다른 언어와 (C# / Java 등) 다릅니다. JavaScript에 *블록 범위* 사고 방식을 가져온다면 다음 예제에서 `456`을 출력하는 대신에 `123`이 출력할 것을 기대할 것입니다.

```ts
var foo = 123;
if (true) {
    var foo = 456;
}
console.log(foo); // 456
```
`{`이 새로운 *변수 범위*를 생성하지 않기 때문입니다. if *블록* 밖과 안에 변수 `foo`는 동일합니다. JavaScript 프로그래밍의 일반적인 오류의 원인입니다. TypeScript에서 (그리고 ES6) 변수를 *블록 범위*로 정의할 수 있는 `let` 키워드를 소개하는 이유입니다. `var`대신에 `let`을 사용한다면 범위 밖에 정의된 것과 분리된 고유한 요소를 얻을 수 있습니다. 같은 예제를 `let`으로 바꿨습니다.:

```ts
let foo = 123;
if (true) {
    let foo = 456;
}
console.log(foo); // 123
```

`let`이 오류를 방지하는 또 다른 장소는 반복문입니다.
```ts
var index = 0;
var array = [1, 2, 3];
for (let index = 0; index < array.length; index++) {
    console.log(array[index]);
}
console.log(index); // 0
```
정말 진심으로 가능하다면 `let`을 사용하는 것이 더 좋음을 알게될 것입니다. 신규 및 기존 다른 언어 개발자들에게 덜 놀라움을 가져다 줍니다.

#### 함수는 새로운 범위를 생성합니다.
앞에서 언급한 것처럼 JavaScript에서 함수가 새로운 변수 범위를 생성한다는 것을 보여줄 것입니다. 다음을 보겠습니다.:

```ts
var foo = 123;
function test() {
    var foo = 456;
}
test();
console.log(foo); // 123
```
이는 기대했던 대로 동작합니다. 이것이 없다면 JavaScript로 코드 작성하는 것이 매우 어려울 것입니다.

#### 생성된 JS
유사한 이름이 주변 범위에 존재한다면 TypeScript가 만들어낸 JS는 `let` 변수의 이름을 변경합니다. 예를 들어 `var`를 `let`으로 단순 변환하여 생성된 JS입니다.:

```ts
if (true) {
    let foo = 123;
}

// becomes //

if (true) {
    var foo = 123;
}
```
그러나 주변 범위에서 이미 사용중이라면 변수 이름은 보이는 것처럼 생성됩니다. (`_foo`를 보세요.):

```ts
var foo = '123';
if (true) {
    let foo = 123;
}

// becomes //

var foo = '123';
if (true) {
    var _foo = 123; // Renamed
}
```

#### 클로저에서 let
JavaScript 개발자들에 대한 공통적인 프로그래밍 인터뷰 질문은 이 간단한 파일의 로그의 출력은 무엇인 지입니다.

```ts
var funcs = [];
// create a bunch of functions
for (var i = 0; i < 3; i++) {
    funcs.push(function() {
        console.log(i);
    })
}
// call them
for (var j = 0; j < 3; j++) {
    funcs[j]();
}
```
누군가는 `0,1,2`를 기대합니다. 놀랍게도 모든 세 함수는 `3`이 될 것입니다. 그 이유는 모든 세 함수는 바깥 범위의 변수 `i`를 사용 중이고 함수들의 실행 시점에 (두 번째 반복문에서) `i`의 값은 3이 될 것이기 때문입니다. (첫 번째 반복분의 종료될 때의 상황입니다.)

해결법은 각 반복에서 새로운 변수를 생성하는 것입니다. 전에 배웠던 것처럼 새로운 함수를 생성하고 즉시 실행함으로 아래 보는 것처럼 새로운 변수 범위를 만들 수 있습니다. (예를 들어 클래스의 IIFE 패턴 `(function() { /* body */ })();`)

```ts
var funcs = [];
// create a bunch of functions
for (var i = 0; i < 3; i++) {
    (function() {
        var local = i;
        funcs.push(function() {
            console.log(local);
        })
    })();
}
// call them
for (var j = 0; j < 3; j++) {
    funcs[j]();
}
```
여기에서 함수는 *지역* 변수를 (편의상 `local`로 이름 지어진) 가두고 (그래서 클로저(closure)로 부름) 반복문 변수 `i` 대신에 사용합니다.

> 클로저는 성능에 영향을 줍니다. (주변 상태를 저장해야 합니다.)

반복문에서 ES6 `let` 키워드는 앞의 예제와 같은 동작을 합니다.

```ts
var funcs = [];
// create a bunch of functions
for (let i = 0; i < 3; i++) { // Note the use of let
    funcs.push(function() {
        console.log(i);
    })
}
// call them
for (var j = 0; j < 3; j++) {
    funcs[j]();
}
```

`var` 대신에 `let`을 사용하여 각 반본 단계에서 고유한 변수 `i`를 생성하세요.

#### 정리
`let`은 코드 대부분에서 매우 유용합니다. 코드 가독성을 크게 향상하고 프로그래밍 오류의 가능성을 줄일 수 있습니다.

[](https://github.com/olov/defs/blob/master/loop-closures.md)
