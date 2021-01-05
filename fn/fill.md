# Array.prototype.fill()

* `**fill()**` 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채운다.

* fill 메서드는 value, start, end의 3개 인자를 가진다.  start와 end 인자는 선택 사항으로써 기본값으로 각각 0과, this 객체의 length를 가진다.

* 매개변수들

  * `value`배열을 채울 값.

  * `start` Optional) 시작 인덱스, 기본 값은 0.

  * `end` Optional)끝 인덱스, 기본 값은 `this.length`.

* 반환값은 변형한 배열==> 복사본이 아님

```javascript
const array1 = [1, 2, 3, 4];

// fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]

// fill with 5 from position 1
console.log(array1.fill(5, 1));
// expected output: [1, 5, 5, 5]

console.log(array1.fill(6, 2, 3));
// expected output: [1, 5, 6, 5]

```



**:star:length가 배열의 길이일 때, start가 음수이면 시작 인덱스는 length+start이다.**

 end가 음수이면 끝 인덱스는 length+end.

fill은 일반 함수이며, this 값이 배열 객체일 필요는 없다.

fill 메서드는 변경자 메서드로, **복사본이 아니라** this 객체를 변형해 반환한다.

value에 객체를 받을 경우 그 참조만 복사해서 배열을 채웁니다.

```javascript

[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}); // [{}, {}, {}]
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```

