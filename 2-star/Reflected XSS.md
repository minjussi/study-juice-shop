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

### Mitigation Strategy (OWASP-Cross Site Scripting Prevention Cheat Sheet)

- 입력값 검증 (input validation): 사용자로부터 데이터를 입력 받을 때 입력 형식을 지정하여 걸러내는 방법이다. 하지만 이 방법은 우회가 쉽기 때문에 추천하는 방법은 아니다.

- 출력 인코딩 (output encoding): 사용자가 입력한 데이터를 서버 측에 전달할 때, 코드가 아닌 글자로 변환해주는 방법이다. 이 방법을 활용하면 사용자가 어떤 값을 입력하더라도 글자만 보여주게 되어 공격을 방어할 수 있다.
- 웬만하면 사용자 입력을 css에 직접 넣는 설계는 피하는 것이 좋다.
- url에 파라미터로 값을 넘길 때는 (ex. <a href=="http://site.com?search= ">) url 인코딩으로 방어할 수 있다. 그러나 href 전체 입력을 사용자 입력으로 받으면, 이는 url 인코딩으로 방어할 수 없게 된다. 이때는 url 전체가 안전한 값인지를 검증하는 과정이 필요해지므로 href 전체를 사용자 입력으로 받는 것은 지양해야 한다. 

```html
<!-- html 본문에 넣는 경우에는 특수문자를 HTML 엔티티로 변환하여 html escape하기 -->
&    &amp;
<    &lt;
>    &gt;
"    &quot;
'    &#x27;

<!-- html attribute(속성값)로 들어갈 때는 알파벳, 숫자를 제외하고는 모두 escape하기-->
<!-- html 태그가 꼭 필요하면 sanitize 라이브러리를 사용할 것 -->
```

```javascript
// javascript: 가장 까다로운 경우로
// 모든 문자를 유니코드 형태로 바꾸어야 한다.
// 그 이유는 백슬래시(\) 만으로는 완벽한 방어가 안 되기 때문이다. (; 같은 문자를 사용하면 명령이 끝나버릴 수 있다)
ex ) alert -> \u0061\u006c\u0065\u0072\u0074 로 변환 
```

- CSP(Content Security Policy): 서버 측 설정을 바꾸어 허락 맡은 스크립트 외에 모든 스크립트를 실행하지 못하게 클라이언트에게 명령하는 방법이다. 
