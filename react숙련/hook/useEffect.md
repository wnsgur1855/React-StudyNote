[useEffect] : 화면이 렌더링이 될때마다 , 특정한 작업을 수행해야 할 때 ->설정하는 hook
ex) 화면이 렌더링 될 때 콘솔을 찍어야한다거나, alert 등을 수행하게 하는 hook
===화면이 렌더링 될 때 어떤 작업을 하고 싶다===

-->1. 어떤 컴포는트가 화면에서 보일때 
	 2. 사라졌을 때 무언가 실행을 하고 싶다면 (return)
	 그때 useEffect를 사용한다
```js
const [value, setVlue] = useState('');
  useEffect(() => {
    console.log('hello');
  });
  return (
    <div>
      <input type="text" value={value} onChange={(e) => setVlue(e.target.value)}/>
    </div>
  );
```
순서 :
1. input에 값을 입력한다면
2. value, 즉 state가 변경된다
3. state가 바뀌었기 때문에 app컴포넌트가 리렌더링 된다.
4. 리렌더링 되면-> useEffect()가 다시 실행
1~4 반복
++value라는 state의 어떠한 값{value}을 input에 입력할 때마다 value라는 state에 저장이 된다. ----> input에 값을 입력할 때마다 렌더링이 다시 되고 있다. 

[의존성 배열(dependencty array) ]
이 배열의 값을 넣으면, 그 값이 바뀔 때만 useEffect를 실행한다.
[이배열] 이 배열에 값을 넣으면 그 값이 바뀔때만 useEffect가 실행된다

사용법 : 함수가 끝나는 부분 뒤에 두 번째 인자로 넣어준다
배열에 어떤 값이 바뀌면 hello가 출력이 되게 할 지를 정하면 된다.
현재 []빈 배열이기때문에 :  어떤 값을 입력하던지간에, 어떤 값이 변하던지 간에 의존성 배열엔 값이 없기 때문에 어떤 state가 변해도 hello는  화면이 처음 로딩 될 때만 동작한다.
```js
useEffect(() => {
    console.log('hello');
  }, [])                  -------->콘솔엔 처음 hello만 찍히고 이제 안 찍힌다
```

화면에서 없어졌을 때도 동작을 하는데 --> clean up
```js
  useEffect(() => {
      console.log('hello');
      return () => {
          console.log("나 사라져요")
      }
  }, []);
 -----------> useEffect안에 body부분에 return문을 함수형태로 쓰면 된다
```

--------------------

✅useEffect는 콜백함수로 들어가는데~
콜백함수 :  매개변수로  함수가 들어가는 것

```js
useEffect(function() {})  //이런 형태
```

