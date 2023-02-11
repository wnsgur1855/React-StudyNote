✅
- 액션 객체를 payload를 껴서 다시 재정의 하자면
  -->action객체라는 것은 action type을 payload 만큼 처리하는 것이다
ex) payload가 3이다. plus라는 type이 들어왔을  때  3만큼을 action type해라(+해라)
		payload만큼을 type에 맞게 처리한다.

✅payload실습
사용자가 5을 더하고 싶으면 어떤 input에 5를 입력해서 버튼을 누르면 5가 더해지고, 10을 더하고 싶으면 10을 입력하고 버튼을 눌렀을 때 10이 더해지는 프로그램인 것 입니다.
지금 까지는 ~을 보내라는 식으로 목적어가 없었다면(type만 사용했을 때0),이제는  payload를 통해 목적어가 생긴것이고 목적어도 액션객체에 담아 리듀서로 같이 보내줘야하는 것이다.
```js
모듈 안
const MINUS_N = "counter/MINUS_N"; ---->type으로 쓸 변수 설정(모듈 내에서 쓸거라 export ㄴ)

export const minus_n = (payload) => { --------->type과 payload를 포함하고있는 
                                                 action creator를 만들어준다
	type : MINUS_N
	payload : payload
}

const counter = (state = initialState, action) => {
	 switch (action.type) {
		 case MINUS_N :
		 return {
		 number : state.number - actin.payload
		 }
	 }
}
```
(payload)로 input테그에서 들어오는 값이 들어온다.

```js
import {minus_n} from "./redux/modules/counter" ->리덕스 모듈의 카운터에서 minus_n을 import

<button onClick = {() => {
	dispatch(minus_n(number))
}}>
```
(number)을 쓰는 이유는 input값에 담겨있는 게 number라는  state이기때문에 (내부 컴포넌트 state이기때문에)

🍀액션객체의 활용방법으로🍀
==액션 객체엔 type이 있고 payload가 있다 payload만큼을 type에 맞게 처리한다.==

⭐⭐잊지말아야할 건 액션객체를 dispatch가 store로 던진다⭐⭐