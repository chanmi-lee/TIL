## React

지속적으로 데이터가 변화하는 대규모 어플리케이션 구축하기

> Virtual DOM

리액트의 주요 특징 중 하나는 Virtual DOM을 사용하는 것입니다.

DOM은 Document Object Model의 약어로, 객체로 문서 구조를 표현하는 방법으로 XML 혹은 HTML로 작성합니다.  웹 브라우저는 DOM을 활용하여 객체에 자바스크립트와 CSS를 적용하는데, DOM은 트리 형태라서 특정 node를 찾거나 수정하거나 제거하거나 원하는 곳에 삽입할 수 있습니다.

DOM은 동적 UI에 최적화되어있지 않다는 점이 단점인데, 리액트는 DOM을 최소한으로 조작하여 작업을 처리하는 방식을 사용합니다. 즉, Virtual DOM 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행합니다.

1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링하비다.
2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교합니다.
3. 바뀐 부분만 실제 DOM에 적용합니다.


> **V**(iew) 만 담당

다른 웹 프레임워크가 Ajax, 데이터 모델링, 라우팅 등 기능을 내장하고 있는 반면, 리액트는 정말 뷰만 신경쓰는 라이브러리이므로 기타 기능은 직접 구현 혹은 이미 만들어진 라이브러리를 추가로 붙여 사용해야 합니다.
즉, 라우팅에는 react-router, Ajax 처리에는 axios나 fetch, 상태관리에는 redux나 MobX를 사용합니다.

---

#### Prerequisite

> Node.js와 npm

[Installation for Windows](https://nodejs.org/en/download/)

설치가 끝나면 terminal을 열고, 제대로 설치 되었는지 확인합니다.
```
$ node -v
```

Node.js는 크롬 V8 자바스크립트 엔진으로 빌드한 자바스크립트 런타임으로, 웹 브라우저 환경이 아닌 곳에서도 자바스크립트를 사용하여 연산할 수 있습니다.

리액트 애플리케이션은 웹 브라우저에서 실행되는 코드이므로 Node.js와 직접적인 연관은 없지만, **프로젝트를 개발하는 데 필요한 주요 도구들이 Node.js를 사용** 하기 때문에 설치해줍니다.

ex) ES6을 호환시켜주는 babel, 모듈화된 코드를 한 파일로 합치고 (번들링) 코드를 수정할 때마다 웹 브라우저를 리로딩하는 등 여러 기능을 지닌 webpack 등이 있다.


> yarn

##### Windows

1. [Installer로 설치하기](https://yarnpkg.com/en/docs/install#windows-tab)
2. Chocolatey를 통해 설치하기
```
$ choco install yarn
```
3. Scoop을 통해 설치하기
```
$ scoop install yarn
```

설치가 끝나면 terminal을 열고, 제대로 설치 되었는지 확인합니다.
```
$ yarn --version
```

npm은 의존하는 라이브러리 개수가 많으면 속도가 매우 저하되고, 의존하는 버전이 설치되는 시점을 기준으로 결정하기 때문에 설치하는 시기에 따라 다른 버전을 설치할 가능성이 있습니다.

yarn 도구는 npm 문제점을 개선한 패키지 매니저로 npm을 대체할 수 있으며, 훨씬 빠르게 설치할 수 있습니다.


> Editor 설치

자주 사용하는 에디터로는 Sublime Text, Bracket, VS Code, Atom 등이 있다.
이 중, 모든 OS를 지원하는 [VS Code](https://code.visualstudio.com/Download)를 내려받아 진행한다.

또한, 함께 사용하면 유용한 확장 프로그램 목록은 다음과 같다.
1. [ESLint](https://eslint.org/) : 자바스크립트 문법 체크
2. Relative Path : 상대 경로에 있는 파일 경로를 편하게 작성할 수 있는 도구
3. Guides : 들여쓰기 가이드라인을 그려줌
4. React-beautify : 리액트 코드를 정리


> create-react-app 으로 프로젝트 생성

```
$ yarn global add create-react-app
// 또는
$ npm install -g create-react-app
```
create-react-app 도구는 npm 또는 yarn으로 설치할 수 있습니다.
패키지는 지역적(local) 혹은 전역적(global)으로 설치 가능하며, 둘의 차이는 프로젝트 디렉터리에서만 사용하느냐  혹은 모든 디렉터리에서 사용하느냐 입니다.

```
$ create-react-app hello-react

...
Success! Created hello-react at /Users/chanmi-kate-lee/dev/hello-react
Inside that directory, you can run several commands:

  yarn start
    Starts the development server

  yarn build
    Bundles the app into static files for production

  yarn test
    Starts the test runner

  yarn eject
    Removes this tool and copies build dependencies, configuration files and scripts into the app directory. If you do this, you can't go back!
```

정상적으로 리액트 프로젝트가 생성된 후, yarn start 명령어로 프로젝트 개발 서버를 실행하면 http://localhost:3000 에서 확인 가능하다
