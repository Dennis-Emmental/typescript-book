* [화살표 함수](#화살표-함수)
* [팁: 화살표 함수의 필요](#팁-화살표-함수의-필요)
* [팁: 화살표 함수의 위험](#팁-화살표-함수의-위험)
* [팁: `this`를 사용하는 라이브러리](#팁-this를-사용하는-라이브러리와-화살표-함수)
* [팁: 화살표 함수와 상속](#팁-화살표-함수와-상속)
* [팁: 빠른 객체 반환](#팁-빠른-객체-반환)

### 화살표 함수

애칭으로 *굵은 화살*이라 부르고 (`->`는 얇은 화살이고 `=>`는 굵은 화살) *람다 함수*라고도 불린다. (다른 언어의 영향) 일반적으로 사용되는 기능은 굵은 화살 함수 `() => something`입니다. *굵은 화살*의 동기는 다음과 같습니다.:
1. `function`을 입력할 필요가 없습니다.
1. `this`의 의미를 내포하고 있습니다.
1. `arguments`의 의미를 내포하고 있습니다.

함수형이라고 주장하는 언어인 JavaScript에서 `function`을 상당히 많이 입력하는 경향이 있습니다. 굵은 화살표는 함수 만드는 것을 쉽게 만들어줍니다. 
```ts
var inc = (x)=>x+1;
```
JavaScript에서 `this`는 전통적으로 아픈 지점이었습니다. "너무 쉽게 `this`의 의미를 잃어버려서 JavaScript가 싫다."라고 말하는 똑똑한 이도 있었습니다. 굵은 화살표는 주변 문맥의 `this`의 의미를 내포함으로 이를 개선합니다. 다음 순수 JavaScript 클래스를 보세요.

```ts
function Person(age) {
    this.age = age;
    this.growOld = function() {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 1, should have been 2
```
브라우저에서 이 코드를 수행시키면 `window`가 `growOld` 함수를 실행하기 때문에, 함수 안의 `this`는 `window`를 가리킵니다. 화살표 함수를 사용해서 수정합니다.:
```ts
function Person(age) {
    this.age = age;
    this.growOld = () => {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```
이 코드가 잘 동작하는 이유는 화살표 함수에 의해서 함수 밖의 `this`에 대한 참조를 잡아 두기 때문입니다. 이는 (TypeScript가 없다면 이렇게 작성했을) 다음 JavaScript 코드와 같습니다.
```ts
function Person(age) {
    this.age = age;
    var _this = this;  // capture this
    this.growOld = function() {
        _this.age++;   // use the captured this
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```
TypeScript를 사용하기 때문에 구문이 더 이쁘고 클래스와 화살표를 조합할 수 있습니다.
```ts
class Person {
    constructor(public age:number) {}
    growOld = () => {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```

> [A sweet video about this pattern 🌹](https://egghead.io/lessons/typescript-make-usages-of-this-safe-in-class-methods)

#### 팁: 화살표 함수의 필요
간결한 문법 이외에 호출될 함수를 넘겨 주려고 한다면 굵은 화살표를 *사용해야* 합니다.
```ts
var growOld = person.growOld;
// Then later someone else calls it:
growOld();
```
직접 호출을 한다면, 즉
```ts
person.growOld();
```
`this`는 정확히 호출한 문맥을 (이 예에서는 `person`) 가리킬 것입니다.

#### 팁: 화살표 함수의 위험

사실 `this`가 *호출 문맥이 되기를* 원한다면 *화살표 함수를 사용하지 않아야* 합니다. jquery, underscore, mocha 및 다른 라이브러리에서 사용하는 콜백의 경우입니다. 문서에 `this`에 대한 함수가 언급되어 있다면, 아마 굵은 화살표 대신 `function`을 사용해야 할 것입니다. 비슷하게 `arguments`를 사용할 예정이라면 화살표 함수를 사용하지 마십시오. 

#### 팁: `this`를 사용하는 라이브러리와 화살표 함수
많은 라이브러리가 반복 처리 중인 객체에 `this`를 전달하는 `jQuery`의 순회 같은 (하나의 예로 http://api.jquery.com/jquery.each/)것을 합니다. 이 경우 둘러싼 문맥뿐만 아니라 라이브러리에 전달된 `this`에 접근하려고 한다면 화살표 함수가 없는 것처럼 `_self`같은 임시 변수를 사용하십시오. 

```ts
let _self = this;
something.each(function() {
    console.log(_self); // the lexically scoped value
    console.log(this); // the library passed value
});
```

#### 팁: 화살표 함수와 상속

화살표 함수를 인스턴스 메소드로 가지고 있다면 `this`는 이어집니다. `this`는 단 하나이기 때문에 그러한 함수는 `super`호출에 참여할 수 없습니다. (`super`는 프로토타입 멤버에만 동작합니다.) 자식에서 덮어쓰기 전에 메소드의 복사본을 만들어 쉽게 처리할 수 있습니다.

```ts
class Adder {
    constructor(public a: number) {}
    // This function is now safe to pass around
    add = (b: number): number => {
        return this.a + b;
    }
}

class ExtendedAdder extends Adder {
    // Create a copy of parent before creating our own
    private superAdd = this.add;
    // Now create our override
    add = (b: number): number => {
        return this.superAdd(b);
    }
}
```

### 팁: 빠른 객체 반환

때로는 단순 객체 그대로를 반환하는 함수가 필요합니다. 그러나 다음의 경우는

```ts
// WRONG WAY TO DO IT
var foo = () => {
    bar: 123
};
```
JavaScript 실행기는 *JavaScript 레이블*을 포함한 *블록*으로 해석합니다. (JavaScript 스펙 때문에)

> 걱정 마세요. 이해가 안 된다면 TypeScript에서는 "사용되지 않는 레이블"이라고 멋진 컴파일러 에러가 발생합니다. 지금의 *GOTO는 나쁘다고 간주하는* 숙련된 개발자로 레이블은 무시할 수 있는 낡은 (거의 사용되지 않음) JavaScript 기능입니다.  

객체를 `()`로 감싸는 것으로 고칠 수 있습니다.:

```ts
// Correct 🌹
var foo = () => ({
    bar: 123
});
```
