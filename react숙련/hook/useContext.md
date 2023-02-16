# useContext

✅전역적으로  사용되는 어떠한 것을 표현할 때 context란 말을 쓴다

- # createContext 
context생성
- # consumer
context 변화 감지
- # Prvider
context 전달(하위 컴포넌트에게로)

props drilling이 너무 많을 경우 useContext를 쓴다(굳이 필요 없는 중간 컴포넌트를 거치는 게 비효율적이라서)

useContext 사용 예시
```js
가져다 쓸 cotext : Familycomponent
import { createContext } from 'react';
export const FamilyContext = createContext(null);  
// 나중에 privider로 주입하는 그 하위 컴포넌트 들에서 사용할 수 있는 context가 완성될 거다
// 가져다 쓰면 된다
```

```js
할아버지 컴포넌트
import React from 'react';
import Father from './Father';
import { FamilyContext } from '../Context/FamiluContext';
//할아버지가 --> Child한테 어떤 정보를 알려줘서 child가 그 내용을 출력하도록
function GrandFather() {
  const houseName = '스파르타';
  const pocketMoney = 10000; 
  return (
    <FamilyContext.Provider
      value={{
        houseName,       ------>프로퍼티로 value를 넘겨줘야하는데 객체다
        pocketMoney,
      }}>
      <Father />;
    </FamilyContext.Provider>
  );
}
export default GrandFather;

//props가 필요가 없는 이유는 props로 값을 내려주는 게 아니라 우리는 이제contex만든 걸 가지고 외부로 접근하니까 <그러므로 props전부 지워준다>

//familyContext를 임포트 해왔으니 이제 provider을 적용해야한다 (제공자)
(현재보다 하위 컴포넌트에게 제공한다)
//해석 : father컴포넌트 밑으로 이걸 제공해준다
```

```js
child컴포넌트
import React, { useContext } from 'react';
import { FamilyContext } from '../Context/FamiluContext';
function Child() {
  const data = useContext(FamilyContext);
  console.log('data', data); 
  return (
    <>
      <div>나는 이 집안의 막내에요</div>; 할아버지가 우리 집 이름을<span>{data.houseName}</span>
      이라했다 <br />
      게다가 용돈도 <span>{data.pocketMoney}</span>원만큼 줬어요 <br />
    </>
  );
}
export default Child;  
//context로부터 데이터를 받아와야하는데 그 방법은 
 const data = useContext(FamilyContext)로 받아와진다.
```
그림 예시
![[Pasted image 20230211033534.png]]

# props로내려준 값을 쓴 게 아니라 context를 이용해서 값을 받아왔다.


✅주의 사항
렌더링 문제
useContext를 사용할 때, [provider에서 제공한 value가 달라진다면 useContex를 사용하고 있는 모든 컴포넌트가 리렌더링 된다] 따라서 value 부분은 항상 신경 ㄱㄱ(엄청 비효율적임)
-->메모이제이션(기억)이 해결방안