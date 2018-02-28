### 비구조화

TypeScript는 다음의 비구조화의 형태를 지원합니다. (글자 그대로 구조를 깨는 비구조화):

1. 객체 비구조화
1. 배열 비구조화

*구조화*의 반대로 비구조화를 생각하면 쉽습니다. JavaScript에서 *구조화*의 방법은 객체 표현입니다.

```ts
var foo = {
    bar: {
        bas: 123
    }
};
```
JavaScript에 내장된 멋진 *구조화* 지원이 없다면, 즉석에서 새로운 객체를 만드는 것은 매우 성가셨을 것입니다. 비구조화는 구조체에서 데이터를 꺼낼 때 같은 수준의 편리함을 제공합니다. 

#### 객체 비구조화
여러 줄로 처리했을 것을 한 줄에 처리할 수 있기 때문에 비구조화는 유용합니다. 다음의 경우를 고려해보세요.:

```ts
var rect = { x: 0, y: 10, width: 15, height: 20 };

// 대입 비구조화
var {x, y, width, height} = rect;
console.log(x, y, width, height); // 0,10,15,20

rect.x = 10;
({x, y, width, height} = rect); // assign to existing variables using outer parentheses
console.log(x, y, width, height); // 10,10,15,20
```
여기에서 비구조화가 없다면 `rect`에서 하나씩 `x,y,width,height`를 선택해야 합니다.

추출한 변수를 새로운 변수에 대입하는 것은 다음처럼 할 수 있습니다.: 

```ts
// structure
const obj = {"some property": "some value"};

// destructure
const {"some property": someProperty} = obj;
console.log(someProperty === "some value"); // true
```

또한, 비구조화를 사용하여 구조체의 *깊은* 데이터를 얻을 수 있습니다. 다음 예제에서 볼 수 있습니다.:

```ts
var foo = { bar: { bas: 123 } };
var {bar: {bas}} = foo; // Effectively `var bas = foo.bar.bas;`
```

#### 나머지에 대한 객체 비구조화
객체에서 여러 개의 요소를 선택할 수 있고 나머지에 대한 객체 비구조화를 사용하여 나머지 요소의 *객체*를 얻을 수 있습니다.

```ts
var {w, x, ...remaining} = {w: 1, x: 2, y: 3, z: 4};
console.log(w, x, remaining); // 1, 2, {y:3,z:4}
```
일반적인 사용 예는 특정 속성을 무시하는 것입니다. 예제입니다.:
```ts
// Example function
function goto(point2D: {x: number, y: number}) {
  // Imagine some code that might break
  // if you pass in an object
  // with more items than desired
}
// Some point you get from somewhere
const point3D = {x: 1, y: 2, z: 3};
/** A nifty use of rest to remove extra properties */
const { z, ...point2D } = point3D;
goto(point2D);
```

#### 배열 비구조
일반적인 프로그래밍 질문: "세 번째 변수를 사용하지 않고 두 변수를 서로 바꾸는 방법은?". TypeScript 해법:

```ts
var x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2,1
```
배열 비구조화는 컴파일러가 효과적으로 `[0], [1], ...`을 하는 것입니다. 이 값들이 존재할 것이라는 보장은 없습니다.

#### 나머지에 대한 배열 비구조화
배열에서 여러 개의 요소를 선택할 수 있고 나머지에 대한 배열 비구조화를 사용하여 나머지 요호의 *객체*를 얻을 수 있습니다.

```ts
var [x, y, ...remaining] = [1, 2, 3, 4];
console.log(x, y, remaining); // 1, 2, [3,4]
```

#### 무시가 포함된 배열 비구조화
대입의 왼쪽에 예를 들어 `, ,` 같이 해당 위치를 비워두는 것으로 무시할 수 있습니다. 예제입니다.:  
```ts
var [x, , ...remaining] = [1, 2, 3, 4];
console.log(x, remaining); // 1, [3,4]
```

#### JS 생성
비 ES6 대상을 위한 JavaScript 생성은 모국어 없이 비구조화를 스스로 지원해야 하는 것처럼 임시 변수들을 생성하는 것을 포함합니다. 예제입니다.:  

```ts
var x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2,1

// becomes //

var x = 1, y = 2;
_a = [y,x], x = _a[0], y = _a[1];
console.log(x, y);
var _a;
```

#### 정리
비구조화는 줄 수를 줄이고 의도를 명확하게 하는 것으로 코드를 읽기 쉽고 유지보수 좋게 만들 수 있습니다. 배열 비구조화는 튜플인 것처럼 배열을 사용할 수 있게 합니다.
