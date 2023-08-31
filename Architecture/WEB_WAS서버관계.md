## WAS서버 8080 포트와 WEB서버 

#### Q

user <---> http://WAS:8080 <---> DB

- 상황 : 온라인서비스 환경에서 WAS:8080 으로 직접 접속하여 request/response를 받고 있다.
- 의문점: 사용자는 WEB서버로 접속하여 request/response를 주고받는것이 일반적인 환경이 아닐까?
#### A

user <---> https://도메인 <---> WEB:80 <---> tomcat:8009 <---> DB

- user가 dns주소로 호출 => WEB:80 접속 => WEB이 tomcat:8009 로 전달 => was에서 처리후 다시 WEB으로 response 전달한다.
- 기존 8080 포트로 접속했던 것은 WAS Tomcat의 8080포트로 바로 접속했던 것을 의미한다.


#### Tomcat 포트별 용도
- 8009 : apache와 연결해서 request 받음
- 8080 : 직접 외부에서 http요청을 받음

