# [⭐⭐⭐⭐⭐⭐] Arbitrary File Write

### 📌 Zip Slip

- 압축 해제 과정에서 발생하는 path traversal 취약점으로,  

### 🔓 문제 풀이

<img width="1145" height="513" alt="image" src="https://github.com/user-attachments/assets/fd61e1ee-033b-45dc-b52f-13eb9a647ac2" />

위의 코드를 보면 압축 파일을 해제할 때, 압축 파일 내에 있는 파일명을 확인하지 않고 그대로 경로에 추가하고 있는 것을 확인할 수 있다.

따라서 압축 파일 내의 파일명을 ../../etc/pwd처럼 path traversal이 가능하게 만들면, 경로를 조작해 임의의 파일을 업로드할 수 있게 된다. 

1. legal.md라는 파일을 새로 만든다.
2. 악성 zip 파일을 만들 python 스크립트를 작성한다. 이때 legal.md 파일명을 ../../ftp/legal.md로 만들어 path traversal이 가능하게 만든다. 
```python
import zipfile

with zipfile.ZipFile('exploit.zip', 'w') as z:
    z.write('legal.md', '../../ftp/legal.md')
```
3. python 스크립트를 실행해 exploit.zip을 만들고, 해당 압축 파일을 complaint 탭에 업로드한다.
4. ftp/legal.md 디렉토리에 업로드된 것을 확인할 수 있다.
<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/5b6298cd-5eeb-4a46-891c-3c1f347be915" />


### 🔐 Mitigation Strategy

1. 파일명 유효성 검사
2. 
