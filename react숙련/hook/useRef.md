# useRef

✅ref = reference참조
✅ref용도
1. [저장 공간]으로서의 useRef
2. [DOM요소 접근 방법으로서의 useRef]  :  DOM요소에 접근할 수 있도록 도와주는 HOOK
ex) 네이버에 들어가면 네이버 창에 focusiing되어 있다(이렇게 focusing되어야하는 특정 DOM을 선택해야하는 상황이 생기는데 이럴경우 useRef사용하고 변수처럼 쓰기위해서도 사용한다. )

```js
const ref = useRef('초기값');        --->콘솔 : ref{current : '초기값'}
  console.log('ref', ref);
  
  ref.current = '변경값';            ---->콘솔 : ref{current : '변경값'}
  console.log('ref2', ref);
	```                                 
=>즉 ref의 키 값은 변경 가능하다(객체로 표현이 된다.)

⭐⭐이렇게 설정된 ref값은 컴포넌트가 계속해서 렌더링 되어도 unmount전까지(즉 컴포넌트가 죽기 전까지) 값을 유지한다.⭐⭐
-------
✅state와 ref의 차이점
- state 
state는 ref와 비슷한 역할을 한다. 하지만 [state는 변화가 일어나면 다시 렌더링이 일어난다. 이때 내부 변수들은 초기화가 된다.]
- ref
ref에 저장한 값은 렌더링을 일으키지 않는다. 즉 ref의 값 변화가 일어나도 렌더링으로 인해 냅주 변수들이 초기화 되는 것을 막을 수 있다.([ref는 값이 변해도 렌더링이 안 일어나기에 초기화도 막음])
--> 즉 컴포넌트가 100번 렌더링이 이러나도 ref에 저장한 값은 유지된다.
```
🍀즉 state는 리렌더링이 꼭 필요한 값을 다룰 때 쓰면 된다
🍀즉 ref는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.
```

```js
STATE
 //count를 누를때마다 숫자가 올라가는데 화면이  렌더링이 계속해서 다시 일어나고있다
  const pulsStateCountButtonHandler = () => {
    setCount(count + 1);
  };
```

```JS
useRef
//count를 눌러도 렌더링이 되지 않는다
 const pulsRefCountHandler = () => {
    countRef.current++;
    console.log(countRef.current);


      <div style={style}>                     ===> 스타일을 변수로 넣어서 편하게함
        ref영역입니다, {countRef.current} <br />   -==> 객체이고 초기값에 접근하려고
        <button onClick={pulsRefCountHandler}>ref 증가</button>
      </div>

  };
```
---------
✅DOM
Document Object Model로 웹 페이지에 대한 인터페이스입니다. 기본적으로 여러 프로그램들이 페이지의 콘텐츠 및 구조 , 그리고 스타일을 읽고 조작할 수 있또록 api를 제공합니다.

DOM예시

```JS
ID아이디로 Focusing되게끔

const idRef = useRef('');
useEffect(() => {
    idRef.current.focus();


    아이디 :{' '}
        <input type="text" ref={idRef} />
  }, [id]); //id라는 state가 바뀔때마다 얘가 수행되어야하니 id를 넣어준다
```

✅응용 예시 로그인 값이 특정 개수를 넘길 때 비밀번호로 넘어가게
==1==, useState를 쓰고 
id 와 pw가 타이핑 되어서 값이 변경 되어야하니까 
==2==, useEffect를 쓰고
id의 값과 pw의 값이 리렌더링 됨에 따라 수행해야하는 어떤 것(포커싱되는 것)이 있기에 
==3==, useRef를 써야하는 이유
아이디가 포커싱 되게 해줘 즉 얘가 focus될 수 있도록 얘를 잡아줘야 한다 지정을 해줘야한다 이에 대한 ==레퍼런스(참조, 언급,가리킴)==를 갖고 있어야한다 ==id의ref니까 idRef==

```js
const [id, setId] = useState('');
const [pw, setPw] = useState('') 
const idRef = useRef('');
const pwRef = useRef('');

  useEffect(() => {
    idRef.current.focus();
  }, [id]); //id라는 state가 바뀔때마다 얘가 수행되어야하니 id를
  
  useEffect(() => {
    if (id.length >= 10) {
      pwRef.current.focus();
    }
    console.log('안녕');

  });
   return (
    <>
      <div>
        아이디 :{' '}
        <input value={id} onChange={(e) => setId(e.target.value)} type="text" ref={idRef} />
      </div>
      <div>
        비밀번호 :{' '}
        <input alue={pw} onChange={(e) => setPw(e.target.value)} type="password" ref={pwRef} />
      </div>
    </>
  );
}
```


✅리렌더링

버튼을 눌렀을 때 숫자가 올라가야 된다라는 거는 그 숫자라는 부분이 올라가는 게 보여야되는거니까 ->렌더링이 다시 일어난다->렌더링이 필요한 것
