1. reducer함수를 export default한다
2. Action creator함수들을 export한다.
3. Action type은 app/reducer/Action_type형태로 작성한다.
==> [결론은 모듈 파일 1개에 Action Type, Action Creator, Reducer가 모두 존재하는 작성방식]
```js
✅action value

const PLUS_ONE = counter / PLUS_ONE;
const MINUS_ONE = counter / MINUS_ONE;
const PLUS_N = "counter /PLUS_N"
```

```js
✅action creator

export const plustOne = () => {
  return {
    type: PLUS_ONE,
  };
};
export const minusOne = () => {
  return {
    type: MINUS_ONE,
  };
}; 
export const plus_n = (payload) => {
    return {
        type : PLUS_N
        payload : payload,
    }
}
const initialState = {
  number: 0,
};
```

```js
✅reducer

const counter = (state = initialState, action) => {
  switch (action.type) {
    case PLUS_ONE:
      return {
        number: state + 1,
      };
    case MINUS_ONE:
      return {
        number: state - 1,
          };
      case PLUS_N:
          return {
              number : state.number + action.payload,
          }
    default:
      return state;
  }
};
```

우리가 이러한 방식을 쓴 이유 =-==> ducks pattern 을 따라가고자 했따ㅏㅏㅏ🤤🤤🤤🤤🤤