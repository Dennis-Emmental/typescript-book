### for...of
시작하는 JavaScript 개발자가 경험하는 공통적인 에러는 배열에 대한 `for...in`은 배열의 항목을 순회하지 않는다는 것입니다. 대신 주어진 객체의 *키*를 순회합니다. 다음 예제에서 설명합니다. `9,2,5`를 기대하지만 인덱스 `0,1,2`를 얻습니다.:

```ts
var someArray = [9, 2, 5];
for (var item in someArray) {
    console.log(item); // 0,1,2
}
```

이는 TypeScript에 (그리고 ES6) `for...of`가 존재하는 이유 중의 하나입니다. 다음은 기대한 것처럼 배열을 정확하게 순회하면서 구성원들을 출력합니다.

```ts
var someArray = [9, 2, 5];
for (var item of someArray) {
    console.log(item); // 9,2,5
}
```

비슷하게 `for...of`를 사용하여 문자열을 글자별로 처리하는 데 TypeScript는 문제가 없습니다.

```ts
var hello = "is it me you're looking for?";
for (var char of hello) {
    console.log(char); // is it me you're looking for?
}
```

#### JS 생성
pre ES6 대상으로 TypeScript는 표준 `for (var i = 0; i < list.length; i++)` 반복문을 생성할 것입니다. 예를 들어 앞의 예제에 대해서 생성된 것은 다음과 같습니다.:
```ts
var someArray = [9, 2, 5];
for (var item of someArray) {
    console.log(item);
}

// becomes //

for (var _i = 0; _i < someArray.length; _i++) {
    var item = someArray[_i];
    console.log(item);
}
```
보는 것처럼 `for...of` 사용은 *의도를* 명확하게 하고 작성하는 코드의 양을 (그리고 작성하는 변수 이름도) 줄여줍니다.  

#### 제약사항
ES6 또는 그 이상을 대상으로 하지 않으면, 생성된 코드는 객체에 `length` 속성이 존재하고 객체는 `obj[2]`처럼 숫자로 색인 될 수 있다고 가정합니다. 예전 JS 엔진에 대해서는 `string`과 `array`만 지원합니다.

TypeScript가 배열 또는 문자열을 사용하지 않는 것을 본다면 명확한 에러 *"is not an array type or a string type"*를 줄 것입니다.; 
```ts
let articleParagraphs = document.querySelectorAll("article > p");
// Error: Nodelist is not an array type or a string type
for (let paragraph of articleParagraphs) {
    paragraph.classList.add("read");
}
```

배열 또는 문자열인지 *아는* 것에만 `for...of`를 사용하세요. 이 제약은 TypeScript의 미래 버전에서 제거될 수 있습니다.

#### 정리
배열의 항목을 얼마나 많이 순회할 것인지에 놀랄 것입니다. 다음번에는 배열을 순회할 때 `for...of`를 사용하세요. 작성한 코들 리뷰하는 다음 사람을 행복하게 만들 것입니다.
