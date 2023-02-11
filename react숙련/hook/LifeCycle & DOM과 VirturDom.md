# LifeCycle
- 리액트 컴포넌트는  ===mount=== ===Update=== ===Unmount===  의 과정을 거치는데 사람처럼 태어나고 죽고 변화하는 것을 말한다. 컴포넌트들은 생명주기가 존재.(컴포넌트 중심 라이브러리 집합체)

- 리액트 생명주기에서 말하는 많은 매서드들은 거의 클래스형컴포넌트이다. 

===mount===
1. construtor(생성자)
컴포넌트가 맨 처음 만들어 질 때 호출되는 생성자 함수이다

2. getDerivedStateFromProps
부모 컴포넌트로부터 props를 전달 받을 때, state에 값을 일치시키는 역할을 하는 메서드
mount될 때 뿐만 아니라 update(리렌더링) 될 때도 호출

3. render(화면에 그리는 메서드)
최초 mount가 준비완료되면 호출되는, 즉 렌더링하는 메서드
컴포넌트를 DOM에 mount하기 위해 사용

4. componentDidmount
컴포넌트가 브라우저에 표시가 된 후 호출되는 메서드(상황에서 어떠한역할을 해야하는 함수, 기능 로깅을 적는다.)

===Update=== : 화면상의 렌더링이 완료돼서[state값이 변화가 있거나], [props를 전달 받거나], [부모 컴포넌트가 변경됐을 때 ]다시 리렌더링이 일어난다.
1. getDerivedStateFromProps
2. shouldComponentUpdate 
	리렌더링의 여부 파단(true:진행 false : ㄴ진행)-->함수형 컴포넌트에서 memo,useMemo,useCallback이 역할을 대신함
3. render
4. getSnapshotBeforeUpdate
	컴포넌트에 변화가 일어나기 직전 dom의 상태를 저장
	스냅샷 형태의 데이터.(특정한 시점에서의 복사본)
5. componentDidUpdate
	컴포넌트 업데이트 작업 완료 후 호출

===Unmount=== 컴포넌트가 죽을때 (컴포넌트가 DOM에서 제거되는 시점)
1. componentWillUnmout
컴포넌트가 사라지기 전 호출되는 메서드
useEffect의 retrun 문과 동일하다
```js
useEffect (() => {
	return () =>{}         ---------->이 부분이 화면에서 제거가 될 때 호출이 되는 부분!!
},[])
```

---------------
# DOM과 VirturDom

===DOM===
컴포넌트들로 이루어진 웹페이지 전체를 document(문서)라고하는데 이 document의 요소 하나 하나를 elment라고 한다. 그 element요소를 tree형태로 표현한 것이 DOM이다.
tree의 요소 하나하나를 '노드'라고 부르고 각각의 노드는 해당 노드에 접근과 제어를 할 수 있는 API를 제공한다["API는 HTML요소에 접근해서 수정할 수 있는 함수"]>
```JS
//id가 demo인 녀석을 찾아, 'hello world'를 대입해줘
document.getElementById("domo").innerHTML = "hello junhyuck"
// p 태그들을 모두 가져와서 element 변수에 저장해줘
const element = document.getElementByTagName('p')
//클래스 이름이 intro인 모든 요소를 가져와서 x 변수 저장해줘
const x = document.getElement.ByClassName("intro")
```

===Virtual DOM===
Brower DOM(실제DOM) 에서 복사한 객체 형태가 Virtual DOM(가상 DOM)이고  그 가상돔은 두 가지 버전의 가상돔을 가지고 있다. 실제DOM(BroswerDOM)을 조작하는 것보다 가상돔을 조작하는 게 훨씬 빠르게 조작 가능하기에 이런 방식을쓴다.

가상돔(virtual dom)에는 2가진 버전이 있는데
1. 화면이 갱신되기 전 구조가 담겨있는 가상DOM객체
2. 화면 갱신 후 보여야 할 가상 DOM객체
--> state가 변경돼야 리렌더링이 되는데 그 때 2번에 해당되는 가상DOM을 만든다
diffing단계에선 두 개를 비교해서 어떤 element가 변화했는지 빠르게 파악
Batch update를 통해 element를 한꺼번에 반영한다!

![[Pasted image 20230211131525.png]]
✅배치성 업데이트
useState에서 배치성 업데이트를 쓴다는 것을 알았는데,   모든 데이터를 한 번에 모아서 보내준다는 뜻 

✅이해하기
<**클릭 한 번으로 화면에 있는 5개의 엘리먼트가 바뀌어야 한다면>**
-   실제 DOM : 5번의 화면 갱신 필요
-   가상 DOM : Batch Update로 인해 단 한번만 갱신 필요