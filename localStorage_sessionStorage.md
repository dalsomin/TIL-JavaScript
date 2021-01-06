## 1. local Storage

- key , value 저장소 
- `localstorage.setItem(key, value)` 함수로 저장
- `localstorage.getItem(key);` 로 value값을 가져올수있다
  - `localStorage[key]`로도 접근가능하다. 
- `localstorage.removeItem(key);` 는 삭제
- `localstorage.clear(key);` 는 모두삭제
- 사용자 세션의 데이터 저장에 용이
- 브라우저를 닫았다가 다시 열엇을때에도 지속된다. 
- 탭을 여러개 열어도 공유된다.
- 명시적으로 삭제될때까지 지속된다. 
- 변경사항은 저장되어 현재 및 향우 사이트 방문시 사용할 수 있다.
- 입력은 무조건 string으로 처리된다. 
- 최대 약 5mb용량
- 크롬은 SQlite를 사용한다. 

==> 즉 사용자가 브라우저 창을 닫았을 때 데이터는 삭제되지 않으며 일, 주 , 월 및 연도에 사용할 수 있는 만료날짜 없이 사용자 정보 데이터를 저장한다. 

```javascript
localStorage.setItem('name', 'somin');
localStorage.getItem('name'); // somin
localStorage.remove('name'); 
localStorage.clear();
```







## 2. session Storage

* 브라우저 세션 기간동안만 사욯할 수 있으며 탭이나 창을 닫을 때 삭제된다.
* 새로고침을 해도 유지
* 변경된 사항은 현재 페이지에서 닫힐때까지 저장되어 사용할 수 있다.
* 탭이 닫히면 저장된 데이터가 삭제된다. 
* 메서드는 localStorage와 동일 