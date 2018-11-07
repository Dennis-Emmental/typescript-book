### 반복자(Iterators)

반복자 자체는 TypeScript 또는 ES6 기능은 아닙니다. 반복자는 객체 지향 프로그래밍 언어를 위한 행동 디자인 패턴입니다. 일반적으로 다음 인터페이스를 구현하는 객체입니다.

```ts
interface Iterator<T> {
    next(value?: any): IteratorResult<T>;
    return?(value?: any): IteratorResult<T>;
    throw?(e?: any): IteratorResult<T>;
}
```

이 인터페이스는 객체에 속한 집합 또는 시퀀스(sequence)에서 값을 가져올 수 있습니다.  

`IteratorResult`는 단순히 `value`+`done` 쌍입니다.: 
```ts
interface IteratorResult<T> {
    done: boolean;
    value: T;
}
```

어떤 Frame을 구성하는 Component 목록을 포함하는 Frame의 객체가 있다고 상상하세요. 다음과 같이 반복자 인터페이스로 이 Frame 객체에서 Component를 가져올 수 있습니다.:

```ts
class Component {
  constructor (public name: string) {}
}

class Frame implements Iterator<Component> {

  private pointer = 0;

  constructor(public name: string, public components: Component[]) {}

  public next(): IteratorResult<Component> {
    if (this.pointer < this.components.length) {
      return {
        done: false,
        value: this.components[this.pointer++]
      }
    } else {
      return {
        done: true
      }
    }
  }

}

let frame = new Frame("Door", [new Component("top"), new Component("bottom"), new Component("left"), new Component("right")]);
let iteratorResult1 = frame.next(); //{ done: false, value: Component { name: 'top' } }
let iteratorResult2 = frame.next(); //{ done: false, value: Component { name: 'bottom' } }
let iteratorResult3 = frame.next(); //{ done: false, value: Component { name: 'left' } }
let iteratorResult4 = frame.next(); //{ done: false, value: Component { name: 'right' } }
let iteratorResult5 = frame.next(); //{ done: true }

//It is possible to access the value of iterator result via the value property:
let component = iteratorResult1.value; //Component { name: 'top' }
```
반복하면 반복자 자체는 TypeScript 기능이 아닙니다. 이 코드는 Iterator와 IteratorResult 인터페이스를 명식적인 구현 없이도 잘 동작할 것입니다. 그러나 코드 일관성을 위해 공통 ES6 [interfaces](./types/interfaces.md)를 사용하는 것이 매우 유용합니다.   

네 좋습니다. 더 유용할 것입니다. ES6는 Iterable 인터페이스가 구현되면 [Symbol.iterator] `symbol`을 포함하는 *iteratable protocol*을 정의하고 있습니다.
```ts
//...
class Frame implements Iterable<Component> {

  constructor(public name: string, public components: Component[]) {}

  [Symbol.iterator]() {
    let pointer = 0;
    let components = this.components;

    return {
      next(): IteratorResult<Component> {
        if (pointer < components.length) {
          return {
            done: false,
            value: components[pointer++]
          }
        } else {
          return {
            done: true,
            value: null
          }
        }
      }
    }
  }
}

let frame = new Frame("Door", [new Component("top"), new Component("bottom"), new Component("left"), new Component("right")]);
for (let cmp of frame) {
  console.log(cmp);
}
```

불행하게도 이 패턴에서는 `frame.next()`는 동작하지 않을 것이고 다소 거추장스럽게 보일 것입니다. 구조를 위해 IterableIterator 인터페이스를!
```ts
//...
class Frame implements IterableIterator<Component> {

  private pointer = 0;

  constructor(public name: string, public components: Component[]) {}

  public next(): IteratorResult<Component> {
    if (this.pointer < this.components.length) {
      return {
        done: false,
        value: this.components[this.pointer++]
      }
    } else {
      return {
        done: true,
        value: null
      }
    }
  }

  [Symbol.iterator](): IterableIterator<Component> {
    return this;
  }

}
//...
```
IterableIterator 인터페이스로 `frame.next()`와 `for` 순환 모두 잘 동작합니다.

반복자는 유한한 값을 반복할 필요는 없습니다.
전형적인 예가 피보나치 수열입니다.:
```ts
class Fib implements IterableIterator<number> {

  protected fn1 = 0;
  protected fn2 = 1;

  constructor(protected maxValue?: number) {}

  public next(): IteratorResult<number> {
    var current = this.fn1;
    this.fn1 = this.fn2;
    this.fn2 = current + this.fn1;
    if (this.maxValue != null && current >= this.maxValue) {
      return {
        done: true,
        value: null
      } 
    } 
    return {
      done: false,
      value: current
    }
  }

  [Symbol.iterator](): IterableIterator<number> {
    return this;
  }

}

let fib = new Fib();

fib.next() //{ done: false, value: 0 }
fib.next() //{ done: false, value: 1 }
fib.next() //{ done: false, value: 1 }
fib.next() //{ done: false, value: 2 }
fib.next() //{ done: false, value: 3 }
fib.next() //{ done: false, value: 5 }

let fibMax50 = new Fib(50);
console.log(Array.from(fibMax50)); // [ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ]

let fibMax21 = new Fib(21);
for(let num of fibMax21) {
  console.log(num); //Prints fibonacci sequence 0 to 21
}
```

#### ES5 대상으로 반복자를 사용하는 코드 만들기
위 예제는 ES6 이상 대상을 요구하지만, 대상 JS 엔진이 `Symbol.iterator`를 지원한다면 ES5 대상에서도 작동할 수 있습니다. 이는 ES5 대상에서 ES6 lib 사용하여 (es6.d.ts를 프로젝트에 추가) 컵파일 하면 됩니다. 컴파일된 코드는 node 4+, 구글 크롬 그리고 다른 브라우저에서 동작할 것입니다.
