# axios interceptor
interceptor : 방해, 간섭 => 중간의
두 상황에서 흐름을 가로채서(간섭해서) 어떠한 코드상의 관여를 할 수 있게 한다.
1. 요청(request)이 처리되기 전(http request가 서버에 전달되기 전)
2. 응답(response)의 then(=성공) 또는 catch(=실패)가 처리되기 전
==axios통신을 할때 request를 할때 reponse를 받을 때, 그 사이에서 뭔가를 할 수 있게 처리해주는 것==
![](https://i.imgur.com/A2AZnuP.png)
인테셉터에서 처리할 수 있는 예들
- 요청 헤더 추가
- 인증관리(jwt)
- 로그 관련 로직 삽입
- 에러 핸들링

## instance
하나도 오염되지 않은 axios, axios패키지에서 가져온 아무 가공도 하지 않은 axios이다.
하지만 이 instance를  가공해서 우리만의 instance로 만들거다
url이 바뀌면 하나하나 다 바꿔야해서 힘들다 --> url을 전체에서 관리

1. axios폴더를 하나 만든다
2. 폴더 안에 api.js 파일을 생성
3. 파일 안에 
```jsx
import axios from "axios"
const instance = axios.create({--> axios를 이용해서 instance로 생성할 수 있는 api를호출 함
	baseURL : "http://localhost:4001",
}) 
export default instance;

instance의 baseURL을 만들었으니 instance를 통해서 우리가axios요청 하는거면 
굳이 baseURL: 'http://localhost:4001'를 적지 않아도 무조건 여기로 요청을 한다.
```

- 객체가 입력값으로 들어가는데 key value페어가 항상 들어간다
- 안에 들어가는 객체는 configuration객체다
- axios를 구성하는 환경설정 관련 코드가 입력값으로 들어간다.


4. app.jsx로 가서 import를 한다
```jsx
import api from './axios/api'  ----->export default를 했으면 이름이 달라도 된다.
                                            굳이 instance를 이름으로 안 해도 된다.
```

5. get 부분만 수정해보자  --> 그냥 axios부분을 api(instance)로 바꿔주고 baseURL만 지워주면 됨
```jsx
const fetchtodos = async () => {
	//const {data} = await axios.get("http://localhost:4001/todos");
	const {data} = await api.get('/todos')   ----> 위 axios.get을 instace로 바꿈
}
```

-----axios에 대한instance자체를 변형해 준 거다-----
--

==request를 보내기 전 response를 받기 전 과정도 한 번 해보자==
==중간과정에서 간섭해보는 연습을 하자==

- 기본 골격
```jsx
instance.interceptors.request.use(  
	//요청을 보내기 전 수행
	function(){}
	//오류 요청을 보내기 전 수행
	function(){}           ---------->콜백함수가 두 개 온다
)
instance.interceptors.reponse.use(
	//서버로부터 정상 응답을 받는 경우
	function(){}
	//서버로부터 오류 응답을 받은 경우
	function(){}
)
```
- 쓰는 방법(정상수신,응답)
```jsx
instance.interceptors.request.use(
  //요청을 보내기 전 수행
  function (config) {
    console.log('인터셉트 요청 성공');
    return config;
  },
  // 오류 요청을 보내기 전 수행
  function (error) {
    console.log('인터셉트 요청 오류');
    return Promise.rejext(error);
  }
);
instance.interceptors.response.use(
  //서버로부터 정상 응답을 받은 경우
  function (response) {
    console.log('인터셉트 정상 응답 수신!');
    return response;
  },
  //서버로부터 오류 응답을 받은 경우
  function (error) {
    console.log('인터셉트 오류 응답 수신!');
    return Promise.reject(error);
  }
);
//인터셉터의 정의 : 이런식으로 axios의 모든 과정에서 우리가 요청과 수신의 모든 과정에서 관여할 수 있다
```
요청을 성공하고 서버에서도 정상적으로 받았을 경우 console.log
![](https://i.imgur.com/sb0kpGD.png)

- 에러를 내봤을 경우
인스턴스에는 타이머 속성이 있다.
[timeout속성 : 우리가 요청을 보낼 때, 몇 초 이상 기다리게 하면 오류 낼 거야(기준: 밀리세컨)]
```jsx
instance에 timeout:1, -->0.001초니까 모든 요청이 실패하게끔 돈다(서버가 응답주기전 
												               무조 건 실패)
```

![](https://i.imgur.com/T4THSl6.png)
요청은 당연히 성공하지만 서버가 응답을 주기전에 시간이 지났으므로 오류를 냈다.
오류 메세지 : message: "timeout of 1ms exceeded" ---> 1ms 타임아웃이 초과됐다.

+
토큰 관련 부분, 인증관련 부분까지도 넣을 수 있다.
ex) 고객이 로그인을 해야만 보낼 수 있는 요청부분들이 있을 수도 있다. 그런 부분들은 instance에서 체크를 한다.