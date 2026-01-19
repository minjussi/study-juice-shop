# [⭐⭐⭐⭐] Allowlist Bypass

### 📌 WhiteList

- 특정 권한, 서비스, 접근 등을 허용하는 대상을 명시적으로 지정한 목록이며, 이 목록에 포함된 대상만 허용하고 그 외의 모든 것은 차단하는 방식
- 

### 🔓 문제 풀이 

<p align="center">
  <img src="https://github.com/user-attachments/assets/4ad7a001-1b6f-417a-bf6c-f11c35c2b66b" width="45%"/>
</p>

- 다른 페이지로 이동하는 url을 확인할 때 include를 활용하고 있어 해당 url을 포함하기만 하면 이동이 가능하게 된다. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/e6576756-3c58-4529-875c-669773d505cb" width="45%"/>
</p>

- redirect?to= 뒤에 오는 파라미터에 화이트리스트에 있는 url을 삽입하면 검증을 우회할 수 있게 된다. 

### 🔐 Mitigation Strategy
