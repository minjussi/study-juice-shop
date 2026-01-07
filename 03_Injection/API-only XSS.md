# [⭐⭐⭐] API-only XSS

### 📌 XSS attack via an API

- 발생 원인
  - API가 신뢰할 수 없는 사용자 입력을 받고, 입력에 대한 적절한 검증 없이 응답을 보낼 때 발생한다.
  - 공격자가 API 앤드포인트로 직접 요청을 보내면, 프론트엔드의 유효성 검사를 우회할 수 있다. 
 
- 작동 방식

  1. 공격자가 API를 통해서 악성 스크립트를 전달한다. 
  2. Stored XSS인 경우에는 악성 스크립트가 영구적으로 서버에 저장된다. 
  3. Reflected XSS인 경우에는 악성 스크립트에 대한 응답이 바로 나온다. 

### 📌 XMLHttpRequest(XHR) API

XMLHttpRequest는 javascript에서 기본으로 제공하는 객체로, 웹페이지 전체를 새로고침 하지 않고도 서버와 데이터를 교환할 수 있게 해준다. 
공격자는 XHR을 활용하거나 도구를 활용해 서버 API에 직접 악성 스크립트를 전달할 수 있다. 

### 🔓 문제 풀이

이 문제에서는 fronted application을 사용하지 않고 해결하라고 했기 때문에 입력창이 아닌 API 요청 도구를 사용해야 한다. 



### 🔐 Mitigation Strategy
