✅store를 생성했으니 연결(조회)를 해야겠죠

++modules에는  state묶음들을 집어 넣는다

modules안에 state파일을 작업한다
```js
1,2,3 번 최종본

const initialState = {
  number: 0,            --------->초기값이 객체형태
const counter = (state = initialState, action) => {
  switch (action.type) {
   default:
      return state;
  }
};
export default counter;
```

1. 초기 상태값이 필요하므로 useState에서 초기값을 0으로 설정한 것처럼 우선 초기값을 0으로 설정한다.
```js
✅초기값이 여러 형태일경우
0일때 
=> const initialState = 0;

0이 있는 배열일때 
=> const initialState = [0];

number = 0, name ="준혁"인 객체일 때 ------------> 현재 객체니까 이 부분에 해당
=> const initialState ={ 
     number : 0,
     name: "준혁",
}
```

2. 리듀서를 쓴다.
--> ==리듀서는 state의 변화를 일으키는 함수이다.== (인자)를 받고 어떤 역할을 수행하는 함수!
(input)값에 state와 action을 이렇게 두 개의 인자를 받는다
여기서는 state를 action의 type에 따라 변경하는 함수이다.-->action은 state를 어떻게 변경,수정할건지를 말한다.
state에 initialState를 할당해줘야한다.(초기값)
```js
const counter = (state = initialState, action) => {
  switch (action.type) {
   default:
      return state;
  }
```

3. 리듀서를 만들었으니 export를 시켜서 counter을내보낸다
```js
export default counter;
```

4. counter모듈을 store에 연결한다
```js
const rootReducer = combineReducers({
 counter : counter   -------> 이곳은 리듀서를 넣는 부분이므로 위에서 만든 리듀서를 넣어줌
})
```

5. store와 모듈이 연결 됐는지 확인한다--> 컴포넌트에서 store에 접근하여 counter를 직접 조회하면 된다.
 useSelector(store조회)라는 redux훅을 사용한다.
 여기서 인자로 들어가는 state는 ==중앙 저장소 안에 있는 state 전체를 말한다==
 ⭐⭐기억할 것 : ()를 하면 이 훅을 호출한다는 의미⭐⭐
 ```js
 import {useSelector} from "react-redux"; ----->useSelctor을 임포트해서 사용

 function App() {
 const data = useSelector((state) => {
 return state;
 })
 console.log('data',data)
 }
```
✅console.log('data',data)를 찍어보면 state 즉 스토어를 조회해 볼 수 있다
    이를 보면 store의 리덕스 값을 가져온다는 것을 알 수 있다.
![[Pasted image 20230212003058.png]]

✅만약 여러개의 리듀서중 counter의 정보만을 가져오고 싶을 때
```js
const counter = useSelector((state) => {
	return state.counter;
})
console.log('counter', counter)
```
![[Pasted image 20230212003248.png]]
이렇게 나온다.