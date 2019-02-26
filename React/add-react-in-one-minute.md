## Add React in One Minute


> Step 1 : HTML에 Dom cantainer 추가하기

우선, 수정하고자 하는 HTML 파일을 오픈합니다.
아래와 같이 React로 나타내고자 하는 부분에 &lt;div&gt; 태그를 추가합니다.

```
<!-- ... existing HTML ... -->

<div id="like_button_container"></div>

<!-- ... existing HTML ... -->
```

&lt;div&gt;태그에 고유한 **id** 속성을 부여해줍니다. 이를 통해 JavaScript 코드로부터 해당 element를 찾아내고 React 컴포넌트를 나타내도록 해줍니다.

> ##### Tip
&lt;body&gt; 태그 안이라면 어디에나 &lt;div&gt;와 같은 'container' 태그를 삽입할 수 있습니다. 또한, 한 페이지에 필요한 만큼 독립적으로 DOM container들이 포함될 수 있습니다. 해당 태그들은 보통 비어있는 상태지만ㅡ React를 통해 유효한 컨텐츠로 대체됩니다.

---

> Step 2 : Script 태그 추가하기

다음으로, 아래의 세 &lt;script&gt; 태그들을 HTML 페이지 내의 &lt;/body&gt; 바로 위에 삽입합니다.

```
<!-- ... other HTML ... -->

<!-- Load React. -->
<!-- Note: when deploying, replace "development.js" with "production.min.js". -->
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
// 위의 두 script 태그를 통해 React를 사용할 수 있습니다.

<!-- Load our React component. -->
<script src="like_button.js"></script>
// 위의 script 태그를 통해 step 1에서 추가한 div에 like_button.js 파일에서 정의한 컴포넌트를 추가할 수 있습니다.
</body>
```

---

> Step 3: React 컴포넌트 생성하기

```like_button.js``` 명 js 파일을 생성합니다.
아래 코드를 해당 파일의 내용으로 하여 저장합니다.

```
'use strict';

const e = React.createElement;

class LikeButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = { liked: false };
  }

  render() {
    if (this.state.liked) {
      return 'You liked this.';
    }

    return e(
      'button',
      { onClick: () => this.setState({ liked: true }) },
      'Like'
    );
  }
}

const domContainer = document.querySelector('#like_button_container');
ReactDOM.render(e(LikeButton), domContainer);
```

마지막 두 줄의 코드를 통해, Step 1에서 생성한 &lt;div&gt; 태그를 찾아 해당 부분에 React 컴포넌트인 "Like" 버튼이 나타나게 해줍니다.

---

> Tip : Javascript 파일 최소화하기

1. [Install Node.js](https://nodejs.org)
2. 프로젝트 폴더에서 command 창을 열고 ```$ npm init -y```
3. ```$ npm install terser```

위에서 생성한 ```like_button.js``` 파일을 최소화하기 위해, 아래 명령어를 입력합니다.

```
$ npx terser -c -m -o like_button.min.js -- like_button.js
```

---

##### References

- [Add React in One Minute](https://github.com/reactjs/ko.reactjs.org/blob/master/content/docs/add-react-to-a-website.md)
- [전체 소스 코드 보기](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605)
- [React Component 재사용하기 - id 대신 class 속성 활용](https://gist.github.com/gaearon/faa67b76a6c47adbab04f739cba7ceda)
- [Minification of JavaScript file](https://gist.github.com/gaearon/42a2ffa41b8319948f9be4076286e1f3)
