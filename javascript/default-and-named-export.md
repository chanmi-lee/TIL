# export in ES6

---

> `export` 문은 JavaScript module에서 함수, 객체, 원시 값을 내보낼 때 사용합니다.

`export`는 ES6에서 정의된 API로, 브라우저에서 기본적으로 제공되지 않기 때문에 Babel, webpack 등과 같은 별도의 트랜스파일러와 함께 사용되어야 합니다.

`export`는 default export 와 named export 두 가지 방법이 있습니다.

### default export
- 한 모듈에서 하나의 `export`만 가능
- `import` 할 때는 어떤 이름으로도 가져올 수 있다

*example #1*
```
// test.js
let k;

export default k = 12;
```

```
// show.js
import m from './test'; // k가 default export 이므로, import 시 k 대신 m 등의 다른 이름을 사용 가능
```

*example #2*
```
// my-module.js
export default function cube(x) {
  return x * x * x;
}
```

```
import cube from './my-module.js';
console.log(cube(3)); // 27
```

### named export
- 한 모듈에서 여러 개의 `export` 가능
- `import` 할 때는 내보낸 이름과 동일한 이름을 사용해야 한다

```
// my-module.js
function cube(x) {
  return x * x * x;
}

const foo = Math.PI + Math.SQRT2
const graph = {
  options: {
    color: 'white',
    thickness: '2px'
  },
  draw: function() {
    console.log('From graph draw function');
  }
}

export { cube, foo, graph };
```

```
import { cube, foo, graph } from './my-module.js';

graph.options = {
  color: 'blue',
  thickness: '3px'
}

graph.draw();
console.log(cube(3)); // 27
console.log(foo); // 3.14 * 1.4 = 4.559
```

---

**Reference**

- [MDN: export](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export)
- [MDN: Math.SQRT2](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/SQRT2)
