# z-index

CSS의 z-index속성은 `겹치는 요소의 쌓임 순서를 제어`합니다.
z-index는 position에 static이 아닌 값을 갖는 요소에만 영향을 줍니다.
  - position : CSS에서 position 속성은 `HTML 문서 상에서 요소가 배치되는 방식`을 결정합니다. 많은 경우, position 속성은 요소의 정확한 위치 지정을 위해서 `top, left, bottom, right` 속성과 함께 사용됩니다.
    - static : 기본 값으로, position 속성을 별도로 지정해주지 않으면 기본값인 static이 적용됩니다.
    position 속성이 static인 요소는 HTML 문서 상에서 원래 있어야하는 위치에 배치됩니다. 즉 작성된 순서 그대로 브라우저에 표기 됩니다. top, left, bottom, right 속성값은 position 속성이 static일 때는 무시됩니다.
    - relative : top, left, bottom, right 속성값을 기준으로 요소를 원래 위치를 기준으로 상대적(relative)으로 배치해줍니다.
    - absolute : relative 속성의 정반대 개념이라고 많이 오해하지만, position 속성이 absolute일 때 해당 요소는 `배치 기준을 자신이 아닌 상위 요소에서 찾습니다`.
    DOM 트리를 따라 올라가다가 `position 속성이 static이 아닌 첫 번째 상위 요소가 해당 요소의 배치 기준으로 설정`되고,
    만약에 해당 요소 상위에 position 속성이 static이 아닌 요소가 없다면, DOM 트리에 최상위에 있는 <body> 요소가 배치 기준이 됩니다.
    - fixed: 배치 기준이 자신이나 부모 요소가 아닌 `뷰포트(viewport), 즉 브라우저 전체화면`으로, `요소를 항상 고정된(fixed) 위치에 배치`합니다.
    - sticky: 비교적 최근에 추가된 속성값으로, 특이하게도 `브라우저 화면을 스크롤링할 때` 효과가 나타납니다.

- z-index 값이 없으면 `DOM에 나타나는 순서대로` 요소가 쌓이게 됩니다(동일한 계층에서 가장 아래의 것이 맨 위에 보여집니다).
- 정적이지 않은(non-static) 위치지정 요소(및 해당 하위 요소)는 HTML 레이어 구조와 상관없이 기본 정적 위치로 항상 요소 위에 나타납니다.

# stacking context

stacking context는 가상의 Z 축을 사용한 HTML 요소의 3차원 개념화입니다.

새로운 stacking context 는 다음 세가지 방법 중 하나로 element 에 생성 됩니다

- element가 document 의 root element 일 때
- element 가 static 이 아닌 다른 position 값을 가지고, z-index 값이 auto 가 아닐 때
- element 가 1보다 작은 opacity 값을 가질 때


https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Using_z-index
https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context
