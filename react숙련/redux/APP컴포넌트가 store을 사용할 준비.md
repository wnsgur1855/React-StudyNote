✅ redux파일 :  리덕스와관련된 코드를 모두 모아 놓을 폴더입니다

✅ config파일 :  리덕스 설정과 관련된 파일들을 놓을 폴더입니다

✅ configStore : "중앙state관리소"인 store를 만드는 설정 코드들이 들어있는 파일

✅ modules : 우리가 만들 state들의 그룹이라고 생각하면 된다.(todolist만든다면 그에 필요한 state들이 모두 모여있을 todo.js를 생성할텐데, 이 파일이 곧 하나의 모듈이 된다.)
----->state들이 그룹이다.

```js
import { createStore} from 'redux';
import {combineReducers} from 'redux';

1. const rootReducer = combineReducers({
       리듀서들을 넣을거야 (함수가 리듀서다.)
})
3. const store = createStore(rootReducer)

export default store;
```
1. createStore() 리덕스의 가장 핵심이 되는 스토어를 만드는 메소드(함수) 입니다. 
 리덕스는 단일 스토어로 모든 상태 트리를 관리한다고 설명해 드렸죠? 리덕스를 사용할 시 creatorStore를 호출할 일은 한 번밖에 없을 거예요.

2. combineReducers() 리덕스는 action —> dispatch —> reducer 순으로 동작한다고 말씀드렸죠? 이때 애플리케이션이 복잡해지게 되면 reducer 부분을 여러 개로 나눠야 하는 경우가 발생합니다. combineReducers은 여러 개의 독립적인 reducer의 반환 값을 하나의 상태 객체로 만들어줍니다.

# 해설:

-->1. 리듀서들을 모아서 한 개로 만들어 놓은 기본 리듀서(rootReducer)
combineReducers란 api는 이제 메서드이니까(함수 형태이니까) 괄호를 열고 닫으면 실행이 되는 것, 인자는 변수가 들어간다. 변수는 객체 형태를 넣을 것이다. 
{}안에 modules안에 넣어놓은 state의 묶음들을 몰아 넣을 것! 그러면 모든 컴포넌트들은 props로 값을 내려주지 않더라도  바로 이 중앙 데이터 관리소로 데이터를 바라 볼 수 있게 된다.

--->2. 실행이 돼서 그 return값을 store에 넣어줄 거다. 인자는 변수 뭐가 들어가냐 reducer의 묶음 들이 들어가야한다. = rootReducer

자 그럼 이렇게 만든 store를 아직 사용하고 있지 않기 때문에 이걸 사용해서 애플리케이션 내부로 넣어야 하니까 export default store;을 써서 만든 것을 바깥으로 내보내야한다.
-->sotre을 바깥으로 내보냄

index.js로 가서 app을 provider로 감싼다 --> app컴포넌트가 provider지배권 안으로 들어옴 
provider : store을 기반으로 지배권을 행사함 ->app컴포넌트 있는 곳 전체에서  store을 쓸 수 있다.
```js
 import store from './redux/config/config/confiStore'; -->export한 store을 import로가져옴
  <Provider store = {store}>
        <App />
  </Provider>
```
❓알면 좋은 것
export default 하면 import 해서 받아갈 때{}가 필요가 없다

# 이제 App컴포넌트에서는 store을 사용할 준비가 됐따~