## base64란? = 64진법

1. base64인코딩은 임의의 바이트 스트림을 화면에 표시할 수 있는 ASCII문자로 바꾸는 인코딩 방식을 말한다. 
   이 인코딩은 인터넷 전자메일을 전송할때 MIME의 content transfer encoding의 하나로 정의된다. 
2. 한마디로 정의하면 64진법 데이터이다. 
3. base64인코딩은 데이터가 전송중에 수정되지 않고 그대로 전송되는 것을 보장한다. 
4. base64는 일반적으로 MIME를 통한 전자메일 또는 복잡한데이터를 XML로 저장하는 등 여러가지 응용프로그램에서 사용된다. 
5. 자바스크립트에서 문자열을 base64로 인코드하려면 window객체의 **btoa메소드**를 사용하면된다. 

​	[ base64로 인코드하려면 btoa ]

6. base64로 인코드된 문자열을 디코드하려면 자바스크립트의 window객체의 **atob메소드**를 이용하면된다. 

​	[ base64로 된 문자열을 디코드하려면 atob ]

```javascript
var str = "somin is wise."
var encode_str = window.btoa(str);
console.log(endcode_str);
var decode_str = window.atob(str);
console.log(decode_str);
```

