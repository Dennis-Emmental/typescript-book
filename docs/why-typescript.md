# 왜 TypeScript인가
TypeScript의 두 가지 주요 목표는 다음과 같습니다.:
* JavaScript를 위해 *선택적 타입 시스템*을 제공합니다.
* 미래의 JavaScript 에디션에 있는 계획된 기능을 현재 JavaScript 엔진에 제공합니다. 

이 목표에 대한 욕구는 아래에 동기 부여가 됩니다.

## TypeScript 타입 시스템

"**왜 JavaScript에 타입을 추가하나요?**"라고 궁금해할 것입니다.

타입은 코드 품질과 이해력을 향상하는 것으로 입증되었습니다. 대규모 팀은 (Google, Microsoft, Facebook) 계속해서 결론에 도달했습니다. 구체적으로:

* 리팩토링을 할 때, 타입은 민첩성을 높여줍니다. *실행 시 오류를 발생시키는 것보다 컴파일러가 오류를 잡아내는 것이 더 좋습니다*.
* 타입은 사용자가 가질 수 있는 가장 좋은 문서 형태 중의 하나입니다. *함수 서명(signature)은 정리이고, 함수 몸체는 증명입니다*. 

그러나 타입은 불필요하게 지나치게 형식적이 되는 방법을 가지고 있습니다. TypeScript는 가능한 진입 장벽을 가능한 낮게 유지하는 것에 매우 각별합니다. 여기에 그 방법이 있습니다.:

### JavaScript는 TypeScript입니다.
TypeScript는 JavaScript 코드에 대해 컴파일 시간에 타입 안정성을 제공합니다. 이름을 보면 놀랍지는 않습니다. 대단한 것은 타입은 완전하게 선택 사항입니다. JavaScript 코드 `.js` 파일은 `.ts` 파일로 이름을 바꿀 수 있고, TypeScript는 원본 JavaScript 파일과 같 유효한 `.js`를 제공할 것입니다. TypeScript는 *의도적으로* 그리고 철저하게 선택적 타입 시스템을 가지고 있는 JavaScript의 상위 집합입니다. 

### 타입은 묵시적일 수 있습니다.
TypeScript는 코드 개발 동안 최소의 생산성으로 타입 안전을 제공하기 위해 최대한 많은 타입 정보를 유추하려고 노력할 것입니다. 예를 들면 다음 예제에서 TypeScript는 아래에 foo가 `number` 타입인 것을 알고 있으며 다음과 같이 두 번째 줄에서 오류가 발생할 것입니다.: 

```ts
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```
이런 타입 추론은 동기 부여가 잘 되어 있습니다. 이 예제에서 보여준 것처럼 한다면 코드의 나머지 부분에서 `foo`가 `number`인지 `string`인지 확실할 수 없습니다. 이러한 문제는 대규모 다중 파일 코드 기반에서 자주 발생합니다. 나중에 타입 추론 규칙에 대해 자세히 살펴볼 것입니다.

### 타입은 명시적일 수 있습니다.
앞에서 언급했듯이 TypeScript는 가능한 한 안전하게 추론할 수 있지만 어노테이션(annotation)을 사용할 수 있습니다.:
1. 컴파일러 실행을 도우며 무보다도 코드를 읽어야 할 다음 개발자를 (미래에는 당신일 수도 있습니다!) 위한 문서 항목입니다.
1. 컴파일러가 봐야 한다고 생각하는 것을 시행합니다. 코드에 대한 이해는 (컴파일러에 의해 수행되는) 코드의 알고리즘 분석과 일치합니다.

TypeScript는 *선택적으로* 어노테이션이 붙는 다른 언어에서 대중적인 뒤에 붙이는 타입 어노테이션을 사용합니다. (예: ActionScript와 F#)

```ts
var foo: number = 123;
```
그래서 뭔가 잘못하면 컴파일러는 다음과 같이 에러를 발생시킵니다.:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

TypeScript에서 지원하는 모든 어노테이션 문법에 대한 자세한 내용을 이후 장에서 다룰 것입니다. 

### 타입은 구조적입니다.
일부 언어에서는 (특히 겉으로 타입을 지원하는) 정적 타이핑이 불필요하게 형식적이 되는 결과를 초래합니다. 코드가 잘 동작하리라는 것을 *알고* 있을지라도 언어 의미상 주변의 잡동사니 복사를 강제하기 때문입니다. [C#을 위한 automapper](http://automapper.org/) 같은 것들이 C#에서 *필수*인 이유입니다. TypeScript에서는 가장 적은 기억력을 가진 JavaScript 개발자가 쉽게 사용할 수 있기를 원하 때문에 타입은 *구조적*입니다. 이는 *덕 타이핑(duck typing)*이 가장 중요한 언어 구조임을 의미합니다. 다음의 예를 생각해보십시. `iTakePoint2D` 함수는 기대하는 모든 것들을 (`x`와 `y`) 포함하고 있는 것들은 다 받아들일 것입니다. 

```ts
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### 타입 오류는 JavaScript 발행을 막지 않습니다.
JavaScript 코드를 TypeScript로 쉽게 옮길 수 있게 컴파일 오류가 있더라도 기본적으로 TypeScript는 할 수 있는 최대한으로 *유효한 JavaScript를 발행할 것입니다*. 예를 들면:

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

다음 js를 발행할 것입니다.:

```ts
var foo = 123;
foo = '456';
```

그래서 JavaScript를 TypeScript로 점진적으로 바꿀 수 있습니다. 많은 다른 언어 컴파일러가 동작하는 것과 매우 다른 점이고 TypeScript로 옮겨야 하는 또 다른 이유입니다.

### 타입은 주변 환경일 수 있습니다.
TypeScript의 주요 설계 목표는 안전하고 쉽게 기존 JavaScript 라이브러리를 TypeScript에서 사용할 수 있게 하는 것이었습니다. TypeScript는 *선언*으로 이를 수행합니다. TypeScript는 선언에 넣기 위한 노력의 양 또는 정도를 조정할 수 있습니다. 더 큰 노력은 더 많은 타입 안정성과 코드 정보를 더 넣습니다. 대부분의 인기 있는 JavaScript 라이브러리에 대한 정의는 [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped)에 이미 작성있어 대부분의 경우 다음 중 하나입니다.:

1. 정의 파일은 이미 존재합니다.
1. 또는 최소한 이미 사용할 수 있는 잘 검토된 TypeScript 선언 템플릿의 방대한 목록을 가지고 있습니다.

선언 파일을 작성하는 방법에 대한 간단한 예로 [jquery](https://jquery.com/)의 간단한 예를 생각해봅니다. 기본적으로 (좋은 JS Code에서 기대되는 것 처럼) TypeScaript는 사용하기 전에 선언할 것을 기대합니다. (예를 들면 `var`를 어딘가에서 사용)
```ts
$('.awesome').show(); // Error: cannot find name `$`
```
빠르게 수정하는 거로 진짜로 `$`로 불리는게 있다는 것을 *TypeScript에 알릴 수 있습니다.*:
```ts
declare var $: any;
$('.awesome').show(); // Okay!
```
원하는 경우 기본 정의를 기반으로 오류로부터 사용자를 보호하는 데 도움이 되는 더 많은 정보를 제공할 수 있습니다. 
```ts
declare var $: {
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

TypeScript에 대해서 (예를 들면 `interface`와 `any` 같은 것들) 알게 된 후 이미 있는 JavaScript에 대해서 TypeScript 정의를 생성하는 구체적인 내용을 자세하게 논의할 것입니다.

## 미래의 JavaScript => 지금
TypeScript는 지금 JavaScript 엔진에 (ES5만 지원) ES6에 계획된 많은 기능을 제공합니다. TypeScript 팀은 이런 기능들을 적극적으로 추가하고 있으며 이 목록은 시간이 지날수록 늘어날 것입니다. 본보기로 여기에 클래스 예제가 있습니다.:

```ts
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // { x: 10, y: 30 }
```

그리고 사랑스런 두툼한 화살표 함수(arrow function):

```ts
var inc = x => x+1;
```

### 정리
이 장에서는 TypeScript의 동기와 디자인 목표를 제시했습니다. 이제 방해받지 않고 TypeScript의 자세한 내용을 살펴볼 수 있습니다. 

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
