# 리액트의 리덕스에서 컴포넌트에서 state를 변경하는 방법이 dispatch를 통해서 action객체를 store에 던져준다

ex) onclick이 생겨서 버튼을 누르면 +1이 돼야 한다면 store에 있는 값이 1이 증가돼야한다.

---------------------------
✅순서

ui에 어떤 이벤트가 발생 
-> store에 있는 state가 바껴야된다고 요청이 들어오면 
-> Dispatch라는 놈이 그 요청을 수행함 
-> ==Dispatch라는 놈이 action객체를 가지고 store로 방문한다.== 
*action이 객체다 => key : value 형태이다(type, payload)*
= action을 store로 던지는 놈이 dispatch 
--> action에 있는 type에 따라서 reducer가 state를 제어함
![[Pasted image 20230212010154.png]]

✅counter + 1, -1실습

기존에 우리는 useState를 사용해 state를 변경 시켜줬다
redux에서는 값의 수정이 리듀서에서 일어난다!!

1. 리듀서에게,  (number + 1)을 할 것이라는 명령을 만든다
```js
const initialState = {
  number: 0,
};
-----리듀서부분-----------------------------------------------------------------------

const counter = (state = initialState, action) => {
console.log(state)           -------->콘솔찍는 법

  switch (action.type) {     --------->action객체의 type이라는 key를 보낼거다
                                        리듀서는 그 type의 key를 보기때문에
    case 'PLUS_ONE':
      return {
        number: state + 1,  ----------> 객체형태이므로 {}안에 싸서 state +1
      };
    case 'MINUS_ONE':
      return {               ---------> 객체형태이므로 {}안에 싸서 state +1
        number: state - 1,
      };
    default:
      return state;
  }
}; 
export default counter;
```
2. 명령을 보낸다(명령을 보내기 위해서 필요한 hook인 [useDispatch적용)]
   ++dispatch를 이용해서 액션객체를 리듀서로 보낼 수 있따.
```js
const dispatch = useDispatch() -------->useDispatch훅 사용을 하겠다. ()있으면 함수이다
                                                                      ()붙여서 함수 실행

<button onClick = {() =. {
	dispatch({type : 'PLUS_ONE'}) -----------> action객체 형태이기에 {}싸고 type을
}}
```
3. 리듀서에서 명령을 받아 number +1 을 한다.

===> 우리는 명령을 action이라고 정할거다 ===action객체==로 표현

-------------
✅ 위의 counter코드를 리펙토링해보자

# Action creator  (말 그대로 액션객체를 변수화)
= ⭐[액션객체를 만드는 함수(모듈파일 안에서 생성)]⭐
액션객체의 value를 변경할 일이 생긴다면 PLUS_ONE이라는 value대신 이 액션 객체가 counter모듈안에 있따는 것을 강조하기 위해서 PLUS_ONE --> counter /PLUS_ONE 이라는 value로 바꾸길 원한다면 ==리듀서 와 dispatch== 부분들을 군데군데 바꿔줘야한다. 하지만 프로젝트 규모가 1000군데라면 곤란하다,
--> 그래서 앞으로 하드코딩하는 것이 아니라 액션객체를 한 곳에서 관리할 수 있도로고 "함수"와 액션value를 상수로 만들어 보겠다.

= ⭐한 마디로 Action객체를 변수화 시켜주고 그것들을 import 와 export를 적절히 시켜준 뒤 재활용하게 만드는 것이다 
```js
<모듈파일 안>
액션 value

const PLUS_ONE = "PLUS_ONE"         ----------->이 모듈 내에서 쓰기때문에 export ㄴㄴ
const MINUS_ONE = "MINUS_ONE"       ----------->이 모듈 내에서 쓰기때문에 export ㄴㄴ
----------------------------------------------------------------------------------------
액션 creator

export const plusOne = () => {
	return {
	 type : Plus_ONE,             ----------->⭐export해서 리듀스에 보내야한다
	}                              -----------> ⭐return을 객체 상태로 넣었기 때문에
};                                              ⭐ =밑의 예시처럼 dispatch(plusOne())
export const minus_One = () => {
	return {                       ---------->export해서 리듀스에 보내야한다
	 type : MINUS_ONE,
	}
};
```

```js
App.js

import {PLUS_ONE, MINUS_ONE} from "./redux/modules/counter";
import {PLUS_ONE, MINUS_ONE} from "./redux/modules/counter";

<button onclick = {() => {
		dispatch(plusOne())      ---->⭐plusOne상수(객체 상태) 안에 PLUS_ONE이 들어가 있고 
}}

<button onClick = {() => {
		dispatch(minus_One())
}}
```

==액션 객체를 쓰는 순간 액션value는 안 써도 된다==

✅ 액션객체 사용이유
1. 휴먼에러(오타방지)
2. 유지보수의 효율 증가
3. 코드 가독성