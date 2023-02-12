✅어떤 자원에 대해 CRUD를 진행할 수 있게 HTTP method( GET, POST, PUT, DELETE)를 사용하여 요청을 보내는 것. 이 때 요청을 위한 자원은 특정한 형태로 표현된다

✅URI를 통해 정보의 자원만 표시하고 자원의 행위는 HTTP method로 명시한다
- (자원) : 브라우저에서 입력하는 주소(uri)
- 행위(ver) :  http method (GET, POST, PUT, DELETE)
- 표현
```js
GET / users/3/profile  --> users중에서 3번 id를 갖고있는 사람의 프로필을 보여줘
(행위) (자원 = uri)
```
GET :  행위
users/3/profile : 자원

==> 이런것들로만 표현해라 = REST API

✅ REST API의 규칙

1. URI는 명사를 사용하고 소문자로 작성
2. 명사는 복수형을 사용한다.
3. URI의 마지막에는 /를 포함하지 않는다
4. URI에는 언더바가 아닌 하이픈을 사용한다 (post-list) ㄴㄴ
5. URI에는 파일의 확장자를 표시하지 않는다 (example.png) ㄴㄴ

위의 까다로운 규칙(조건)을 만족시킨 통신 설계 상태를 ==RESTFUL==이라고 한다.

✅ Path Variable 과 Query Parameter

Path Variable  : 10이라는 id를 식별하기 위해 사용
```js
/users/10 -----> 전체 데이터 또는 특정 하나의 데이터를 다룰 때처럼, 리소스 식별하기 위해 사용.
```

Query Parameter : 데이터를 정렬하거나 필터링하는 경우 적합
```js
/users?user_id=10 
```

quiz
```js
/users # Fetch a list of users  -----------------------------> Path Variable
/users?occupation=programer # Fetch a list of programer user  ->Query Parameter
/users/123 # Fetch a user who has id 123 --------------------> Path Variable
```

🍀언제 이걸 왜 써야하는지만 기억하자🍀
Path Variable  : 전체 데이터 또는 특정 하나의 데이터를 다룰 때처럼, 리소스 식별하기 위해 사용
Query Parameter : 데이터를 정렬하거나 필터링하는 경우 적합