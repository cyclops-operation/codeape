# CLI 만들기

<strong>CLI(Command line interface)</strong>란 명령 줄 인터페이스 또는 명령어 인터페이스라고 불리며, 커맨드 라인이라고 불리는 **텍스트 줄을 입력하여 사용자와 컴퓨터가 상호 작용하는 방식**을 의미합니다.

> [!NOTE]
>
> **Vite 프로젝트에서 `vite` 만 입력했을 때, `command not found` 가 나오는 이유**
>
> 단지 `vite` 을 입력하고 해당 명령어가 실행되려면 환경변수 `PATH` 에 해당 실행 파일이 존재해야합니다. 반면에, `npm run` 혹은 `yarn` 이후 명령어를 입력했을 때 동작하는 이유는 `npm` 혹은 `yarn` 이 수행하는 역할 덕분입니다.
>
> `npm run` 혹은 `yarn` 과 함께 명령어를 입력하는 경우 실행 파일을 찾는 경로를 해당 프로젝트 내 `node_modules/.bin` 폴더에 있는 명령 실행 파일도 함께 감지하여 실행하게됩니다. 보통 `vite` 의 경우 전역으로 설치되지 않고 해당 프로젝트 내에 지역적으로 설치되기 때문에 그냥 실행하게 된다면 동작하지 않는 것입니다.

> [!IMPORTANT]
>
> **`vite` 명령어 뜯어보기**
>
> `node_modules/vite` 폴더의 `package.json` 을 살펴보면 `bin` 이라는 프로퍼티에 `vite: "bin/vite.js"` 라고 명시되어 있음을 확인할 수 있습니다. 이어서 해당 폴더 내 `bin/vite.js` 를 확인해보면 앞서 말한 `node_modules/.bin/vite` 파일에 작성되어 있는 명령어가 동일하게 작성되어 있음을 확인할 수 있는데, 이는 `npm` 을 통해 만들어진 **심볼릭 링크**에 해당합니다.
>
> 심볼릭 링크는 글로벌로 설치하는 경우 버전 매니저가 관리하는 폴더 안에, 프로젝트 등의 내부에 종속성으로 설치하는 경우 `./node_modules/.bin` 폴더에 추가됩니다.

<br/>

## 패키지 만들어 배포하기

패키지를 만들기 위해 `package.json` 을 작성해줍니다. `npm init` 혹은 직접 `package.json` 파일을 생성하여 만들어줍니다.

```json
{
  "name": "@scope/cli", // 패키지명입니다. 별다른 명령어를 정의하지 않은 경우 스코프를 제외한 패키지명을 명령어로 가져옵니다.
  "bin": "src/cli.js", // CLI를 통해 실행할 스크립트의 위치를 의미합니다. 여기선 src/cli.js 파일을 실행합니다.
  "files": [
    "src" // npm에 배포될 파일을 정의합니다.
  ]
  // else...
}
```

패키지명이 아닌 다른 명령어로 설정하고 싶다면 `bin` 프로퍼티를 오브젝트로 변경하여 경로를 입력해주시면 됩니다.

```json
{
  "bin": {
    "do-something": "src/cli.js" // 이와 같이 작성했다면, `do-something` 명령어를 통해 실행할 수 있게 됩니다.
  }
  // else...
}
```

이후 스크립트가 작성되었다고 가정하여 NPM에 배포를 진행합니다.

```sh
npm publish --access public # `--access public` 을 통해 스코프가 있는 패키지도 공개 배포를 할 수 있다.
```

<br/>

## 파라미터 받아오기

Node.js에서는 명령줄에서 전달되는 인수를 `process.argv` 를 통해 전달받을 수 있습니다. `process.argv` 는 Node.js가 기본 제공하는 개체이며, Node.js 애플리케이션 실행 시점에 명령행으로 전달된 인수의 배열을 담고 있습니다.

```js
console.log(process.argv); // ['Node.js_실행_파일_경로', '스크립트_파일_경로', ...인수들]
```

<br/>

## 명령어 쉽게 작성하기

명령어 작성을 쉽게 하기 위해 [`commander.js`](https://www.npmjs.com/package/commander) 라는 라이브러리를 활용해봅시다.

```sh
npm install commander
```

패키지를 설치했다면 다음과 같이 명령어를 작성한다.

```js
import { Command } from "commander";

const program = new Command();

program
  .name("string-util")
  .description("CLI to some JavaScript string utilities")
  .version("0.8.0");

// `split` 명령어 정의
program
  .command("split")
  .description("Split a string into substrings and display as an array")
  .argument("<string>", "string to split")
  .option("--first", "display just the first substring")
  .option("-s, --separator <char>", "separator character", ",")
  .action((str, options) => {
    const limit = options.first ? 1 : undefined;
    console.log(str.split(options.separator, limit));
  });
```

추가로 다음의 라이브러리들을 경우에 따라 활용할 수 있습니다.

- `node-fetch`: Node.js에서 fetching을 하기 위해 활용하는 라이브러리
- `open`: 파일 혹은 앱을 열 수 있는 명령어 라이브러리
