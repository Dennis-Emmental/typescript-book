#### `super`

자식 클래스에서 `super`를 호출하면 아래에 보이는 것처럼 `prototype`으로 전달됩니다.

```ts
class Base {
    log() { console.log('hello world'); }
}

class Child extends Base {
    log() { super.log() };
}
```
다음 코드를 생성합니다.:

```js
var Base = (function () {
    function Base() {
    }
    Base.prototype.log = function () { console.log('hello world'); };
    return Base;
})();
var Child = (function (_super) {
    __extends(Child, _super);
    function Child() {
        _super.apply(this, arguments);
    }
    Child.prototype.log = function () { _super.prototype.log.call(this); };
    return Child;
})(Base);

```
`_super.prototype.log.call(this)`를 주목하세요.

멤버 속성에서 `super`를 사용할 수 없음을 의미합니다. 대신에 `this`를 사용해야 합니다.

```ts
class Base {
    log = () => { console.log('hello world'); }
}

class Child extends Base {
    logWorld() { this.log() };
}
```

`Base`와 `Child` 클래스 사이에서 `this`만 공유되기 때문에 *다른* 이름을 사용할 필요가 있습니다. (여기에서는 `log`와 `logWorld`)

또한 TypeScript는 `super`를 잘못 사용하면 경고할 것입니다.:

```ts
module quz {
    class Base {
        log = () => { console.log('hello world'); }
    }

    class Child extends Base {
        // ERROR : only `public` and `protected` methods of base class are accessible via `super`
        logWorld() { super.log() };
    }
}
```
