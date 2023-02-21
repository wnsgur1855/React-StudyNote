# Thunk
thunk는 비동기 통신을 위한 미들웨어이다
![](https://i.imgur.com/jxo9cVg.png)

리덕스에서 dispatch를 하면 action이 리듀서로 전달되고, 리듀서는 새로운 state를 반환한다. 미들웨어를 사용하면 이 과정 사이에 우리가 하고 싶은 작업들을 넣어서 할 수 있다.
`counter프로그램에서 plus버튼을 눌렀을 때 바로 1을 더하지 않고 3초를 기다렸다가, +1이 되도       록 구현하려면 미들웨어를 사용하지 않고서 구현이 불가능`

- 미들웨어를 사용하는 이유?
==--> 서버와 통신을 위해 사용한다.==

- 많이 사용되는 미들웨어 종류>
--> Thunk 와 saga 와 react 쿼리

thunk를 사용하면 dispatch할 때 ==객체가 아닌 함수를 dispatch==할 수 있게 해준다.
--> 함수 안에서 어떤 동작을 실행할 수 있따. (3초를 기다릴 수 있게 한다.)

- thunk함수를 만들기 : createAsyncThunk 라는 api
-->reduxToolkit 내장 API
- extraReducer(createSlice안에 있는)를 수정해줌으로써 거기에 thunk를 등록

- dispatch하기(이전과 똑같은데 이 안에 함수를 넣는다)
```
<이해의 신>
디슷패치를 사용해서 액션 크리에이터를 이 안에서 호출 그리고 리듀서로 보냄(action)
액션크리에이터를 호출했다는 건 ()이 안에 액션 객체가 들어있다는 얘기
```

### counter 3초 기다리고 더해주기 실습

- 1. 리듀서.js 파일 안에 createAsyncThunk라는 api를 쓴다
청크를 쓰는 이유가 함수 안에서 3초를 기다리는 등의 역할을 하기 위해서, 비동기 함수를 수행하기위해(즉 서버 요청을 하기 위해서 함수부분을 사용한다.)
2개의 input이 들어가는데 <<1. 이름(암거나) 2. 함수>>
함수에는 인자가 두 개 들어가는데
1. 컴포넌트에서 보내 줄(쭌) 페이로드
2. thunk에서 가지고 있는 내장 기능을 가지고 있는 객체
`thunkAPI이놈 안에 dispatch라는 놈이 있는데 => thunkAPI.dispatch이놈이 우리가 이전에 썼었던 컴포넌트에서 디스패치를 호출했었던 거와 동일하다. 그래서 여기에서 컴포넌트로부터 넘겨받았던 payload를 그대로 전달 해주기만 하면 똑같이 동작한다. 다만 가운데서 3초를 기다리게 하는 역할을 추가했음`
```jsx
counterSlice.jsx

import {createAsyncThunk} from "@reduxjs/toolkit"

export const __addNumber = createAsyncThunk(   ------> api호출 & __addNumber로 청크쓰기
	"ADD_NUMBER_WAIT",
	 (payload, thunkAPI)=> {
	 //수행하고 싶은 동작을 넣는다 : 3초를 기다리게 할 예정
	 setTimeout(()=> {
     thunkAPI.dispatch(addNumber(payload))->  addNumber()가있어야(액션크레이터가 있어야)  
	 },3000)----------->3초입니다               전체()부분이 액션 객체로 바뀐다->dispatch
	 }                                                    정상호출()
)}                                       ==>액션크레이터를 이용해서 다시 dipatch를 호출

const initialState = {
nunber : 0,
}

const conterSlice = createSlice({
name : "counter",
initialState,
reducers : {
	addNumber: (state, action) => {
	state.number = state.number + action.payload}-->여기로 100이라는 값이 들어가며
}                                                  state가 업데이트 되는데 3초 후에!!
})
```

app.jsx에서 (__addNumber) 을  import해서
흐름 : 
1. +number로  에드넘버가 들어온다.
2. 리듀서.js의 페이로드 자리(payload)로 에드넘버에 넣어줬던 넘버값이 들어오게 된다.
3.  들어와서 3초 후에 밑의 action.payload에 값이 들어간다.
4. counterSlice 툴킷 리듀서의 action.payload에 100(input에 넣은 수)이라는 값이 들어가면서 state가 업데이트가 되는 것이다. ----> 다만 여기서는 3초를 기다리게 하는 것
```jsx
app.jsx

import {__addNumber} from "./redux/modules/counterSlice"   --->IMPORT하고

function App() {
const globalNumber = useSelector((state) => state.counter.number)
const [number, setNumber] = useState(0);

const dispatch = useDispatch();

const onClickAddNumberHandler = () => {
dispatch(__addNumber(+number));     ------------->이것만 바꿔주면 장땡
}
}
```

==>액션이 리듀서로 전달되기 전에 [중간]에 어떤 작업을 더 할 수 있다.
==> thunk를 사용하면, 객체가 아닌 함수를 dispatch할 수 있게 해준다.
==> 리덕서 툴킷에서 청크함수를 생성할때는 createAsynkThunk  api를 사용한다. 기본적으로 청크가 내장되어있어서 따로 설치 ㄴ
==> createAsynkThunk()의 첫번째 자리는 action value, 두 번째에는 함수가 들어간다.
==> 두 번째로 들어가는 함수에서 2개의  인자를 꺼내 사용할 수 있는데, 첫번째 인자는 컴포넌트에서 보내준 payload이고(app.js에서 보낸 payload) 두번째 인자는 thunk에서 제공하는 여러가지 기능이다

---------
## thunk심화(Promise다루기)
- 현재 리듀서가 없는 상태 --> createAsyncThunk API를 이용해서 새롭게 만든다
- 나중에 extraReducers로 나중에 만든다.

✅ 순서!
1. thunk함수를 구현

2. 리듀서 로직을 구현 (기존엔 reducers에 넣었지만 이제는 extraReducers에 넣을거다)
서버랑 통신을 할건데, 100%성공 보장이 없기때문에 진행에 관련된 여러가지 state들을 관리할거
 -지금까지의 redux state => (todos, counter)였다면
```
앞으로의 state는
isLoading : 현재 통신중인가
isError : 혹시 서버에 오류가 났는가
data : 

---> 이런것을 통해서 화면도 제어할 것이다
ex) isLoading이면 화면을 아직 대기중인 상태로 만들어 놔야겠지(그래서 사용자가 알 수 있게끔)

```

 
3. 기능을 확인한다 -->network탭에서 확인

4. store의 값을 조회 + 화면에 랜더링

#### 실습(todolist로 실습)
 👌기존
윗 부분에서 비동기 통신이 끝나고 나면 이 thunkAPI(thunkAIP.dispatch)를 이용해서 reducers로 어떤 동작을 하도록, state를 변경하도록, 업데이트하도록 넘겨줬었는데
 👌이제는
thunkAPI.fulfillwithValue 와 thunkAPI.rejectwithValue 이 두가지를 이용해서 reducer가 아니라 extraReducer 안으로 보내줄거다.

서버에서 데이터를 가져온다.(db.json)
청크함수는 서버와 통신을 하기때문에 비동기 함수여야만 한다.
### 1) __리듀서파일에서 서버로 통신해서 데이터 가져오기
```jsx
todosSlice.js(리듀서파일)

import {createAsyncThunk} from "@reduxjs/toolkit";   --->thunk api불러오기
import {createSlice} from "@reduxjs/toolkit";
import {axios}

const initialState = {
	todos : [],
	isLoading : false,
	isError : false,
	error : null,         --->에러가 나면 이 에러 객체를 채워주면 된다.
}

export const __getTodos = createAsyncThunk(         ----->청크는 언더바로 변수 만든다
	"getTodos",                            ------->첫 인자는 "이름"둘째 인자는 함수(두개)
	try {                                 수행하고싶은 동작 넣기
	async (payload, thunkAPI) =>              ----->  비동기 통신이기에 async await
   const response await axios.get('http//localhost:4001/todos') --> 시도하는 부분
       console.log("response", reponse)            ----->콘솔찍어보려고
	}catch(error) {
		console.log("error", error)                   ----->콘솔찍어보려고
	}
	---------------------------------------------------------------------------
	

)
export const todosSlice = createSlice({
  name : "todos",
  initialState,
  reducers :{}
  extraReducers : {}
})
```

__getTodos로 만들어 놨으니까(콘솔도 읽으려고 만들어놓음) 이 부분을 각 컴포넌트에서 마운트할 때 호출을 하면 직접 읽어올 수 있을듯  ---->app.js가서 읽어와 보자
```jsx
import {useEffect} from "react"
import {useDispatch} from "react-redux"
import {__getTodos} from "./redux/modules/todoSlice"

const App = () => {
	const dispatch = useDispatch();
	useEffect(() => {
	dispatch(__getTodos())---->payload인자가 필요없는 이유는 axios.get하는데 payload사용 ㄴ
	},[])
}이 부분까지는 데이터를 가져왔다는 걸 app.js에서 useEffect로 확인했으니
	이놈을 애플리케이션 내부의 리덕스 스토어로 가져오는 부분을 구현해보겠다
	그 부분을 구현을 해야 그거를 스토어 내부의 스테이트로써 활용할 수 있다
```
--> async thunk함수를 이용해서 비동기로 서버에 접근해서, 서버에서 데이터를 가져오는 부분은 문제가 없다는 걸 확인함  
그럼 이제 가져온 데이터를 store로 넣는 로직을 구현해보자

### 2) 가져온 데이터를 store로 넣는 로직을 구현
```jsx
export const __getTodos = createAsyncThunk(
	 "getTodos",
	 (payload, ThunkAPI) => {
	  try {
	    await axios.get("http://localhost:4001/todos")
	    thunkAPI.fulfillwithValue(response.data);   -->그냥 reponse는 너무 많은 정보라서 
	  }catch(error) {                            data만 넘겨주게 했다. 그놈만 store에 저장
		thunkAPI.rejectWithValue()
		console.lot("error",error)
	  }
	 }
)
```
- thunkAPI.fulfillwithValuem, thunkAPI.rejectWithValue 둘다 툴킷에서 제공하는 api이다
🔔 _thunkAPI.fulfillwithValuem
promise객체가 resolve된 경우(네트워크 요청이 성공한 경우) dispatch해주는 기능을 가진 api
🔔 thunkAPI.rejectWithValue
promise객체가 reject된 경우(네트워크 요청이 실패한 경우) dispatch해주는 기능을 가진 api

dispatch는 리듀서에게 action과 payload를 전달해 주고 state를 업데이트 시키는 과정인데 ,
위 코드는 우리가 reducer을 작성한 적이 없다. 그렇기 때문에 지금부터 그 리듀서 구현을 할 거다.
그 리듀서를 reducers가 아니라 extraReducers에서 구현할거다.
extraReducers 안에 fulfillwithValue랑 rejectWithValue를 통해서 얻는 결과 값이 자동으로 분류가 되는데 어떻게 분류가 되냐면
 thunkAPI.fulfillwithValue(response.data)이 부분은  --> [__getTodos.fulfilled] 라는 곳을 찾아서 감
 thunkAPI.rejectWithValue()
```jsx
export const todosSlice = createSlice({
	 name: "todos",
	 initialState,
	 reducers: {},
	 extraReducers: {
	 [__getTodos.fulfilled] = (state,action) => {     ----->인자로state와action을 가짐
	     console.log("fulfilled :",action )     
	 } 
	 }    ==>정상적으로 처리가된 경우는 extraReducers에서처리가 된다는 것
})
```
console.log("fulfilled :",action )을 찍어보면 아래와 같이 나온다
![](https://i.imgur.com/A460aIz.png)

정상적으로 처리가된 경우는 extraReducers에서처리가 된다는 것을 알 수 있는데 이럴 경우 어떻게 처리를 해야하나 
state로 가지고 있는 값 중에서isLoading이 있는데 그런 부분을 처리하기 위해서fulfilled이외에도 적어야하되는 값이 또 있는데  pending(진행중)
```jsx
extraReducers: {
	  [__getTodos.pending] : (state, action) => {     ----->state와action을 인자로 갖는다
	    //아직 진행중일 때
	    state.inLoading = true;      ----->state에 있는 isLoading을 true만들어주는게 적합
	    state.isError = false; ----->진행중일때 에러인지 모르니까 false로 저장
	 }, 
	 [__getTodos.fulfilled] = (state,action) => {     
	     console.log("fulfilled :",action )    
	     state.isLoading = false; 
	     state.isError = false     ------>에러가 아니니까 fasle
	   + state.todos =  action.payload;   state의 todos에 서버에서부터 받아온 값을 넣어줘
	                                    야한다. ==reponse.data = action.payload
	 },                                       ->todos의 빈 배열에 들어가게 된다.
	  [__getTodos.rejected] = (state,action) => {        
	     state.isLoading = false;        ---->오류가 나도 끝나긴 끝난거니 false
	     state.isError = true;
	     state.error = action.payload
	 
	 }    
```

### 만들었던 state구조가 ui에서 어떻게 쓰고 보여지는지 봐보자
```jsx
const App = () => {
	const dispatch = useDispatch();
	const {usLoading, error, todos} = Selector((state)=> {
	  return state.todos
	})
	useEffect(() => {
	dispatch(__getTodos())
	},[])

 if (isLoading) {
   return <div>로딩중...</div>;
 if(error) {
   return <div>{error.message}</div>
 }
 return (
	 <div>
	 {todos.map((todo) => {
	    return <div key = {todo.id} >{todo.title}</div>
	 })}
 )  
 }
}

//useEffect를 통해 가져온 dispatch(__getTodos())는 우리가 저장하가 위해서 state를 만들어 볼건데
// extraReducers를 통해서 state에 todos,isError,isLoading을 다 세팅해놨었다. 그래서 useSelector을 통해 store안에 있는 state에 접근이 가능한데 state의 todos로 접근을 하게 되면(state.todos) todos안에 있는 것들을 다 가져올 수 있겠지 
//그리고 그 안에 있는 것들을 구조분해할당으로 빼올 수가 있다..
//구조분해할당을 해서 읽어왔더니 현재 상태가 만약 isLoading이야
//이런 구조를 통해서 위해서 이미 다 걸러지게끔 구조를 짰다.
1. (isLoading)로딩이되고 있는 경우에는 그 밑으로 내려오지 않도록 되어있었고
2. 로딩이 끝난 후 에러가 나면 그 밑으로 로딩되지 않게 todos.map에 접근이 아예 안 되도록 --> 그래서 unddfined나 null관련된 오류가 안 나도록 (옵셔널 체이닝 안 쓰도록) 조치를 취함
3. 
```




✅ 서버의 와 클라이언트의 통신은 항상 성공을 보장할 수 없기때문에
try - catch문법을 쓴다 (오류가 날 수도 있기 때문에)
```jsx
try {
 시도하는 부분
}catch(error) {
 에러가 나는 부분