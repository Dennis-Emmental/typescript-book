* [TypeScript 시작하기](#getting-시작하기)
* [TypeScript 버전](#typescript-버전)

# TypeScript 시작하기

TypeScript는 JavaScript로 컴파일됩니다. (브라우저나 서버에서) 실제로 실행되는 것은 JavasScript입니다. 그래서 다음 것들이 필요할 것입니다.

* TypeScript 컴파일러 ([소스](https://github.com/Microsoft/TypeScript/)로도 [NPM](https://www.npmjs.com/package/typescript)으로도 가능한 OSS)
* TypeScript 편집기 (원한다면 노트패드를 사용할 수 있습니다만 전 [alm 🌹](https://alm-tools.github.io/)을 사용합니다. 또한 [다른 IDE들도 많이 지원합니다.]( https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support))


![alm editor window](https://raw.githubusercontent.com/alm-tools/alm-tools.github.io/master/screens/main.png)


## TypeScript 버전

*안정적인* TypeScript 컴파일러를 사용하는 대신 이 책에서는 아직 버전 번호가 없을 수도 있는 많은 것을 보여줄 것입니다. **컴파일러 테스트 스위트(test suite)는 시간이 지남에 따라 더 많은 버그를 잡을 수 있기**때문에 야간 버전을 사용할 것을 권합니다.

명령 줄에서 다음과 같이 설치할 수 있습니다.

```
npm install -g typescript@next
```

이제 `tsc` 명령은 가장 최신이고 최고일 것입니다. 다양한 IDE 역시 지원합니다. 예를 들면

* `alm`은 항상 최신 TypeScript 버전과 같이 배포됩니다.
* vscode에 다음과 같은 내용으로 `.vscode / settings.json`을 생성하여 이 버전을 사용하도록 요청할 수 있습니다.:

```json
{
  "typescript.tsdk": "./node_modules/typescript/lib"
}
```

## 소스 코드 얻기
이 책의 소스는 책의 github 저장소 https://github.com/dennis-emmental/typescript-book-korean/tree/master/code 에서 볼 수 있습니다. 대부분의 코드 예제는 alm으로 복사하여 그대로 사용할 수 있습니다. 추가 설정이 필요한 코드 예제 (예: npm 모듈) 의 경우 코드를 보여주기 전에 코드 예제로 연결할 것입니다. 예:

`this/will/be/the/link/to/the/code.ts`
```ts
// This will be the code under discussion
```

개발 설정을 넘어 TypeScript 문법으로 넘어갑니다.
