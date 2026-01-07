# [⭐⭐⭐] API-only XSS

### XSS attack via an API

- 발생 원인
  - API가 신뢰할 수 없는 사용자 입력을 받고,
  - 입력에 대한 적절한 검증 없이 응답할 때 발생한다.
 
- 작동 방식

  1. 공격자가 API를 통해서 악성 스크립트를 전달
  2. Stored XSS인 경우에는 악성 스크립트가 영구적으로 서버에 저장되는 반면, Reflected XSS인 경우에는 악성 스크립트에 대한 응답이 바로 나옴
  3.  

### XMLHttpRequest(XHR) API

XMLHttpRequest는 javascript에서 기본으로 제공하는 객체로, 웹페이지 전체를 새로고침 하지 않고도 서버와 데이터를 교환할 수 있게 해준다. 

### 문제 풀이

이 문제에서는 fronted application을 사용하지 않고 해결하라고 했기 때문에 reflected가 아니라 stored xss 공격을 수행해야 한다.



### Mitigation Strategy
