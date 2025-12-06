# XXE Data Access

### XXE (XML eXternal Entity) Injection

XML 데이터란 Extensible Markup Language(확장 가능한 마크업 언어)로, 데이터를 구조화하고 저장하며, 기계와 사람이 모두 읽을 수 있도록 설계된 반정형 데이터 형식이다. HTML과 유사하게 태그를 활용하지만, XML은 데이터 의미와 구조를 정의한다. XML 문서 예시는 다음과 같다. (출처: https://play-with.tistory.com/288)
```XML
<?xml version="1.0" encoding="UTF-8"?>
<book category="fantasy">
  <title>Harry Potter and the Sorcerer's Stone</title>
  <author>J.K. Rowling</author>
  <year>1997</year>
</book>
```

- XML 데이터를 처리할 때 (= parsing) 발생하는 공격으로 주로 공격자가 서버 시스템 파일을 읽을 수 있게 되거나 백엔드 시스템과 상호작용할 수 있게 되는 취약점이다.

### XXE 취약점 발생 & XXE 공격 예시 (출처: PortSwigger)

> 브라우저(client)와 서버(server) 간에 데이터를 주고 받을 때 XML 형식을 사용하는 경우가 있는데, 이때 주로 표준 라이브러리나 API를 사용해 서버 단에 있는 XML 데이터를 처리한다. XML은 잠재적인 위험 요소들이 존재하는데, 대부분의 처리 도구들이 이 요소들을 다루고 있기 때문에 취약점이 발생한다. (cf) DTD(Document Type Definition) 는 XML 문서의 구조, 요소, 속성 등의 규칙을 정의하는 문법이다. )

- 파일을 찾을 때 XXE 활용

  - DOCTYPE 정의: 파일 경로를 포함하는 외부 엔터티 정의
  - Data Value 수정 (밑의 예시에는 xxe): 정의된 외부 엔터티 사용
 
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>  <!--/etc/passwd 파일을 찾는 구문 삽입-->
<stockCheck>
  <productId>&xxe;</productId>                                <!--&xxe 변수에 /etc/passwd 파일 내용이 들어감-->
</stockCheck>
```

- SSRF 공격을 하기 위해 XXE 활용

  - URL을 활용해 외부 XML 엔터티를 정의한 다음 data value에 정의한 외부 엔터티를 활용
  - application 응답을 data value에 정의한 외부 엔터티에서 확인할 수 있게 되고, 백엔드 시스템과 상호작용할 수 있게 된다.
  - data value에 외부 엔터티를 사용할 수 없으면 blind SSRF 공격만 실행할 수 있다. (이 또한 잠재적 위협 존재) 

```XML
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>
```

- Content-Type 수정으로 XXE 공격 시도

  - 기본적으로 POST method의 content-type은 application/x-www-form-urlencoded인데 이를 수정하여 text/xml로 바꾸어 xml 문서를 파싱하게 할 수 있다. 

```
POST /action HTTP/1.0
Content-Type: text/xml
Content-Length: 52

<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo>
```


### 문제 풀이

1. XML 파일이 업로드 되는 곳을 찾는다.
<p align="center">
  <img src="https://github.com/user-attachments/assets/9910895a-f9cf-4c34-88ea-cbe6746c56f8" width="45%" style="margin-right:10px;"/>
  <img src="https://github.com/user-attachments/assets/658f5412-afdb-4437-abe6-85603f86638c" width="45%"/>

</p>

2. Complaint 탭에서 XML 파일을 업로드할 수 있으므로 업로드한 XML 파일에 악의적인 구문을 추가하여 XXE 공격을 통해 시스템 파일 읽기 시도 -> 성공
   
<img src="https://github.com/user-attachments/assets/74132e8b-d3c2-4c1f-9306-15e8b3339c9d"  width="60%"/>


### Mitigation Strategy

- 대부분의 XXE Injection은 XML을 파싱하는 라이브러리가 필요하지 않은 잠재적으로 위험한 XML 기능을 지원하기 때문이다. 따라서 잠재적으로 위험한 XML 기능을 지원하지 않으면 문제가 가장 쉽게 해결된다. 

- XInclude, DTD, 외부 엔티티 지원을 중지하기

> XInclude를 활용해 다양한 공격이 가능하다. 예를 들면, <xi:inclue> 태그를 활용해 임의의 파일을 시스템 서버 파일에 업로드하거나 SSRF 공격을 시도할 수 있다. 또한, recursive XInclude payload를 이용해 DoS 공격(e.g. XML bomb : XML 파싱 도구가 과도한 메모리와 CPU를 사용해 시스템 마비)을 시도할 수도 있다. 

- 덜 복잡한 데이터 형식을 사용하고 (ex. JSON), 민감한 데이터 나열 피하기 
