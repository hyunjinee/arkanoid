# tsconfig.json

이 프로젝트에쓰인 tsconfig.json 파일에서 모르는 속성들에 대해 알아보자! (typescirpt-kr 의 공식문서를 읽게 되었다.)
먼저 아래는 이 프로젝트의 루트에 있는tsconfig.json 파일이다.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "esnext",
    "lib": ["ESNext", "DOM"],
    "jsx": "react",
    "sourceMap": true,
    "moduleResolution": "node",
    "baseUrl": "./src",
    "paths": {
      "~/*": ["./*"]
    },
    "typeRoots": ["src/images/"],
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*"]
}
```

디렉토리에 tsconfig.json 파일이 있다면 해당 디렉토리가 TypeScript 프로젝트의 루트가 됩니다.
tsconfig.json 파일은 프로젝트를 컴파일하는 데 필요한 루트 파일과 컴파일러 옵션을 지정합니다. 프로젝트는 다음 방법들로 컴파일됩니다:

입력 파일 없이 tsc를 호출하면 컴파일러는 현재 디렉토리에서부터 시작하여 상위 디렉토리 체인으로 tsconfig.json 파일을 검색합니다.
입력 파일이 없이 tsc와 tsconfig.json 파일이 포함된 디렉토리 경로 또는 설정이 포함된 유효한 경로의 .json 파일 경로를 지정하는 --project (또는 -p) 커맨드 라인 옵션을 사용할 수 있습니다.
커맨드 라인에 입력 파일을 지정하면 tsconfig.json 파일이 무시됩니다.

## Details

"compilerOptions" 속성은 생략될 수 있으며 생략하면 컴파일러의 기본 값이 사용됩니다.
"files" 속성은 상대 또는 절대 파일 경로 목록을 갖습니다.
"include"와 "exclude" 속성은 glob과 유사한 파일 패턴 목록을 갖습니다.
지원되는 glob 와일드카드는 다음과 같습니다:

- - 0개 이상의 문자와 매칭 (디렉토리 구분 기호 제외)
- ? 한 문자와 매칭 (디렉토리 구분 기호 제외)
- \*\*/ 반복적으로 모든 하위 디렉토리와 매칭

glob 패턴의 구분에 * 또는 . *만 있다면, 지원하는 확장자 파일만 포함됩니다 (예: 기본적으로는 .ts, .tsx 및 .d.ts이고, allowJs true로 설정되어 있다면 .js와 .jsx).

"files"과 "include" 모두 지정되어 있지 않다면 컴파일러는 기본적으로 "exclude" 속성을 사용하여 제외된 것은 제외하고 모든 TypeScript (.ts,.d.ts 그리고 .tsx) 파일을 포함하는 디렉토리와 하위 디렉토리에 포함시킵니다. allowJs가 true로 설정되면 JS 파일(.js와 .jsx)도 포함됩니다. "files"과 "include" 모두 지정되어 있다면 컴파일러는 그 두 속성에 포함된 파일의 결합을 포함합니다. "exclude" 속성이 지정되지 않으면, "outDir" 컴파일러 옵션을 사용하여 지정된 디렉토리의 파일은 제외됩니다.
"include"을 사용하여 포함된 파일들은 "exclude" 속성을 사용해 필터링할 수 있습니다.
그러나 "files" 속성을 명시적으로 사용하는 파일은 "exclude"에 관계없이 항상 포함됩니다.
"exclude" 속성에 디렉토리가 지정되지 있지 않다면 기본적으로 node_modules, bower_components, jspm_packages 그리고 <outDir>를 제외합니다.
"files" 또는 "include" 속성에 포함되어 있는 파일이 참조되는 모든 파일도 포함됩니다.
비슷하게, 파일 B.ts가 또 다른 파일 A.ts에 의해 참조된다면, 참조 파일 A.ts가 "exclude" 목록에서도 지정되지 않는 한 B.ts는 제외될 수 없습니다.

컴파일러에는 출력이 가능한 파일이 포함되어 있지 않다는 점에 주의해야 합니다; 즉 입력에 index.ts가 포함되면 index.d.ts와 index.js는 제외됩니다. 일반적으로 확장자만 다른 파일은 서로 옆에 두지 않는 것이 좋습니다.

tsconfig.json 파일은 완전히 비어둘 수 있으며, 기본 컴파일러 옵션을 사용하여 기본적으로 (위에서 설명한대로) 포함된 모든 파일을 컴파일합니다.

커맨드 라인에 지정된 컴파일러 옵션은 tsconfig.json 파일에 지정된 옵션을 오버라이드합니다.

### esModuleInterop

esModuleInterop 속성이 true로 설정될 경우, ES6 모듈 사양을 준수하여 CommonJS 모듈을 가져올 수 있게 됩니다.

해당 옵션을 통해 코드는 아래와 같이 트랜스파일링 되며, 그 결과 정상적으로 import 하는 것이 가능해집니다.

```ts
// Before
import difference from "lodash/difference";
difference();

// After
const difference = __importDefault(require("lodash/difference"));
difference();
```
