# [⭐⭐⭐⭐⭐] Local File Read

### 📌 File Inclusion Vulnerability

- LFI(Local File Inclusion): 공격자가 웹 애플리케이션을 속여 서버 내부에 존재하는 파일을 읽거나 실행하게 만드는 취약점이다. 

### 📌 CVE-2021-32820

- Express의 Handlebars라는 템플릿 엔진(node.js)에서 발생하는 취약점으로 render 함수 인자로 파일 경로를 알려줄 수 있게 되면 파일이 유출되는 취약점이다.
  - 공격자가 layout 파라미터를 조작할 수 있게 되면 서버 내의 임의의 파일을 템플릿 레이아웃으로 지정하여 그 내용을 읽을 수 있게 되는 것이다.  

### 🔓 문제 풀이

- Request Data Erasure 기능은 url에 입력된 파라미터를 그대로 받아 화면을 렌더링하는 구조를 가지고 있다.

<img width="1500" height="1278" alt="image" src="https://github.com/user-attachments/assets/15186937-4846-4998-b002-e6f504fe1837" />


- 만약 서버 내의 코드가 다음과 같이 이뤄져 있다면 req.query가 그대로 렌더링 옵션으로 들어가게 된다. 
```
app
```
- layout 속성을 주입하여 템플릿 레이아웃을 원하는 파일로 지정할 수 있게 되어 해당 파일이 유출된다.
<img width="1103" height="319" alt="image" src="https://github.com/user-attachments/assets/05f784a3-96c1-4c1a-a883-69251e1df662" />



### 🔐 Mitigation Strategy

- 라이브러리 업데이트: 취약점이 패치된 이후 버전으로 업데이트한다. 
