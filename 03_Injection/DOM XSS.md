# [⭐] DOM XSS

### Cross-site scripting (XSS)

XSS는 취약한 웹사이트를 조작해 사용자에게 악의적인 Javascript 실행 결과를 제공하는 웹 취약점의 일종이다. 악의적인 코드가 피해자의 브라우저에서 실행되면, 공격자는 피해자의 권한으로 브라우저를 임의로 조작할 수 있게 된다. 

### XSS PoC

- 통상적으로 취약점을 확인하기 위해 짧은 javascript 코드인 alert() 함수를 많이 사용했다.

- 크롬 92 버전부터는 cross-origin iframe 안에서 alert()의 호출이 막혔다. (iframe은 다른 사이트 콘텐츠를 삽입(cross-origin)할 때 자주 사용하는 HTML 태그인데, 보안 강화 차원에서 막혔다.(same-origin Policy 때문))

- 이런 경우에는 print() 함수를 대신해서 사용해 브라우저에서 인쇄 대화창을 활용해 실행 결과를 확인할 수 있다. 


### DOM-based XSS 개념 (출처 : PortSwigger)

- DOM-based XSS는 Javascript에서 공격자가 제어할 수 있는 source에 있는 데이터를 가져와 eval()이나 innerHTML 같은 동적 코드를 실행하는 함수에 전달해 공격자가 임의로 넣은 코드를 클라이언트 단에서 실행할 수 있게 한다. (특히 타인의 계정을 훔칠 때 유용하게 사용된다.)
  
- DOM-based XSS를 실행하려면 해당 source에 내가 넣어서 공격하고자 하는 data를 삽입해 Javascript에서 실행되도록 만들어야 한다. 
  
- 가장 흔하게 공격자가 제어할 수 있는 source는 url로, 취약한 페이지로 이동하게 하는 링크를 만들어 전달하는 방법이 있다.


### 문제 풀이

- DOM-based XSS 공격을 시도하려면, 공격을 시도해 임의의 코드를 실행하게 하는 위치를 먼저 찾아야 한다. (payload는 정해져 있다- <iframe src="javascript:alert(`xss`)">)

- 첫 번째로 시도해 본 것은 localhost:3000/#/<iframe src="javascript:alert(`xss`)"> 와 같이 url에 공격 payload를 삽입하는 것이었다. (이는 url에 실행할 수 있는 payload를 삽입한 것이 아니라 단지 디렉토리 이동을 한 것에 불과함으로 실행하고자 하는 코드가 실행이 될 수 없다.)

<img width="2811" height="1469" alt="image" src="https://github.com/user-attachments/assets/d8ff1a93-4ecf-4bba-bd91-60a2e68a0aa4" />




- 검색창의 url을 살펴보면 *search? q=* 와 같이 url 파라미터 값을 전달해 검색을 진행하는 것을 알 수 있다. 검색 파라미터 값으로 공격자가 임의로 실행하고자 하는 payload를 삽입하면 해당 코드가 실행될 수 있음을 알 수 있다. 
(http://localhost:3000/#/search?q=%20%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E)




### Mitigation Strategy


<p align="center">
  <img src="https://github.com/user-attachments/assets/28598f0c-1462-44e2-979d-6e8ad349875e" width="45%" style="margin-right:10px;"/>
  <img src="https://github.com/user-attachments/assets/6170b56b-21e0-4957-96ee-f7aca884fa3b" width="45%"/>
</p>


- 코드를 더 자세히 살펴보면 파라미터로 입력된 값이 sanitizer를 우회하는 것이 가능한 코드가 있는 것을 확인할 수 있다.

- 이를 오른쪽처럼 입력값이 sanitizer를 거쳐 필터링을 거친 다음에 입력되도록 수정하면 보안 조치를 할 수 있다. 
