# JSON과 비동기통신 실습
jason : javaScript Object Notation
자바스크립트 객체 문법에 토대를 둔 문자 기반의 데이터 교환 형식
- ""만 허용

메서드
1.  자바스크립트객체(javascript object)를 json형태로 바꾸는 것
2.  jason형태를 자바스크립트 객체 형태로 바꾸는 것
이렇게 두 개를 다루는 api를 공부해보자

## stringify() 
string화(문자열화)를 시킨다 --> 자바스크립트 객체를 json문자열로 바꾼다.
===사용법 : JSON.stringify({자바스크립트 객체})===
```js
console.log(JSON.stringify({x : 5, y : 6}))
//--> 예상 아웃풋 : "{"x" : 5, : "y" : 6}"
```
아웃풋이 JSON인지 어떻게 아나요?  ---> key부분에  ""가 있으면 JSON이라고 한다.

## purse()
JSON문자열을 자바스크립트 객체로 변환한다.
네트워크 통신의 결과를 통해 받아온 JSON문자열을 --> 프로그램 내부에서 사용하기 위해 JS객체로 변환
```JS
const json = '{"result" : true, "count" : 42}'
const obj = JSON.parse(json);

console.log(obj.count)  -->42
console.log(obj.result) --> true
```

+{JSON}Placeholder
가짜 서버(fake server)

컴포넌트가mount 될 때 실행되는 함수가 useEffect