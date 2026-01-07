# [⭐⭐⭐⭐] Poison Null Byte

### Null Byte Injection

입력값의 끝에 널 바이트(%00 혹은 \0)를 삽입하여 입력값 필터링이나 검증 로직을 우회할 수 있게 되는 취약점이다. 

### Null byte

- 널 바이트(null byte)는 많은 프로그래밍 언어에서 문자열의 끝을 의미한다. (시스템 입장)

- 따라서 이를 활용한 필터링 우회가 가능해진다. 예를 들어, image.jpg 라는 파일이 있을 때 image.jpg%00.exe와 같이 입력하면 실행 가능한 파일인 .exe는 무시되고 jpg로 인식하게 된다.


### 문제 풀이

<p align="center">
  <img src="https://github.com/user-attachments/assets/fd3fc1e5-818a-4bcd-a80e-6d2370386587" width="45%" style="margin-right:10px;"/>
  <img src="https://github.com/user-attachments/assets/3eaf6c9d-f223-4e10-b570-c6a07679d9a7" width="45%"/>
</p>

- package.json.bak 파일은 확장자가 .md 거나 .pdf인 경우에만 접근이 가능하다.

- ftp/package.json.bak%25%30%30.pdf(url encoding)로 null byte를 추가하면 **서버 측**에서는 %00을 끝으로 보고 pacakge.json.bak 파일을 열어준다.

- 그런데 **클라이언트**(브라우저)는 url 전체를 확인하므로 package.json.bak%00.pdf 라는 이름을 가진 파일을 다운로드하려고 한다. 즉, 확장자 필터링 로직이 이 파일을 .pdf 파일로 인식하고 .pdf 파일을 다운로드 받게 되는 것이다. 

### Mitigation Strategy
(node.js v.0.12.0 이상부터는 null byte injection 불가)

> 문자열 끝에 입력되는 null byte를 처리하는 로직을 따로 두어 null byte injection을 해결

- Allowlist 사용: 입력값에 대한 allowlist를 만들어서 allowlist에 있는 입력값이 아니면 모두 차단한다.

- MIME 타입 사용: 파일 이중 확장자 문제를 해결하기 위해 파일의 MIME 타입을 확인한다.

- 인코딩된 null byte 제거(입력값 sanitize): 사용자 입력값을 그대로 쓰지 않고, 인코딩 된 값을 확인한 후 잘못된 입력값이 있다면 이를 제거하고 파일 경로로 사용한다. 

- string concatenation 하지 않기: node.js에 있는 path.join() 함수 혹은 php에 있는 realpath() 함수를 사용하면 사용자가 입력한 문자열을 검열하지 않고 그대로 합치기 때문에 이를 활용한 우회가 쉬워진다. 따라서 file 경로를 조작하는 안전한 함수를 활용해야 한다.

- parameterized queries 사용하기 (prepared statement): 사용자 입력값과 데이터베이스 명령어를 분리하여 데이터베이스에서 일어날 수 있는 모든 종류의 injection을 방어하는 역할을 한다.
 
> parameterized query(매개변수 쿼리)는 SQL 쿼리 문자열 내에 사용자가 입력한 값을 직접 입력하는 대신, placeholder를 사용하여 SQL 쿼리 자체를 먼저 컴파일 하고, 나중에 해당 placeholder를 실제 값으로 채워 쿼리를 실행하는 동작 방식
