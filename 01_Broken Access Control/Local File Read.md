# [⭐⭐⭐⭐⭐] Local File Read

### 📌 File Inclusion Vulnerability

- LFI(Local File Inclusion): 공격자가 웹 애플리케이션을 속여 서버 내부에 존재하는 파일을 읽거나 실행하게 만드는 취약점이다. 

### 📌 CVE-2021-32820

- Express의 Handlebars라는 템플릿 엔진(node.js)에서 발생하는 취약점으로 render 함수 인자로 파일 경로를 알려줄 수 있게 되면 파일이 유출되는 취약점이다.
  - 공격자가 layout 파라미터를 조작할 수 있게 되면 서버 내의 임의의 파일을 템플릿 레이아웃으로 지정하여 그 내용을 읽을 수 있게 되는 것이다.  

### 🔓 문제 풀이

- Request Data Erasure 기능은 url에 입력된 파라미터를 그대로 받아 화면을 렌더링하는 구조를 가지고 있다.

- 만약 서버 내의 코드가 다음과 같이 이뤄져 있다면 req.query가 그대로 렌더링 옵션으로 들어가게 된다. 
```
app.get('/', (req, res) => {
  const name = 'Express.js';
  res.render('index', {name});
});
```
- layout 속성을 주입하여 템플릿 레이아웃을 원하는 파일로 지정할 수 있게 되어 해당 파일이 유출된다.
<p>
  <img src="https://github.com/user-attachments/assets/912d1240-96a3-4690-8bf0-0aac25534ae3" width="45%" style="margin-right:10px;"/>
  <img src="https://github.com/user-attachments/assets/5e71af97-94dd-4e69-9d21-7f532980f237" width="45%"/>
</p>

### 🔐 Mitigation Strategy

- 라이브러리 업데이트: 취약점이 패치된 이후 버전으로 업데이트한다.
- 입력값 검증 및 분리: res.render 함수 안에서 입력 객체를 검증한 후에 필요한 부분만 추출해서 전달하게 한다. 
