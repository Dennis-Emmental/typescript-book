### 템플릿 문자열
문법적으로 단일 (') 또는 쌍 (") 따옴표 대신 역 따옴표 (\`)를 사용하는 문자열입니다. 템플릿 문자열의 동기는 다음 세 가지입니다.:

* 문자열 삽입
* 여러 줄 문자열
* 태그가 붙은 템플릿

#### 문자열 삽입
또 다른 공통 사용 사례는 정적 문자열 + 변수에서 문자열을 생성하기를 원하는 경우입니다. 이를 위해 *템플릿 논리*가 필요하고 여기에서 *템플릿 문자열* 이름을 가져왔습니다. 이전에 html 문자열을 생성하는 방법이 여기에 있습니다.: 

```ts
var lyrics = 'Never gonna give you up';
var html = '<div>' + lyrics + '</div>';
```
이제 템플릿 문자열을 사용하면 다음처럼 할 수 있습니다.: 

```ts
var lyrics = 'Never gonna give you up';
var html = `<div>${lyrics}</div>`;
```

중간 삽입 (`${` 과 `}`) 안의 자리 표시자는 JavaScript 표현으로 취급되고 처리됩니다. 예를 들어 근사한 계산을 할 수 있습니다.  

```ts
console.log(`1 and 1 make ${1 + 1}`);
```

#### 여러 줄 문자열
JavaScript 문자열에 개행 문자를 넣기를 원한 적이 있었습니까? 아마도 가사를 포함하기를 원하셨었나요? 우리가 좋아하는 이스케이프 문자 `\`를 사용하여 *문자 그대로의 개행 문자를 피하고* 다음 줄에 `\n` 문자열을 직접 넣어야 했었습니다. 이는 아래와 같습니다.:

```ts
var lyrics = "Never gonna give you up \
\nNever gonna let you down";
```

TypeScript에서는 템플릿 문자열을 사용할 수 있습니다.: 

```ts
var lyrics = `Never gonna give you up
Never gonna let you down`;
```

#### 태그가 붙은 템플릿

템플릿 문자열 앞에서 (`태그`라 부르는) 함수를 놓을 수 있습니다. 그리고 템플릿 문자열의 문자들과 위치 표시자의 모든 값을 전처리하고 결과를 얻을 기회를 가집니다. 몇 가지 메모:
* 모든 정적인 문자들은 첫 인자에 배열로 전달됩니다.
* 모든 위치 표시자 표현의 값은 남은 인자들로 전달됩니다. 가장 일반적으로 나머지 인자를 사용하여 배열로 전달할 수 있습니다.

모든 위치 표시자에서 html의 특수 문자 처리를 하는 (`htmlEscape` 이름의) 태그 함수 예제가 있습니다.:

```ts
var say = "a bird in hand > two in the bush";
var html = htmlEscape `<div> I would just like to say : ${say}</div>`;

// a sample tag function
function htmlEscape(literals, ...placeholders) {
    let result = "";

    // interleave the literals with the placeholders
    for (let i = 0; i < placeholders.length; i++) {
        result += literals[i];
        result += placeholders[i]
            .replace(/&/g, '&amp;')
            .replace(/"/g, '&quot;')
            .replace(/'/g, '&#39;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;');
    }

    // add the last literal
    result += literals[literals.length - 1];
    return result;
}
```

#### 생성된 JS
ES6 이전 컴파일 대상의 코드는 매우 단순합니다. 여러 문자열은 이스케이프 문자열이 됩니다. 문자열 삽입은 *문자열 연결*이 됩니다. 태그가 붙은 템플릿은 함수 호출이 됩니다.

#### 정리
여러 줄 문자열과 문자열 삽입은 어떤 언어에서든 있는 좋은 것입니다. JavaScript에서 이것들을 사용할 수 있다면 좋은 것입니다. (TypeScript 고맙습니다!) 태그가 붙은 템플릿은 강력한 문자열 도구를 만들 수 있게 합니다.
