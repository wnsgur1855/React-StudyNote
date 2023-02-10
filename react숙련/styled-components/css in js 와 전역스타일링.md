✅
#### 스타일드 컴포넌츠는 자바스크립트 언어를 사용하여 Css를 코드를 작성하는 것 

-  vsc에서 쓸 때 import를 해와야지 쓸 수 있따.
- StBox라는 변수 명을 설정하고 styled.태그명(html요소) 을 쓰고 사용할 수 있다.

```js
const StBox = styled.div`
 border : 1px solid red;
`
const StP = styled.p`
 color : blue
`
function App() {

    return (
    <StBox>바스</StBox>
    <StP>박스<StP>
    )    
}
```


- props를 통해서 스타일드 컴포넌트
```js
const StBox = styled.div`

  border: 1px solid ${(props) => props.borderColor};

  background-color: ${(props) => props.backgroundColor};

`;

function App() {

  return (

      <StBox borderColor="red" backgroundColor="green">

        바스

      </StBox>
  );
```

조건부 스타일링 한 예시
```js
const StContainer = styled.div`
  display: flex;
`;
const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid ${(props) => props.borderColor};
`;
//box의 색을 담는다
const BoxList = ['green', 'red', 'blue'];
//색을 넣으면, 이름을 반환하는 함수(switch문법을 사용하여 조건부 만듬)
const getBoxName = (color) => {
  switch (color) {
    case 'green':
      return '초록박스';
    case 'red':
      return '빨간박스';
    case 'blue':
      return '파란박스';
    default:
      return '검정박스';
  }
};
function App() {
  return (
    <StContainer>
    //여기서box는 배열의 색 하나하나를 가리키는 요소를 말한다.(색깔)
      {BoxList.map((box) => {
        //map함수를 이용하여 StBox를 반복하여 화면에 그린다
        <StBox borderColor={box}>{getBoxName(box)}</StBox>;
      })}
    </StContainer>
```
![[Pasted image 20230210182634.png]]

#CSS-in-JS의장점
 1. 자바스트립츠로 CSS를 작성하는 방식으로 원하는 조건문들을 쓸 수 있다
 2. PROPS를 통해서 부->자로 데이터를 전달 받아서 조건부 스타일링이 가능하다

---------------
✅
## GlobalStyling(전역 스타일링)
프로젝트 전체를 아우르는 스타일~!

기존의 스타일드 컴포넌트는 --> 해당 컴포넌트 내에서만 사용했다 but
컴포넌트 파일을 하나 만들고 (자식 컴포넌트) 그 안에 그 페이지를 꾸미는 Styled component가 그 페이지를 전역적으로 꾸민다. 그리고 그 TestPage를 App.jsx가 씀으로써 전역적으로 스타일링이 된다.

GlobalStyle.jsx 이것을 App.jsx에 적용한다(이 부분은 testpage의 공통된 부분을 바디에 넣어놓은 것이다)
```js
전역스타일링 :
공통적으로 적용되어야하는 것이 있으면 createGlobar Api를 이용해서 글로벌style을 적용해서 하면 좋다--->(testPage에 공통적으로 들어간 css들)

import { createGlobalStyle } from 'styled-components';--->
const GlobalStyle = createGlobalStyle`
  body {
    font-family: "Helvetica", "Arial", sans-serif;
    line-height: 1.5;    //
  }
`;
export default GlobalStyle;
```

```js
APP.jsx를 보면 전역 스타일링도 받아오고 testpage도 props 한 것을 알 수 있다.

import TestPage from './components/TestPage';
import GlobalStyle from './components/GlobalStyle';
function App() {
  return (
    <>
      <GlobalStyle />
      <TestPage title="바보" contents="내용" />;
    </> );
}
export default App;
```

✅
### 우리가 프로젝트를 만들면 기본적으로 브라우저에 제공되는 style들이 있는데 그것을 default style이라고 한다 

- (다양한 웹 브라우저들은 저마다 조금씩 다른 default style을 제공하기에 이것을 초기화하고 우리가 정한대로만 표현하는 것이 중요)
-->제거 방식 : CSS RESET 
[링크] (https://teamsparta.notion.site/02-Styled-Components-GlobalStyles-Sass-css-reset-77f8dd3d5772414e816161266ff7d4d9) 이곳에서 찾아서 쓰길 바란다
쓰는 방법 
1. reset.css파일을 하나 만든 후 
2. 거기에 저 코드들을 복사 붙여넣기한다
3. index.js파일에 import "./reset.css";  해서 파일을 import시킨다
