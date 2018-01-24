#### IIFE는?
클래스에 대해서 생성된 js는 다음과 같을 수 있습니다.: 
```ts
function Point(x, y) {
    this.x = x;
    this.y = y;
}
Point.prototype.add = function (point) {
    return new Point(this.x + point.x, this.y + point.y);
};
```

예를 들어 즉식 호출 함수 표현식으로(IIFE) 쌓여진 이유는 

```ts
(function () {

    // BODY

    return Point;
})();
```

상속과 관련이 있습니다. TypeScript가 기반 클래스를 변수 `_super`로 저장할 수 있습니다. 예를 들어

```ts
var Point3D = (function (_super) {
    __extends(Point3D, _super);
    function Point3D(x, y, z) {
        _super.call(this, x, y);
        this.z = z;
    }
    Point3D.prototype.add = function (point) {
        var point2D = _super.prototype.add.call(this, point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    };
    return Point3D;
})(Point);
```

IIFE는 TypeScript가 `_super` 변수에 기반 클래스 `Point`를 쉽게 저장할 수 있게 해주고 클래스 본체에서 일관되게 사용합니다.

### `__extends`
클래스를 사용하자마자 TypeScript는 다음 함수를 생성합니다.:
```ts
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
```
`d`는 파생된 클래스를 나타내고 `b`는 기반 클래스를 나타냅니다. 이 함수는 두 가지 일을 합니다.:
1. 기반 클래스의 정적 멤버를 자식 클래스에 복사합니다. 예를 들면 `for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];` 입니다.
1. 자식 클래스 함수의 프로토타입에 부모의 `proto`의 멤버를 선택적으로 보게 설정합니다. 예를 들면 `d.prototype.__proto__ = b.prototype` 입니다.

1을 이해하는 데 어려움을 겪는 사람은 드물지만, 많은 사람은 2를 이해하는 데 어려움을 겪습니다. 이어서 설명을 합니다.

#### `d.prototype.__proto__ = b.prototype`

이를 많은 사람에게 가르쳐본 후 다음 설명이 가장 단순하다는 것을 알게 되었습니다. 먼저 `__extends`의 코드가 `d.prototype.__proto__ = b.prototype`와 같은지 설명하고 왜 이 라인이 중요한지 설명할 것입니다. 이 모든 것을 이해하려면 다음 사항을 알아야 합니다.

1. `__proto__`
1. `prototype`
1. 호출된 함수 안의 `this`에 미치는 `new`의 영향
1. `prototype`과 `__proto__`에 미치는 `new`의 영향

JavaScript의 모든 객체는 `__proto__` 멤버를 가지고 있습니다. 이 멤버는 오래된 브라우저에서는 지원하지 않을 수 있습니다. (때로는 문서에서 이 마법 속성을 `[[prototyp]]`이라고 합니다.) 하나의 목표가 있습니다.: 만약 찾는 동안 (예를 들어 `obj.property`) 객체에서 속성이 발견되지 않으면 `obj.__proto__.property`에서 찾으려고 합니다. 그래도 찾지 못하면 `obj.__proto__.__proto__.property`에서 둘 중 하나를 찾으려고 반복합니다. *찾았거나* *마지막 `.__proto__`가 null*이거나.  JavaScript가 *프로토타입 상속*을 지원한다고 말하는 이유입니다. 다음 예제를 chrome 콘솔 또는 Node.js에서 수행할 수 있습니다.

```ts
var foo = {}

// foo.__proto__와 foo 설정
foo.bar = 123;
foo.__proto__.bar = 456;

console.log(foo.bar); // 123
delete foo.bar; // 객체에서 제거
console.log(foo.bar); // 456
delete foo.__proto__.bar; // foo.__proto__에서 제거
console.log(foo.bar); // undefined
```

`__proto__`를 이해했습니다. 또 다른 유용한 정보는 JavaScript의 모든 `함수`는 `prototype`이라는 속성을 가지고 있고, `prototype`은 그 함수를 가리키는 `constructor` 멤버를 가지고 있습니다. 아래에 나와 있습니다.:

```ts
function Foo() { }
console.log(Foo.prototype); // {} i.e. 존재하고 정의되지 않음
console.log(Foo.prototype.constructor === Foo); // 함수를 가리키는 `constructor` 멤버를 가지고 있음
```

*호출된 함수 안에서 `this`에 미치는 `new`의 영향*을 살펴보겠습니다. 기본적으로 호출된 함수 안에서 `this`는 함수에서 반환될 새롭게 생성된 객체를 가리킬 것입니다. 함수 안에서 `this`의 속성을 변경시켜보면 간단하게 볼 수 있습니다.  

```ts
function Foo() {
    this.bar = 123;
}

// new 연산자로 호출
var newFoo = new Foo();
console.log(newFoo.bar); // 123
```

이제 함수에서 `new`를 호출하는 것은 함수 호출에서 반환된 새롭게 생성된 객체의 `__proto__`에 함수의 `prototype`이 할당되는 것도 알아야 합니다. 완벽한 이해를 돕는 코드가 여기 있습니다.: 

```ts
function Foo() { }

var foo = new Foo();

console.log(foo.__proto__ === Foo.prototype); // True!
```

이것이 전부입니다. 이제 바로 다음과 같은 `__extends`를 보세요. 제 맘대로 줄에 번호를 매겨놨습니다.

```ts
1  function __() { this.constructor = d; }
2   __.prototype = b.prototype;
3   d.prototype = new __();
```

이 함수를 뒤에서부터 보면 3번 줄에 `d.prototype = new __()`은 `d.prototype = {__proto__ : __.prototype}`을 의미하고 (`prototype`과 `__proto__`에 대한 `new`의 영향이기 때문에), 앞줄과 (2번 줄 `__.prototype = b.prototype;`) 하나로 합쳐져서 `d.prototype = {__proto__ : b.prototype}`이 됩니다.

그러나 잠깐, `d.prototype.__proto__`가 변경되고 예전 `d.prototype.constructor`를 유지하기를 원했습니다. 이는 첫 번째 줄(즉, `function __() { this.constructor = d; }`)으로 충분합니다. 여기에서 (호출된 함수 안에서 `this`에 대한 `new`의 영향 때문에) 사실상 `d.prototype = {__proto__ : __.prototype, d.constructor = d}`이 될 것입니다. `d.prototype.constructor`를 원상 복구했기 때문에 실제 변경한 것은 `__proto__`이므로 `d.prototype.__proto__ = b.prototype`이 됩니다.

#### `d.prototype.__proto__ = b.prototype` 중요함

중요한 것은 멤버 함수를 자식 클래스에 추가하고 기본 클래스의 다른 것들을 상속받을 수 있다는 점입니다. 다음 간단한 예제에서 알 수 있습니다.:

```ts
function Animal() { }
Animal.prototype.walk = function () { console.log('walk') };

function Bird() { }
Bird.prototype.__proto__ = Animal.prototype;
Bird.prototype.fly = function () { console.log('fly') };

var bird = new Bird();
bird.walk();
bird.fly();
```
기본적으로 `bird.fly`는 `bird.__proto__.fly`를 보고 있을 것이고 (`new`가 `bird.__proto__`가 `Bird.prototype`을 가리키게 만드는 것을 기억하세요) `bird.walk`는 (상속받은 멤버) `bird.__proto__.__proto__.walk`를 보고 있을 겁니다. (`bird.__proto__ == Bird.protytype`이고 `bird.__proto__.__proto__` == `Animal.prototype`이기 때문)
