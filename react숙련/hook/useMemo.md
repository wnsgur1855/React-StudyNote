value에 대한 메모이제이션(함수가 리턴하는 값, 값 자체)
사용법 
```js
const value = 반환할 함수()

const value = useMeomo(()=> {      ------> <<useMemo사용했을 때>>
	return 반환할 함수()
},[]);
```
의존성배열(dependency Array)의 값이 변경 될 때만 반환할 함숙()가 호출된다.
그 외의 경우는 memorization(기억)해놨던 값을 가져오기만 함

무거운 작업 ===for(let i = 0; i < 100000; i++)=== --.>===return 100===
이 같은 경우
```js
function HeavyButton() {
const [count, setCount] = useState(0);
const heavyWork =() => {
for(let i = 0; i < 1000000; i++) {}
	return
}
const value = useMemo(() => heavyWork(), [])
}
return (
	버튼이있다. 이하 생략
)
```
만약 const value = heavyWork 이대로 썼다면 버튼을 눌렀을 경우 숫자가 바로바로count 되지 않고 조금 delay가 된다,
[const value = useMemo(() => heavyWork(), []) 씀으로써 값을 메모리에 따로 저장해서 기억?]

useMemo는 많이 사용하면 성능이 악화되므로 필요할 때만 사용할 것
