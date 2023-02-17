# jason-server패키지

개념 :  아주 간단한 DB와 API서버를 생성해주는 패키지  --> 임시 fake server같은 느낌임(test용)

패키지 설치법 
1. npm일경우 :  npm install json-server
2. yarn일경우 : yarn add jason-server

사용법 
- src폴더 안에 -- db.json파일을 하나 만든다 (데이터베이스 역할파일) 
- 그 폴더 안에 이걸 넣는다
```jsx
{ 
"todos": [        ------->key값에 ""가 있기에 json형식이란 걸 알 수 있다.
{ "id": 1, 
 "content": "HTML",
  "completed": true 
  }, 
{ "id": 2,
 "content": "CSS", 
 "completed": false
  },
}  
```
- 특정 포트를 지정해줘야한다 :  port라는 옵션을 지정
```jsx
json-server --watch db.json --port 4000    ------>리액트랑 안 겹치게 하기 위해 port를 4000으로 지정한다
```
- 그리고 웹페이지에 localhost:4000/~ 쓰면 db에 저장된 정보가 표출이 된다.

## 내 컴퓨터에서 간편하게 하는 법
`yarn json-server --watch db.json --port 3001`