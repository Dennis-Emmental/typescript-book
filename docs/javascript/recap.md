# JavaScript는 TypeScript

*JavaScript* 컴파일러의 *일부 문법*에서 많은 경쟁자가 있었습니다. (그리고 앞으로도 그럴 겁니다) TypeScript는 *JavaScript가 TypeScript*라는 점에서 다릅니다. 여기에 다이어그램이 있습니다.:

![JavaScript is TypeScript](https://raw.githubusercontent.com/basarat/typescript-book/master/images/venn.png)

하지만 이는 *JavaScript를 배워야 한다*는 것을 의미합니다. (좋은 소식은 *오직 JavaScript **만** 배울 필요가 있습니다.*) TypeScript는 JavaScript에 *좋은 문서*를 제공하는 방법을 표준화합니다.

* (CoffeeScript를 보면) 새로운 문법을 제공한다고 버그를 수정하는 데 도움이 되지는 않습니다.
* (Dart를 보면) 새로운 언어를 만드는 것은 런타임과 커뮤니티로부터 멀어지게 만듭니다.

TypeScript는 그저 문서가 포함된 JavaScript입니다.

## JavaScript를 개선하기

TypeScript는 절대로 동작하지 않는 JavaScript로부터 보호하려고 노력할 것입니다. (그래서 이 부분을 기억할 필요는 없습니다):

```ts
[] + []; // JavaScript will give you "" (which makes little sense), TypeScript will error

//
// JavaScript에서 이상한 다른 것들
// - 런타임 오류를 내지 않는다. (디버깅을 어렵게 만듦)
// - 그러나 TypeScript는 컴파일 때 오류를 낼 겁니다. (디버깅이 필요 없게 함)
//
{} + []; // JS : 0, TS Error
[] + {}; // JS : "[object Object]", TS Error
{} + {}; // JS : NaN or [object Object][object Object] depending upon browser, TS Error
"hello" - 1; // JS : NaN, TS Error

function add(a,b) {
  return
    a + b; // JS : undefined, TS Error 'unreachable code detected'
}
```

본래 TypeScript는 JavaScript를 검사(lint)합니다. *타입 정보*가 없는 다른 검사기들보다 검사를 더 잘하고 있습니다.

## 여전히 JavaScript 배워야 합니다.

*JavaScript를 작성한다는* 사실에 대해 TypeScript는 매우 실용적이라고 말합니다. 그래서 방심하지 않으려면 JavaScript에 대해 알아야 할 몇 가지가 있습니다. 다음에 그것들에 관해 이야기합니다.

> 참고: TypeScript는 JavaScript의 상위 집합입니다. 컴파일러 / IDE가 사용할 수 있는 문서가 포함된 것입니다. ;)
