밑에 자료 첨부가 안 되는 관계로 여기에 첨부(children사용법)
![[Pasted image 20230212211751.png]]

app컴포넌트 밑에 Router을 주입해서 router안에서  element만 바꿔주면서 페이지 이동을 돕는다
```JS
<Router파일>

import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '../pages/Home';
import About from '../pages/About';
import Contact from '../pages/Contact';
import Works from '../pages/Works';      ----->각 감싼 Route들을 imort해온다

//최상단을 감싸는 껍데기:사용법 숙지만
const Router = () => {
  return (
    <BrowserRouter>                  -------->BrouswerRouter와 Routes로 Route를 감싼다
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/" element={<About />} />
        <Route path="/" element={<Contact />} />
        <Route path="/" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};
export default Router;                   ------>각 감싼 Route들을 exort한다.

```
만든 router파일을 app컴포넌트에게 적용을 시킨다.(주입시킨다,.)
```js
<App.jsx>

import Router from './shared/Router'; ------>많이하는 실수 shared를 import해야한다.

function App() {
  return <Router />;
}
```
일일이 쓰지 않고 편하게 버튼 눌러서 하려면 hook을 기억해야한다
- useNavigate(hook)
다른 페이지로 보내고자할 때 사용. (버튼을 누르면 a에서 b로 보내고싶을때)  ==html에 a태그와 유사==
```js
<어느 위치에서 어디로 보내고 싶은지 파일 지정(현재위치는 home)>

import { useNavigate } from 'react-router-dom';
const navigate = useNavigate(); ------------------->애를 통해서 이동시킨다

<button onClick={() => { navigate('/works')}}>  ----->works파일로 이동시기겠다
```

- useLocation
```
```js
  const location = useLocation();
  console.log('location', location);-->현재의 위치나 정보를 알 수 있는 유용한 정보가 있따
```
콘솔을 찍어보면
![[Pasted image 20230212153926.png]]

- Link (훅은 아니지만 꼭 알아야할 api)
html태그중 a태그의 기능을 대체하는 Api입니다(=useNavigate와 비슷)
jsx에서 a태그를 사용한다면, 반드시 Link를 사용해서 구현해야한다. ==새로고침 없애기위해==
--> a태그를 사용하면 페이지를 이동하면서 브라우저가 새로고침 되기 때문이다.
--> 새로고침 된다는 건 모든 컴포넌트가 다시 렌더링되야하고, 우리가 리덕스나 usestate를 통해 메모리상에 구축해놓은 모든 상태값이 초기화 된다는 것. -->성능 악영향

```

Link라는 api를 이용할 땐 to라는 속성값을 입력해줘야한다(어디로가야할지)
useNavigate 와 Link의 다른점은 Link는 a태그를 표방하는 것

- useParams : 현재 페이지로 넘어온 파라미터들의 정보를 알 수 있다
```js
 const params = useParams();
  console.log(params);
```

props를 전달하는 방식 2가지
1. <tag name = {name}>
2. <>어떤 값</>  -----> 어떤 값을 입력함으로써 이 값을 children으로서 받는 방법도 props의 일종

children사용법
공통 header공통navigate공통 footer을 사용할 때 사용하면 좋다
ex)
![[이미지 1.png]]
B컴포넌트는 고정을 시키고 중간컴포넌트A만 렌더링을 시키고 싶다
```JS
const Router = () => { 
return ( 
<BrowserRouter> 
	<Layout>                                                                  children을 이만큼 가지고 있따
		<Routes>                                                           ----------------------------------------
			<Route path="/" element={<Home />} />  
			<Route path="about" element={<About />}
			<Route path="contact" element={<Contact />} /> 
			<Route path="works" element={<Works />}/> 
		</Routes>                                                        -----------------------------------------
	</Layout>
</BrowserRouter> ); };

이렇게layout으로 감싸주면 된다.(Routes감싸기)--물론 Layout.js 의UI를 따로 만들고 export한 후


------