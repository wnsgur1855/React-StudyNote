# HTTP

HTTP통신이란? =  communication
![](https://i.imgur.com/ACeTnpR.png)
약속을 protocol이라고 하는데
웹서버 <---> 클라이언트 간의 주고 받는 상호간의 약속을 ==HTTP 프로토콜==이라고 한다.
- 요청(Requiest) : 우리는 서버에게 계속 요청을 할 거다=([요청하는 대상은 클라이언트])
- 응답(Response) : 서버는 그 요청에 응답을 할 거다=([응답을 "주는" 대상은 서버])
-->http resquest를 보내고 http reponse를 받는다

#### url
url구성 :     http(s)://    +       www.        +   hostinger.com   +   /tutorials/what-is-...
				프로토콜          서브도메인         메인도메인               path와 page

#### 메서드 :  http요청의 종류 (⭐클라 -> 서버⭐)에 어떤 종류의 요청할 것인가
http 프로토콜이 어떤 약속을 가지고 있는데 그 약속의 종류를 알아보자(메서드)
1.  GET요청 : 어떤 데이터를 조회하고싶어 요청
2.  POST요청 : 어떤 데이터를 생성하고싶어 요청
3.  PUT, PATCH : 어떤 데이터를 수정하고 싶어 요청 (isdone을 true에서 false로 바꾸고싶어)
4.  DELETE :  어떤 데이터를 삭제하고싶어 요청
5.  등등 더 많은 메소드가 있따 [https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods]

#### HTTP 상태코드
클라이언트가 서버에 어떤 요청(request)를 하고 나면, 서버는 응답(response)를 제공한다. 그때 각 응답은 상태코드를 갖는다.
- 1로시작(정보) : 요청을 받았고, 프로세스 계쏙 진행 ------->1xx
- 2로 시작(성공) : 요청 성공적으로 받았고, 인식, 수용했다. ------->2xx
- 3으로 시작(리다이렉션) : 요청 완료를 위해 추가 작업 조치가 필요 ------->3xx
- 4로 시작(클라리언트 오류) : 클라이언트쪽(요청쪽)의 문법이 잘못 되었거나 요청을 처리할 수 ㄴ
- 5로 시작(서버 오류) : 서버가 명백히 유효한요청에 대한 충족을 실패 ------->5xx

![](https://i.imgur.com/ODuFRsp.png)
<4로 시작하는 클라이언트 오류>
이런 느낌!