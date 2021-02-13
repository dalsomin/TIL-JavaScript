### [자바스크립트의 유용한 배열 메소드 사용하기... map(), filter(), find(), reduce()](https://bblog.tistory.com/300)





# [.forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

`forEach`는 가장 기본적인 Loop 메소드입니다.

간단한 예제(배열의 짝수만 출력하는 프로그램)를 통해서 `for` 구문과 비교해 봅시다.

```
// for 구문 버전
var arr = [3, 9, 4, 2, 7, 6];
for (var i = 0; i < arr.length; i++) {
    if (arr[i] % 2 == 0) {
        console.log(arr[i]);
    }
}
// forEach() 버전
var arr = [3, 9, 4, 2, 7, 6];
arr.forEach(function (n) {
    if (n % 2 == 0) {
        console.log(n);
    }
});
```

코드 라인의 수는 같습니다. 하지만 `for` 구문과는 다른 점을 알겠나요?

1. 스코프를 더럽히지 않는다.
   - `for` 구문은 배열의 인덱스를 저장하기 위한 임시 변수 `i`를 할당했습니다. 사실 이 프로그램은 아주 작아서 임시 변수 할당하는 것은 큰 문제가 안됩니다. 그러나 시스템이 커지고 유지보수를 해야 한다면 언제 사용한지 모르는 `i` 때문에 가독성이 떨어지게 됩니다.
2. 요소 접근 방법 `arr[i]` vs `n`
   - `forEach`의 콜백 함수의 첫 번째 인자로 각 요소의 값이 들어옵니다. 덕분에 우리는 깔끔한 방법으로 각 요소의 값을 얻을 수 있습니다.

# Array 메소드 동작 방법

`forEach`와 앞으로 소개 할 메소드는 모두 비슷한 방식으로 동작합니다.

콜백 함수를 통해 각 요소에 대한 정보를 주는 것이 전부입니다. 즉, 배열의 요소가 6개라면 콜백 함수도 6번 호출되고 인자에 각 요소의 값, 인덱스 등이 전달됩니다.

콜백 함수의 인자로부터 전달되는 정보는 다음과 같습니다:

- currentValue: 배열에서 현재 처리 중인 요소.
- index: 배열에서 현재 처리 중인 요소의 인덱스.
- array: forEach()가 적용되고 있는 배열.

참조: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

# [.map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

지금부터 다양한 기능을 제공하는 메소들을 살펴봅시다.

`map` 메소드는 요소를 일괄적으로 변경하는데 효과적입니다. 예제를 봅시다.

```
// 문자열 배열에서 문자열 길이만 획득하기
var arr = ['foo', 'hello', 'diamond', 'A'];
var arr2 = arr.map(function (str) {
    return str.length;
});
console.log(arr2); // [3, 5, 7, 1]
```

`arr`에는 문자열만 `arr2`에는 문자열의 길이만 담겼습니다.

`map`은 콜백 함수의 리턴을 모아서 새로운 배열을 만드는 것이 목적입니다.

위의 예제에서는 `str.length` 문자열 길이만 반환했기 때문에 `arr2`에는 문자열 길이로 이루어진 새로운 배열이 담겼습니다.

`forEach`로는 구현할 수 없을까요? `forEach`가 `map`의 완벽한 상위호환이 되지는 않을까요? 그렇지 않습니다. 우리가 `for` 구문 대신 `forEach`를 사용해야 하는 것처럼 `forEach` 대신 `map`을 반드시 구분해서 사용해야 하는 이유가 있습니다.

forEach로 구현하면 상위 스코프 변수를 수정하면서 사이드 이펙트를 가지게 됩니다:

```
var arr = ['foo', 'hello', 'diamond', 'A'];
var arr2 = [];
arr.forEach(function (str) {
    arr2.push(str.length);
});
console.log(arr2); // [3, 5, 7, 1]
```

콜백 함수만 봐서는 이 콜백 함수가 하는 일을 모두 알 수 없습니다. 사이드 이펙트를 가지기 때문입니다. 이는 추적하기 어려운 코드를 의미합니다.

`map`은 부모 스코프 영역을 건드리지 않고 콜백 함수만으로 목적을 달성합니다. 다른 코드는 신경쓰지 않아도 됩니다.

소개 할 메소드는 함수형 프로그래밍 패러다임에서 아이디어를 착안합니다. 함수형 프로그래밍 패러다임은 순수 함수의 연결로부터 도메인을 해결하는 방법입니다. 순수 함수는 사이드 이펙트를 가지지않는 함수를 말합니다. Input과 Output이 중요할 뿐이지 내부 로직은 다른 영역에 간섭하지 않기 때문에 추적하기 쉽고 간결한 구조가 자연스럽게 형성됩니다.

`for`, `forEach` 또는 다른 메소드로도 서로의 도메인을 해결할 수 있겠지만, 각 목적에 따라서 사용함으로써 직관적이고 가독성있는 코드를 작성하는데 도움 줄 것입니다.

# [.filter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) : 요소들을 걸러낸후 배열을 return

`filter` 메소드는 이름 그대로 요소들을 걸러내는 것이 목적입니다. 예제를 봅시다.

```
// 정수 배열에서 5의 배수인 정수만 모으기
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var arr2 = arr.filter(function (n) {
    return n % 5 == 0;
});
console.log(arr2); // [15, 395, 400, 3000]
```

콜백 함수의 리턴은 `boolean`을 가집니다. 리턴이 `true`인 요소만 모아서 새로운 배열을 만듭니다. 생략하면? 리턴은 `undefined`이므로 `false`가 됩니다.

만족하는 요소가 없다면? 빈 배열이 반환됩니다.

```
var arr = [4, 377, 1024];
var arr2 = arr.filter(function (n) {
    return n % 5 == 0;
});
console.log(arr2); // []
```

`undefined`도 아닌 빈 배열을 반환하는 것은 매우 큰 의미를 가집니다. 보통 도메인을 해결하기 위해서 `Array` 메소드를 여러개 연결하여 사용하는데 빈 배열이라도 반환 함으로써 중간에 오류가 나지 않고 다음 `Array` 메소드를 사용할 수 있습니다.

```
// 5의 배수만 구해서 각 요소를 2배
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var arr2 = arr.filter(function (n) {
    return n % 5 == 0;
}).map(function (n) {
    return n * 2;
});
console.log(arr2); // [30, 790, 800, 6000]
```

이상적인 입력 값은 위와 같겠지만.. 현실은 그렇지 않죠:

```
// 5의 배수만 구해서 각 요소를 2배
var arr = [4, 377, 1024]; // 5의 배수가 없음.
var arr2 = arr.filter(function (n) {
    return n % 5 == 0;
}).map(function (n) { // filter로부터 빈 배열이 반환됨.
    return n * 2;
});
console.log(arr2); // []. map의 콜백 함수는 결국 한 번도 호출되지 않았으나 문제 없음.
```

만약 `filter`로부터 빈 배열이 아닌 결과 없음을 의미하는 다른 값이 반환되었다면 에러를 뿜었을 것입니다.

# [.find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find) : 단하나의 요소만 리턴

`find` 메소드는 `filter`와 비슷하지만 :star:**단 하나의 요소**만 리턴합니다. 예제를 볼까요?

```
// 정수 배열에서 5의 배수인 정수 '하나' 찾기
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var arr2 = arr.find(function (n) {
    return n % 5 == 0;
});
console.log(arr2); // 15
```

그러면 콜백 함수는 몇 번 호출 될까요? 요소의 갯수인 7번?

```
// 정수 배열에서 5의 배수인 정수만 모으기
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var count = 0;
var arr2 = arr.find(function (n) {
    count++;
    return n % 5 == 0;
});
console.log(count); // 2
```

정답은 2번 입니다. 첫 번째는 `4`에 대해서, 두 번째는 `15`에 대해서 호출하겠죠?

즉, `find`는 콜백 함수의 리턴이 `true`인 요소를 찾을 떄 까지 순회하다가 찾으면 거기서 끝납니다. 만약 발견하지 못하면? `undefined`가 반환됩니다.

```
// 정수 배열에서 5의 배수인 정수만 모으기
var arr = [4,  377, 1024];
var arr2 = arr.find(function (n) {
    return n % 5 == 0;
});
console.log(arr2); // undefined
```

`filter`는 찾지 못하면 빈 배열을 반환했습니다. *그런데 `find`는 `undefined`라고요? 그러면 함수를 연결하여 사용할 수 없는 거 아닌가요?* 조금만 생각해보면 `find`는 빈 배열을 반환할 필요가 없음을 알 수 있습니다. 정상적으로 반환하더라도 이미 배열이 아니기 떄문입니다. 따라서 `find`의 반환은 항상 배열이 아니기 때문에 어차피 다른 `Array` 메소드와 연결하여 사용할 일이 없습니다.

# [.reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) : 독특! 유연! 

`reduce` 메소드는 위에서 나온 메소드를 모두 대체할 수 있는 아주 유연한 메소드입니다. `map`, `filter`, `find`로 구현할 수 있는 문제라면 `reduce`로도 구현할 수 있습니다. 하지만 상황에 따라 적절한 메소드를 사용하는 것이 가독성 측면에서 더 유리하겠죠?

먼저 `reduce`의 사용 방법을 봅시다. 조금 특이합니다.

> arr.reduce(callback[, initialValue])

1. callback
   - previousValue: 이전 마지막 콜백 호출에서 반환된 값 또는 공급된 경우 initialValue.
   - currentValue: 배열 내 현재 처리되고 있는 요소(element).
   - currentIndex: 배열 내 현재 처리되고 있는 요소의 인덱스.
   - array: reduce에 호출되는 배열.
2. initialValue: 선택사항. callback의 첫 호출에 첫 번째 인수로 사용하는 값.

참조: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce

다른 메소드와는 다르게 콜백 함수와 initialValue라는 두 번째 인자를 넣을 수 있습니다. 또 `reduce`의 리턴이 중요한데 배열이 될 수도 요소 하나의 값이 될 수도 사용자가 원하는 값 뭐든지될 수 있습니다. 예제를 봐야겠죠?

```
// 배열 요소의 합 계산하기
var arr = [9, 2, 8, 5, 7];
var sum = arr.reduce(function (pre, value) {
    return pre + value;
});
console.log(sum); // 31
```

콜백 함수는 몇 번 호출될까요? 정답은 4번입니다. `2`, `8`, `5`, `7`에 대해서 각각 호출되었습니다. 쉽게 알기 위해서 호출 순서를 보면 다음과 같습니다:

| 호출 순서 |       pre        | value |     return      |
| :-------: | :--------------: | :---: | :-------------: |
|     1     | 9 (첫 번쨰 요소) |   2   | `11` (`9 + 2`)  |
|     2     |        11        |   8   | `19` (`11 + 8`) |
|     3     |        19        |   5   | `24` (`19 + 5`) |
|     4     |        24        |   7   | `31` (`24 + 7`) |

`reduce`의 두 번째 인자인 initialValue를 생략하였기 때문에 첫 번쨰 콜백에서의 `pre`는 첫 번째 요소인 `9`가 전달되었습니다. 이후 콜백에서의 `pre`는 이전 콜백의 리턴이 됩니다.

initalValue가 주어진 경우를 볼까요?

```
// 배열 요소의 합 계산하기
var arr = [9, 2, 8, 5, 7];
var count = 0;
var sum = arr.reduce(function (pre, value) {
    count++;
    return pre + value;
}, 0); // initialValue가 주어졌다!
console.log(sum); // 31
console.log(count); // 5
```

같은 결과지만 콜백 함수의 호출 횟수는 5회 입니다.

사용 방법에 익숙해지셨다면 왜 이 메소드의 리턴이 무엇이든 될 수 있는지 눈치 채셨을 것입니다. 그리고 왜 다른 메소드의 상위 호환이 되는지 감이 오실 겁니다.

위 `map`, `filter`, `find`에서 사용된 예제들을 `reduce`로 해결 해볼까요? 정답을 보기전에 직접 해보시면 도움이 되실겁니다.

```
// map - 문자열 배열에서 문자열 길이만 획득하기
// reduce로 구현
var arr = ['foo', 'hello', 'diamond', 'A'];
var arr2 = arr.reduce(function (pre, value) {
    pre.push(value.length);
    return pre;
}, []);
console.log(arr2); // [3, 5, 7, 1]
// filter - 정수 배열에서 5의 배수인 정수만 모으기
// reduce로 구현
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var arr2 = arr.reduce(function (pre, value) {
    if (value % 5 == 0) {
        pre.push(value);
    }
    return pre;
}, []);
console.log(arr2); // [15, 395, 400, 3000]
// find - 정수 배열에서 5의 배수인 정수 '하나' 찾기
// reduce로 구현
var arr = [4, 15, 377, 395, 400, 1024, 3000];
var arr2 = arr.reduce(function (pre, value) {
    if (typeof pre == 'undefined' && value % 5 == 0) {
        pre = value;
    }
    return pre;
}, undefined);
console.log(arr2); // 15
```

사용 방법이 정말 무궁무진한 메소드죠? 단점도 보셨을 겁니다. `find`를 흉내낼 때 이미 찾은 경우 더 이상 찾지 않게하기 위해서 조건문이 장황해졌죠. `find`는 찾으면 더 이상 콜백 함수가 불리지 않지만 `reduce`는 요소의 수 만큼 항상 호출되는 단점도 있죠.

중요한 것은 상황에 맞는 메소드를 이용하는 것입니다. `reduce`는 `map`, `filter`, `find`로 구현할 수 없는 문제에 대해서 사용하는 것이 가장 바람직합니다.

# Object.keys

다음과 같은 오브젝트가 있습니다:

```
var obj = {
    apple: 500,
    grape: 2000,
    berry: 30
};
```

잘 살펴보면 key에 대한 value 타입이 `number`로 일정합니다. 각 요소 `apple`, `grape`, `berry`의 값의 총합을 구하고 싶지만 배열이 아니라서 `reduce`를 사용할 수 없습니다. 어떻게 할까요? 사실 `for in` 구문을 이용할 수도 있습니다.

```
var obj = {
    apple: 500,
    grape: 2000,
    berry: 30
};
var sum = 0;
for (var prop in obj) {
    sum += obj[prop];
}
console.log(sum); // 2530
```

하지만 위에서 `forEach`를 설명할 때와 마찬가지로 같은 문제들을 가지고 있습니다. 이를 위해서 `Object.keys` 메소드가 있습니다.

이 메소드는 오브젝트의 property를 배열로 만들어 줍니다. 그러니까 `['apple', 'grape', 'berry']`로 변환 해 줍니다. 어떻게 사용하면 좋은지 감이 오시죠?

```
var obj = {
    apple: 500,
    grape: 2000,
    berry: 30
};
var sum = Object.keys(obj).reduce(function (pre, value) {
    return pre + obj[value];
}, 0);
console.log(sum); // 2530
```

한가지 문제점이 있습니다. 사이드 이펙트를 제거하지 못한 것이죠. 콜백 함수에서 `obj`에 접근해야 합니다. 약간 아쉬운 부분이네요.

Object도 배열의 유용한 메소드를 이용할 수 있다는 것을 알 수 있습니다.

# 종합

지금까지 자바스크립트 Array가 지원하는 강력하고 유용한 메소드를 알아봤습니다.

장황한 코드를 간결하고, 도메인을 해결하는 부분만 모듈화하여 추적하기 쉽게 만들어 주어서 매우 유용하죠.

다시 한 번 강조하지만 단순히 `forEach` 대신 다른 메소드를 적절히 사용할 수 있어야 합니다. 또한 콜백 함수에서 상위 스코프의 변수를 건드리는 사이드 이펙트를 발생시키지 말아야 하구요.

아래에 문제 하나를 준비했습니다. 위 문제점 말고도 사용하면서 유의해야 하는 사항이 한가지 있는데요. 문제를 풀면서 알 수 있도록 구성했습니다. 만약 이 글이 도움이 되었다고 느끼셨다면 반드시 풀어보세요.

# 문제

```
var arr = [
  {x1: 1, x2: 1},
  {x1: 2, x2: 2},
  {x1: 3, x2: 3}
];
var arr2 = null;


// 여기에 코드를 작성하여 아래 조건을 만족 시키세요.

/**
1. arr은 변경되면 안됩니다:
[
  {x1: 1, x2: 1},
  {x1: 2, x2: 2},
  {x1: 3, x2: 3}
]
*/
console.log(arr);

/**
2. arr2는 다음과 같아야 합니다:
[
  {x1: 1, x2: 1, result: 1},
  {x1: 2, x2: 2, result: 4},
  {x1: 3, x2: 3, result: 9}
]
*/
console.log(arr2);
```

## 정답

```
var arr = [
  {x1: 1, x2: 1},
  {x1: 2, x2: 2},
  {x1: 3, x2: 3}
];
var arr2 = null;

// 정답
arr2 = arr.map(function (obj) {
    return {
        x1: obj.x1,
        x2: obj.x2,
        result: obj.x1 * obj.x2
    };
});

/**
1. arr은 변경되면 안됩니다:
[
  {x1: 1, x2: 1},
  {x1: 2, x2: 2},
  {x1: 3, x2: 3}
]
*/
console.log(arr);

/**
2. arr2는 다음과 같아야 합니다:
[
  {x1: 1, x2: 1, result: 1},
  {x1: 2, x2: 2, result: 4},
  {x1: 3, x2: 3, result: 9}
]
*/
console.log(arr2);
```

실수하기 쉬운 오답:

```
var arr = [
  {x1: 1, x2: 1},
  {x1: 2, x2: 2},
  {x1: 3, x2: 3}
];
var arr2 = null;

// 오답!
arr2 = arr.map(function (obj) {
    obj.result = obj.x1 * obj.x2;
    return obj;
});

/**
1. arr이 함께 변경됩니다. (X)
[
  {x1: 1, x2: 1, result: 1},
  {x1: 2, x2: 2, result: 4},
  {x1: 3, x2: 3, result: 9}
]
*/
console.log(arr);

/**
2. arr2는 원하는 구조가 되었습니다. (O)
[
  {x1: 1, x2: 1, result: 1},
  {x1: 2, x2: 2, result: 4},
  {x1: 3, x2: 3, result: 9}
]
*/
console.log(arr2);
```

왜 `arr`이 변경 되었을까요? 바로 콜백 함수로 요소의 참조가 넘어왔기 때문입니다. 지금까지 사용했던 예제들은 원시 타입이었습니다. 하지만 이번 문제는 요소가 오브젝트이며, 자바스크립트는 오브젝트를 참조하는 주소를 넘기기 때문입니다. (call by reference)

이 오브젝트를 수정하는 행위 `obj.result = obj.x1 * obj.x2;`는 엄연히 상위 스코프의 변수를 변경하고 있는 것입니다. 콜백 함수의 스코프에서는 반드시 부모 스코프의 영역의 변수를 참조하거나 변경하는 일 없이 콜백 함수 자체 스코프 영역을 벗어나지 않도록 주의하는 것이 좋습니다.

