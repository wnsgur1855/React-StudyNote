# useState
- 가장 기본적인 hook이다
- 함수형 컴포넌트 내에서 가변적인 상태를 갖게 한다.
- 사용법 : const [state, setState] = useState(초기값)
- 사용법 해석 :  useState가 return하는 값이 배열이다. 그 배열을 staet, setState로 구조분해할당으로 받은 것 분이다. 배열의 return값은 초기값으로 지정해놓은 변수(state)가 있고 그 state를 변경(컨트롤)할 수 있는 setState. 이렇게 두 개의 배열로 이루어져있다.

#useState의기존방식
```jsx
 const [number, setNumber] = useState(0);
  const buttonClick = () => {
    setNumber(number + 1)
  };
  return (
    <>
      <div>Number : {number}</div>;<button onClick={buttonClick}>버튼</button>
    </>
  );
```

## 함수형 업데이트

함수의 인자부분()에는 현재 state를 가져올 수 있다(여기선 currentNumber)
```jsx
  const [number, setNumber] = useState(0);
  return (
    <>
      <div>Number : {number}</div>;
      <button onClick={() => {                      //기본형에서 함수형으로 변환
          setNumber((currentNumber) => {
            return currentNumber + 1;
          });
        }}
      >버튼</button>
    </>
  );
```

```jsx
onClick={() => {setNumber((currentNumber) => currentNumber+1)}}

{/*return문이 한 줄일경우 이렇게 줄여서 쓸 수 있다*/}
```
✅✅차이점은 뭘까?
일반 업데이트 방식 :  [배치성으로 처리된다]]
리액트에서 렌더링을 하기 위해서 state를 파악하는데 그 state를 파악하는 방법이 "배치업데이트" 한꺼번에 변경된 내용들을 모아서 한 번만 실행을 한다.  setNumber을 여러번 해도 이거를 한 꺼번에 모으기 때문에 => 똑같은 건 한 개로 친다
```js
  <div>Number : {number}</div>;<button onClick={
              setNumber(number + 1)
              setNumber(number + 1)
              setNumber(number + 1)}       --------->count가 1씩만 증가
              >버튼</button>
```

함수형 업테이트 방식 :  명령들을 모아서 각각 한 번씩 실행을 시킨다. 세 번을 동시에 실행을 시키면 명령을 모아서 순차적으로 한 번씩 한 번씩 시킨다 
이유 : 인자 부분에 현재 상태의 state가 들어오는데 바뀐 state를 반환하고  그 다음엔 바뀐 값(즉 현재 state)에서 또 바뀐 값을 반환하고 ...이것을 반복한다.----->최신값 유지

```js
      <button onClick={() => {
                  setNumber((currentNumber) => currentNumber + 1)
                  setNumber((currentNumber) => currentNumber + 1)
                  setNumber((currentNumber) => currentNumber + 1)
                  ---->카운트가 3씩올라감
```

❓❓그럼 리액트 환경에서 "[배치성]"을 쓰는 이유는?
->렌더링이 잦다: 성능에 이슈가 있다. 그러기때문에 불필요한 렌더링을 피하기 위해 한꺼번에 요청사항을 모아서 한 번만 처리하는 게 렌더링을 줄일 수 있는 방법이기에 리액트에서 사용함


