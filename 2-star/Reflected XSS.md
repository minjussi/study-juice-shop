# Reflected XSS

### Reflected XSS

Reflected XSS는 서버 측 취약점으로 악성 스크립트가 반사되는 방식으로 일어나는 공격이다.

1. 공격자가 악성 스크립트가 담긴 링크를 제작 (<script>alert(1)</script>)
2. 링크를 클릭하거나 서버로 전송하면, 서버는 이 검색을 받아서 검색 결과를 바로 응답 (DB에 저장하지 않고 바로 반사)
3. 클라이언트는 서버가 준 링크이기 때문에 따로 검증 과정을 거치지 않고 스크립트를 실행 

- DOM XSS와의 차이점: DOM XSS는 클라이언트의 자바스크립트가 사용자 입력을 문서 객체 모델 (DOM)에 잘못 처리해서 발생하는 반면, Reflected XSS는 서버가 사용자의 입력을 받아 동적 페이지 생성 시 악성 스크립트를 반사한다. (즉 반영해서 실행)

### Reflected XSS가 위험한 이유

피해자가 링크를 클릭하게 만들어 정보를 탈취할 수 있다. 

- 공격자가 악성 스크립트를 만들어 피해자에게 배포하면, 피해자는 신뢰할 수 있는 도메인이기 때문에 해당 링크를 클릭하게 된다.
- 이를 통해 피해자의 개인 정보를 탈취하거나, 피해자의 세션 ID를 탈취하여 피해자로 로그인하여 해당 권한을 이용할 수 있게 된다.

### 문제 풀이

<p align="center">
  <img src="https://github.com/user-attachments/assets/cde23300-a516-4a1a-bc0c-eeb831c163ff" width="45%" style="margin-right:10px;"/>
  <img src="https://github.com/user-attachments/assets/75929614-5691-496a-a1cd-d4a91b30cb1b" width="45%"/>
</p>

- Reflected XSS는 서버가 내 요청(파라미터)을 받아서 응답(반사)해 줄 때 발생하는 취약점이다.
- 따라서 파라미터(?id = 혹은 ?q= )에 <iframe src="javascript:alert(`xss`)">을 주어 서버가 악성 url을 실행하게 한다.

### Mitigation Strategy
