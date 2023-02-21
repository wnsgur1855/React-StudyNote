비동기 부분을 다룰 때 axios패키지가 등장한다.

## Axios
==Axios통신을 통해서 DB에 CRUD를 한다==
개념 :  promise를 기초로 해서 http통신을 할 수 있는 라이브러리(패키지)
패키지 설치 :  yarn add axios
1. axios패키지를 설치하고 , yarn json-server 도 설치

2. yarn json-server --watch db.json --port 4000(임시 서버를 만든다. 포트는 4000으로 )
--->클라이언트와 서버는 완전히 다른 공간이라는 것을 인식하자

3. 추가 수정 삭제를 하면서 db를 업데이트 할 거다(crud과정을 할듯)

4. http 메서드들을 이용

### GET (async / await사용법)
서버에 어떠한 데이터를 달라고 요청하는 부분 
서버한테 어떤 투두 데이터를 요청할거야 - ID만 줄테니까 ID에 해당되는 모든 값들을 서버는 줘 - 서버가 줄 때는 ID , TITLE, CONTENTS , ISDONE, 날짜 이런 것들을 줬으면 좋겠어 
-->==이런 것들의 명세를 만들어 놓는다. 요청하는 방법 또한 만들어놔야한다.(PATH / QUERY방법)==

```jsx
import axios from 'axios';
import { useEffect } from 'react';
import './App.css';

function App() {
  //2. db로부터 값을 가져오기 위해서 함수를 하나 만들거다
  //3. 비동기 함수를 만들어야한다 -> 서버 통신을 한다는 것 자체가 비동기를 의미(제어권이 나한테 없는 것)
  //4. async await 문법 이용해서 비동기 함수 만들어보자

  const fetchTodos = async () => {
    const response = axios.get('http://localhost:4001/todos');
    console.log('response', response); //6. 이렇게 콘솔을 찍으면 pending이 나오는데 응답을 받기 전에 reponse가 찍혀서 그런다
  };

//7. 응답을 받을 때까지 기다려줘야하는데 그 문법이 await이다
  useEffect(() => {
    //1. 최초의 mount될 때 db로부터 값을 가져올 것이다
    fetchTodos(); //-->5. mount가 처음 될 때 이 함수를 호출하기 위해서
  }, []);
  return <div>Axios</div>;

}
export default App;
```

```jsx
const response = await axios.get('http://localhost:4001/todos');

//async 블럭 안에서 await 키워드를 만나면 이 줄은 이 줄이 끝날때까지 기다려준다
//await를 만나면 response를 할당 받을 때까지 기다렸다가 밑의 줄이 실행된다
```
console.log
![](https://i.imgur.com/bHWsftD.png)

### get 방식 사용
```jsx
function App() {
  const [todos, setTodos] = useState(null) ---->받아온 data를 state로 쓰기위해서 state관리
  const fetchTodos = async () => {
    const { data } = await axios.get('http://localhost:4001/todos');  ---->await사용
    console.log('data', data);
    setTodos(data);
  };
  useEffect(() => {
    fetchTodos();
  }, []);
  console.log(todos);
  return (
    <div>
      {todos?.map((item) => { ---------------------->옵셔널 체이닝
        return (
          <div key={item.id}>
            {item.id} : {item.title}
          </div>
        );
      })}
    </div>
  );
}
//1. 컴포넌트가 렌더링 처음 됐을 때 비동기 함수가 실행되기 전에 return문이 먼저 렌더링 된다

//2. 렌더링 되고 나서 비동기함수가 async니까 더 타이밍이 늦는다

//3. 코드는 위에서부터 실행되지만 async이기때문에 async부분이 돌아갈때까지 밑에가 기다리지 않는다

//4. 그래서 return문이 먼저 실행이 되는데

//5. 현재todos는 없을수도있는 즉 null일 수도 있다.

//6. 그렇기 때문에 todos?를 해서 "옵셔널 체이닝"을 넣어주면 잘 나온다.
```

### POST
서버에 어떤 데이터들을 추가해달라 요청하는 방식. = ex) todolist의 추가기능
```jsx
  const [inputValue, setInputValue] = useState({
    title: '',
  });

const onSubmitHandler = async () => {    ----->원래 인자를 받아야하는데 state로 저장돼있는                            
                                                  값을 활용하면 되니까 안 받아도 된다
    axios.post('http://localhost:4001/todos', inputValue); -->입력하려고 하는 값 inputValu
    setTodos([...todos, inputValue]);   ---> *밑의 설명에서 다룰게*
  };
  return (
    <>
      <div>
        {/* input영역 */}
        <form
          onSubmit={(e) => {    
            e.preventDefault();           ------->새로고침 방지 속성
  //버튼 클릭 시, input에 들어있는 값(state)을 이용하여 db에 저장(post요청)
            onSubmitHandler();
          }}
        >
          <input
            value={inputValue.title}     ---------> type이 객체에서title을 가져와야하니까
            onChange={(e) => {
              setInputValue({
                title: e.target.value,   ---------> 객체형태로 똑같이 맞춰줘야함
              });
            }}
            type="text"
          />
          <button type="submit">추가</button>   ---->form태그에서의 버튼은 submit속성이
                                                      내장되어있다.(누르면 새로고침이 발생)
        </form>
      </div>
//1. 추가를 누르고 새로고침을 해야 추가가 된니까 바로 갱신으로 바꿔주고싶다

//2. 그러려면 state도 같이 렌더링을 시켜줘야한다.(화면도 같이 렌더링시킨다.)

//3. 컴포넌트가 같이 렌더링 안 되는 이유 : state값이 안 변해서 그렇다.

//4. 컴포넌트가 렌더링 되는 조건 1. state가 변경되거나 2. props가 변경, 3. 부모 컴포넌트가 변경

//5.  onsubmithandler가 처리가 되고 나서 이것과 동일하게 state도 바꿔줘야지 화면도 정상 렌더링된다

//6. todos도 같이 바꿔줘야한다

//7. setTodos([...todos, inputValue]) 기존에 있던 todos에 inputValue인 title을 넣어주면 된다.

//8. ...todos로 객체를 그냥 요소로 발가벗기고 inputValue값으로 바꿀거야 
```

### DELETE
```JSX
const onDeleteHandler = async (id) => {     //-->인자로id를 받아와야지
    //---> axios통신을 통해 db의 데이터를 삭제해야하니 비동기 async를 써야한다
    axios.delete(`http://localhost:4001/todos/${id}`); //--->``을 사용해 id를 delete할거다
    setTodos(
      todos.filter((item) => {
        return item.id !== id;          ------------------>새로고침 안 해도 삭제가 되도록
      })
    );
  };

<div key={item.id}>
          {item.id} : {item.title}
 <button onClick={() => onDeleteHandler(item.id)}>삭제</button> -->인자로 KEY값 넣음
      </div>

//1. onClick={() => onDeleteHandler(item.id)이렇게 하는 이유는

//2. onDeleteHandler(item.id)이렇게만 쓸 경우 렌더링 과정에서 이미 함수를 실행해버리고 렌더링을 한다 --> 페이지시작하자마자 저게 호출되어버림

//3. 그렇기에 한 번 함수로 감싸줘야한다. 함수 호출하고 렌더링 하지 않는다.      
```

### PATCH
데이터를 수정하고자 서버에 요청을 보낼 때 쓰는 메서드
```jsx
  const onupDateHandler = async () => {
    try {
      await axios.patch(`http://localhost:4001/todos/${targetId}`, {
        title: content,              --------->DB의 title을 INPUT의 content로 바꾼다.
      });                             --------->await를 사용해서 줄 끝나야 다음단계 ㄱ
      setTodos(
        todos.map((item) => {
          if (item.id == targetId) {
            return { ...item, title: content };  --->MAP에 있는 요소를 풀어서 저렇게 바꾼다
          } else {
            return item;
          }
        })
      );
    } catch (error) {
      console.log(error);
    }
  };

```

✅ 더 알고가기
try-patch문법
보통 에러가 발생하면 스크립트는 즉시 중단되고 , 콘솔엔 에러가 출력된다.
이 문법을 사용하면 스크립트 중단을 방지하고, 에러를 잡아서(catch) 더 합당한 걸 할 수 있다.
```jsx
try {
 ---코드---
}catch(error) {
 ---에러 핸들링---
}
```

✅  e.preventdefault의 유효성 검사 (이벤트 처리)