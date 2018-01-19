## 클로저

JavaScript 얻은 것 중에 최고는 클로저입니다. JavaScript에 함수는 바깥쪽에 정의된 어떤 변수도 접근할 수 있습니다. 클로저는 예제로 가장 잘 설명됩니다.:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;

    function bar() {
        console.log(variableInOuterFunction); // 바깥쪽에 있는 변수 접근
    }

    // arg 접근하는 것을 시연하는 지역 함수 호출
    bar();
}

outerFunction("hello closure"); // hello closure 출력!
```

안쪽에 있는 함수가 바깥쪽에 있는 변수에 (variableInOuterFunction) 접근할 수 있습니다. 안쪽 함수가 바깥쪽 변수를 가두었습니다. (또는 묶었습니다) 그래서 **클로저(closure)**라고 합니다. 그 자체로 개념은 간단하고 꽤 직관적입니다. 

이제 멋진 부분: *바깥쪽 함수가 반환된 후에도* 안쪽 함수는 바깥쪽의 변수에 접근할 수 있습니다. 바깥 함수에 의존하지 않고, 여전히 변수는 안쪽 함수에 엮여 있기 때문입니다. 다시 예를 살펴보겠습니다.:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;
    return function() {
        console.log(variableInOuterFunction);
    }
}

var innerFunction = outerFunction("hello closure!");

// outerFunction이 반환되었음에 주목하세요
innerFunction(); // hello closure를 출력!
```

### 왜 이것이 굉장한 이유
예로 공개 모듈 패턴같이 객체 만들기가 쉽습니다.: 

```ts
function createCounter() {
    let val = 0;
    return {
        increment() { val++ },
        getVal() { return val }
    }
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```

높은 수준에서 Node.js같은 것을 만들 수 있습니다. (지금 바로 머릿속에서 이해가 안 되더라도 걱정하지 마세요. 곧 그렇게 될 것입니다. 🌹):

```ts
// 개념 설명을 위한 의사 코드
server.on(function handler(req, res) {
    loadData(req.id).then(function(data) {
        // `res`는 가뒀고 사용 가능
        res.send(data);
    })
});
```
