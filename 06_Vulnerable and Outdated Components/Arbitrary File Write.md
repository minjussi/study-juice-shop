# Arbitrary File Write

### Zip Slip

- zip slip:

- 

### 문제 풀이

<img width="1145" height="513" alt="image" src="https://github.com/user-attachments/assets/fd61e1ee-033b-45dc-b52f-13eb9a647ac2" />

위의 코드를 보면 압축 파일을 해제할 때, 압축 파일 내에 있는 파일명을 확인하지 않고 그대로 경로에 추가하고 있는 것을 확인할 수 있다.

따라서 압축 파일 내의 파일명을 ../../etc/pwd처럼 path traversal이 가능하게 만들면, 경로를 조작해 임의의 파일을 업로드할 수 있게 된다. 

**문제 요구 조건** : legal.md를 임의의 파일로 바꾸는 것

(사진 첨부)

### Mitigation Strategy
