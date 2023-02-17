# Redux Toolkit
## COUNTER 예제
- TOOL(도구) + kit(간편하게 쓰기위한 kit) = 리덕스의 구조와 패러다임과 다르지 않다. 다만 조금만 간편해진다.
1. 리덕스 툴킷 패키지를 깐다
```
리덕스 툴킷 설치
yarn add @reduxjs/toolkit
```
2. store를 구성하는 방법
```jsx
기존 리듀서 :                               ---------->(기존에는 api를 두 개 썼다)

// const rootReducer = combineReducers({
//     //리듀서가 온다
//     counter,
// })
// const store = createStore(rootReducer);

----------------------------------------------------------------------------------------
redux toolkit :         ------> (toolkit으로 변경하면 configureStore를 써서 한 개로 바꿈)

import { configureStore } from "@reduxjs/toolkit";     ----->configureStore로 쓴다
import counter from "../../modules/reducer";
const store = configureStore({               ------>인자 안에는 객체가 들어간다.
  //리듀서가 들어간다.(모듈에서 가져온다)
    reducer: {                    -------------> 한번 더 객체 형태로 리듀서 배출
        counter: counter,
    }
})```

3. 모듈에서 기존의 리듀서 변경하기(덕스 패턴이었던 기존것을 변경)
- 리덕스 Toolkit안에 있는 createSlice라는 API를 사용 -> action creator와 reducer를 한번에 생성
```jsx
변경 전에는 리듀서가 덕스 패턴으로 action value - action creator - reducer로 구성되어있따.
```

```JSX
<toolkit으로 변경 후>
import {createSlice} from "@redux/toolkit";

const initialState = {
 number : 0,
}
//                           --------> createSlice를 통해 모든것을 한번에 만들기가 가능하다
const counterSlice = createSlice({      ------>객체가 인자로 들어간다.
    name: 'counter',                    ------>name, initialState, reducer 3개 인자 가짐
    initialState: 'initialState',
    reducer: {
        addNumber: (state, action) => {      ----->state와 action을 인자로 가져간다.
            state.number = state.number + action.payload;  -->state변경 로직
        },
        minusNumber: (state, action) => {
            state.number = state.number - action.payload;
        },
    },
})
export default counterSlice.reducer;                       ----->리듀서 만들어낸 것
export const {addNumber, minusNumber} = counterSlice.actions   -->ation creator만들어낸것
```
------>createSlice에서 얻은 결과물을 couterSlice에 저장(action creator와 reducer가 들어있다)
------> export하려면 둘 중 reducer를 보내야하니까 export default counterSlice.reducer;
------>export const {addNumber, minusNumber} = counterSlice.actions 에서 action이 의미하는 것은 addNumber와 minusNumber를 가지고 있는 객체를 의미한다.(리듀서를 의미)
==때문에 aciton creator와 reducer를 만들어낼 필요없이 API만 호출하면 된다,==

## TOdolist예제
 알고 가기
일반 redux에서 store에 있는 state를 업데이트 할때 state에 대한 새로운 값을 부여할 때는 반드시 새로운 객체를 return하도록 되어있따. -->(불변성때문에)
```jsx
const counter = (state = initialState, action) => {
  switch (action.type) {
    case ADD_NUMBER:
      return {                                   ------>이렇게 새로운 객체를 return함
        number: state.number + action.payload,
      };```
redux -toolkit을 할때는 새로운 객체가 아니라 기존에 있었던 state에 number에 직접 접근을해서 값을 plus , minus해줬다. 
```jsx
reducer: {
        addNumber: (state, payload) => {
            state.number = state.number + action.payload;     --->새 객체 안 만들고 접근
        },
```
리덕스 툴킷에서는 불변성을 관리하지 않아도 자동으로 관리할 수 있도록 알아서 변경됐다고 인지하고 렌더링을 한다 -->immer가 내장되어있어서

```jsx
<TOdolist>

const todosSlice = createSlice({
    name: 'todos',
    initialState: initialState,
    reducer : {
        addTodo: (state, action) => {
            state.push(action.payload);
        },                     -------------->//각각 key:value페어로 묶어준다
        removeTodo: (state, action) => {
            return state.filter((item) => {item.id !== action.payload})
         },
        switchTodo: (state, action) => {
            return state.map((item) => {
                if (item.id == action.payload) {
                    return {...item, isDone : !item.isDone}
                } else {
                    return item;
                }
            })
        },
    }
})
export default todosSlice.reducer
export const { addTodo, removeTodo, switchTodo } = todosSlice.action;
```

==결론 :  redux toolkit을 사용하면 좋은 점
1. store를 만드는데 configStore라는 api를 제공해서 편하게 만들었따
2.  createSlice라는 api를 통해 action creator 와 reducer를 한 번에 만드는 역할을 함
3.  불변성을 신경 안 써도 된다!!!! --->immer가 내장되어 있어서
4. DevTools가 내장되어 있어서
--------
## DevTool
 현재 프로젝트의 [state의 상태], 어떤 action이 일어났을 때 [action이 무엇]이고 [action으로 인해 state가 어떻게 변경 되었는지]를 다 파악할 수 있도록 보여주는 tool.
 -->확장 프로그램을 깔면 리덕스 툴킷으로 된 파일 오른쪽 상단에 초록색을 누르면 볼 수 있따.
 -->디버깅 파악에도 쉽다.