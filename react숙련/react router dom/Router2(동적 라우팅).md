works페이지에 여러개의 work가 보이고 우리가 하나의 work를 누르면 각각의  독립적인 페이지를 가지도록 구현하려면  --> 2000개 넘어가면 전부 유지보수 할 수가 없다.
```js
router.js파일

<Route path="works/:id" element={<Work />} />
```
데이터 파일을 하나 따로 만들어서 관리하기
```js
const data = [
  {
    id: 1,
    todo: '리액트 배우기',
  },
  {
    id: 2,
    todo: '공부 배우기',
  },
  {
    id: 3,
    todo: '노는거 배우기',
  },
export default data;
```

이 데이터들을 map함수로 돌리면서 하나씩 불러오기
```js
work 컴포넌트 하나 생성

import { Link } from 'react-router-dom';      ------>Link api를 썼다
import data from '../shared/data';             ------->data파일을 import

{data.map((item) => {
        return (                                        map함수는 key값이 존재해야함
          <div key={item.id}>                   ------->key로 unique값 정해놓기
            {item.id} &nbsp
            <Link to={`/works/${item.id}`}>{item.todo}</Link>   ->link는 to와 같이
          </div>
        );
      })}
```

무슨 파라미타()가 들어왔는지 알기위해 hook이 필요하다 -->useParam
- useParam
useParams 은 path의 있는 id 값을 조회할 수 있게 해주는 훅 입니다.

- 정리
Dynamic Route를 설정할때는 :id 로 설정하고, id 값은 useParams을 이용해서 각 컴포넌트에서 조회할 수 있다.