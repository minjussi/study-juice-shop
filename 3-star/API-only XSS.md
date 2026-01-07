# API-only XSS

### API (Application Programming Interface)



### XSS attack via an API

- 발생 원인
  - API가 신뢰할 수 없는 사용자 입력을 받고,
  - 입력에 대한 적절한 검증 없이 응답할 때 발생한다.
 
- 작동 방식

1. 공격자가 API를 통해서 악성 스크립트를 전달
2. 

### 문제 풀이

이 문제에서는 fronted application을 사용하지 않고 해결하라고 했기 때문에 reflected가 아니라 stored xss 공격을 수행해야 한다.



### Mitigation Strategy
