### 클래스
JavaScript에서 첫 번째 항목으로 클래스를 가져야 하는지 중요한 이유는 다음과 같습니다.:
1. [클래스는 유용한 구조적 추상화를 제공합니다](./tips/classesAreUseful.md)
1. 각자의 버전으로 제공하는 모든 프레임워크 (emberjs, reactjs 등) 대신 개발자가 클래스를 사용하는 일관된 방법을 제공합니다.
1. 객체 지향 개발자들은 이미 클래스를 알고 있습니다.

마지막으로 JavaScript 개발자는 *`class`를 가질* 수 있습니다. 여기 Point라는 기본 클래스가 있습니다.: 
```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```
이 클래스는 다음의 ES5 JavaScript를 생성합니다.
```ts
var Point = (function () {
    function Point(x, y) {
        this.x = x;
        this.y = y;
    }
    Point.prototype.add = function (point) {
        return new Point(this.x + point.x, this.y + point.y);
    };
    return Point;
})();
```
일급 클래스(first class) 언어 구조로서 상당히 관용적인 JavaScript 클래스 패턴입니다.

### 상속
TypeScript의 (다른 언어처럼) 클래스는 아래에 보는 것처럼 `extends` 키워드를 사용한 *단일* 상속을 지원합니다.:

```ts
class Point3D extends Point {
    z: number;
    constructor(x: number, y: number, z: number) {
        super(x, y);
        this.z = z;
    }
    add(point: Point3D) {
        var point2D = super.add(point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    }
}
```
클래스 안에 생성자가 있다면 *반드시* 생성자 안에서 부모 생성자를 호출해야 합니다. (TypeScript가 알려줄 것입니다.) `this`에 설정해야 할 내용이 설정됩니다. `super` 호출에 이어서 생성자 안에서 하고자 하는 것들을 추가할 수 있습니다. (여기에 다른 멤버 `z`를 추가했습니다.)

부모의 멤버 함수를 쉽게 오버라이드하고 (여기에서는 `add`를 오버라이드) 멤버에서 (`super` 문법을 사용하여) 여전히 부모 클래스의 기능을 사용합니다.

### Statics
TypeScript 클래스는 클래스의 모든 인스턴스 공유할 수 있는 `static` 속성을 지원합니다. 이 속성을 넣고 (그리고 접근하는) 자연스러운 위치는 클래스 자체이고 TypeScript가 하는 일입니다.:

```ts
class Something {
    static instances = 0;
    constructor() {
        Something.instances++;
    }
}

var s1 = new Something();
var s2 = new Something();
console.log(Something.instances); // 2
```

정적 함수뿐만 아니라, 정적 멤버를 가질 수 있습니다. 

### 접근 제어자
TypeScript는 아래에 보는 것처럼 `class` 멤버에 접근을 결정하는 접근 제어자 `public`, `private`와 `protected`를 지원합니다.:

| 접근 위치         | `public` | `protected` | `private` |
|-----------------|----------|-------------|-----------|
| class           | yes      | yes         | yes       |
| class children  | yes      | yes         | no        |
| class instances | yes      | no          | no        |


접근 제어자가 명시되지 않았다면 묵시적으로 JavaScript의 *편리한* 성격에 맞는 `public`입니다. 🌹

(생성된 JS로) 실생 시에는 의미가 없지만 잘 못 사용하면 컴파일할 때 오류가 발생합니다. 아래에 각각의 예제가 있습니다.:

```ts
class FooBase {
    public x: number;
    private y: number;
    protected z: number;
}

// EFFECT ON INSTANCES
var foo = new FooBase();
foo.x; // okay
foo.y; // ERROR : private
foo.z; // ERROR : protected

// EFFECT ON CHILD CLASSES
class FooChild extends FooBase {
    constructor() {
      super();
        this.x; // okay
        this.y; // ERROR: private
        this.z; // okay
    }
}
```

언제나처럼 이 제어자는 멤버 속성과 멤버 함수 둘 다 잘 동작합니다.

### Abstract
`abstract`는 접근 제어자로 생각할 수 있습니다. 앞에 언급한 제어자와 다르게 클래스의 멤버뿐만 아니라 `class`에도 사용할 수 있기 때문에 별도로 보여줍니다. `abstract` 제어자를 갖는 것은 이 기능을 *직접 호출할 수 없으며* 자식 클래스가 기능을 제공해야 함을 의미합니다.

* `abstract` **클래스**는 직접 인스턴스로 만들 수 없습니다. 대신에 `abstract 클래스`를 상속한 `클래스`를 만들어야 합니다.
* `abstract` **멤버** 는 직접 접근할 수 없으며 자식 클래스가 기능을 제공해야 합니다.

### 생성자는 선태적입니다

클래스는 생성자를 가질 필요는 없습니다. 예를 들면 다음은 완벽하게 괜찮습니다. 

```ts
class Foo {}
var foo = new Foo();
```

### 생성자 사용하여 정의

클래스에 멤버가 있고 아래처럼 이를 초기화:

```ts
class Foo {
    x: number;
    constructor(x:number) {
        this.x = x;
    }
}
```
생성자 인자에 *접근 제어자*를 붙여 자동으로 멤버가 클래스에 선언되고 생성자에서 복사되는, TypeScript에서 제공하는 약식의 일반적인 패턴이 있습니다. 앞의 예는 다시 작성될 수 있습니다. (`public x:number`를 주목):

```ts
class Foo {
    constructor(public x:number) {
    }
}
```

### 속성 초기화
TypeScript에서 지원되는 멋진 기능입니다. (실제로 ES7에서 제공) 기본값을 제공하는 데 유용하게 클래스 생성자 밖에서 클래스의 어떤 멤버도 초기화할 수 있습니다. (`members = []`를 주목)

```ts
class Foo {
    members = [];  // 직접 초기화
    add(x) {
        this.members.push(x);
    }
}
```
