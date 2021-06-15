## 중괄호(`{}`)를 생략하지 않는다

`if`/`while`/`do`/`for` 문 사용 시 한 줄짜리 블록이면 중괄호(`{}`)를 생략하지 않는 것이 좋다. 중괄호가 없을 경우에 제어문의 동작 범위를 한눈에 파악하기 힘들기 때문이다.

```text
▶ 해결책
한 줄짜리 블록에도 {}를 사용하여 명확하게 작성한다.
// Bad
if (condition) doSomething();

if (condition) doSomething();
else doAnything();

for (let prop in object) someIterativeFn();

while (condition) iterating += 1;

// Good
if (condition) {
  doSomething();
}

if (condition) {
  doSomething();
} else {
  doAnything();
}

for (let prop in object) {
  someIterativeFn(object[prop]);
}

while (condition) {
  iterating += 1;
}
```

> 참고
>
> - [ESLint - curly](https://eslint.org/docs/rules/curly)

## `parseInt`는 두 번째 파라미터인 기수를 생략하지 않는다

`parseInt`는 문자열을 정수로 바꿔주는 함수이다. 첫 번째 파라미터는 정수로 바꿀 문자열이고 두 번째 파라미터는 기수(진법)이다. 기수가 생략될 경우 브라우저는 변환될 숫자 형식을 자체적으로 판단하며, 브라우저에 따라 다르게 해석될 수 있다. (문자열이 '0x' 또는 '0X'로 시작하면16진수로, '0'으로 시작하면 8진수 또는 10진수로 간주한다.)

```js
▶ 해결책
항상 두 번째 파라미터를 명시하여 오류를 미리 예방한다.
10진수로 변환이 필요한 경우라면 Number()를 사용하는 것이 속도 측면에서 이득이다.
// Bad
const month = parseInt("08");
const day = parseInt("09");

// Good
const month = parseInt("08", 10);
const day = parseInt("09", 10);

// Tip : 10진수로 변환하는 경우라면 'Number()'를 사용하거나 '+'연산자를 붙이는 것이 더 빠름
const month = Number("08");
const day = +"09";
```

> 참고
>
> - [ESLint - radix](https://eslint.org/docs/rules/radix)

## `switch`문에서 `break`를 생략하지 않는다

`switch`문의 각 `case`절은 `break` 키워드를 만나면 해당 `case`절을 벗어난다. `break` 키워드를 생략하면, `break` 키워드를 만날때까지 다음 `case`절을 연달아 실행하는데, 해당 코드를 읽는 사람에게는 생략이 의도인지, 실수인지 확인하기 힘든 코드가 된다. 예를 들어 첫번째 `case`절에서 `A()`함수를 호출하고 `break`을 생략한 뒤 두번째 `case`절에서 `B()`를 호출하면 두번째 `case`절은 `A()`와 `B()`를 둘 다 수행한다. 이 경우 코드 가독성에 좋지 않은 영향을 주고 혹시라도 `break` 키워드를 실수로 작성하지 않았을 때 버그의 원인을 찾기 힘들다. 그러므로 `case`절 내부에는 반드시 `break`키워드를 작성하도록 한다. 하지만, 다수 `case`절을 한번에 처리하는 경우는 예외로 한다.

```js
▶ 해결책
break를 생략하지 않는다. (단, 다수의 case절이 동일한 기능을 수행할 경우는 break를 생략할 수 있다.)
어떠한 case에도 해당하지 않으면 default를 사용하여 기본 동작을 설정해주도록 한다.
// Bad - break를 생략하여 case 2일 때는 A()과 B()를 수행하는 코드
switch (foo) {
  case 1:
    A()
  case 2:
    B();
    break;
  ...
}

// Good
switch (foo) {
  case 1:
    A()
    break;
  case 2:
    C();  // A(), B()에서 하는 기능을 모두 포함한 C함수
    break;
  ...
}

// Good - 다수의 case절이 동일한 기능을 수행할 경우 break 생략 가능
switch (foo) {
  case 1:
  case 2:
    doSomething();
    break;
  ...
}

// Good
switch (foo) {
  case 1:
    doSomething();
    break;
  case 2:
    doSomethingElse();
    break;
  ...
  default:
    defaultSomething();
}
```

> 참고
>
> - [ESLint - no-fallthrough](https://eslint.org/docs/rules/no-fallthrough)
> - [ESLint - default-case](https://eslint.org/docs/rules/default-case)

## 배열의 순회는 `for-in`을 사용하지 않는다

보통 객체(*Object*)의 프로퍼티 순회가 필요할때는 `for-in`을 사용한다. 자바스크립트의 배열(*Array*)도 객체지만 배열의 요소를 순회할 때는 `for-in`을 사용하지 않아야 한다. `for-in`은 프로토타입 체인에 있는 모든 프로퍼티를 순회하므로 `for`를 사용할 때보다 훨씬 느리다. 게다가 순회 순서는 브라우저에 따라 다르게 구현되어 있어서 배열의 요소 순회가 늘 index 순서대로 수행되지 않을 수 있다.

```js
▶ 해결책
배열을 순회할 때는 for문을 사용한다.
// Bad
const scores = [70, 75, 80, 61, 89, 56, 77, 83, 93, 66];
let total = 0;

for (let score in scores) {
  total += scores[score];
}

// Good
const scores = [70, 75, 80, 61, 89, 56, 77, 83, 93, 66];
let total = 0;
const { length } = scores;

for (let i = 0; i < length; i += 1) {
  total += scores[i];
}
```

## 배열의 요소를 삭제할 때 `delete`를 사용하지 않는다

보통 객체(*Object*)의 프로퍼티를 삭제할 때 `delete`를 사용한다. 단순히 `undefined`로 설정되는 것이 아니라 프로퍼티 자체가 완전히 삭제되어 더 이상 존재하지 않는다. 자바스크립트에서는 배열(*Array*)도 객체이기때문에 `delete`를 사용할 수 있지만, 객체의 프로퍼티 삭제와 조금 다르게 동작한다. 배열에서 요소가 완전히 삭제되어 배열의 길이가 줄어들 것 같지만, 실제로는 해당 요소 값이 `undefined`가 될 뿐 배열 길이는 줄어들지 않는다.

```css
▶ 해결책
배열의 요소를 삭제할 때는 Array.prototype.splice()를 사용하거나 배열의 length 프로퍼티를 변경한다.
// Bad
const numbers = ["zero", "one", "two", "three", "four", "five"];
delete numbers[2]; // ['zero', 'one', undefined, 'three', 'four', 'five'];

// Good
const numbers = ["zero", "one", "two", "three", "four", "five"];
numbers.splice(2, 1); // ['zero', 'one', 'three', 'four', 'five'];

// Tip - 배열 길이를 줄이고 싶다면 length를 사용
const numbers = ["zero", "one", "two", "three", "four", "five"];
numbers.length = 4; // ['zero', 'one', 'two', 'three'];
```


반복문에서 continue를 사용하지 않는다
더글라스 크락포드는 "리팩토링을 통해 continue를 제거했을 때 성능이 향상되지 않은 경우를 본 적이 없다"고 말했을 정도로 continue사용 유무는 성능에 큰 영향을 미친다. 반복문 안에서 continue를 사용하면 자바스크립트 엔진에서 별도의 실행 컨텍스트를 만들어 관리한다. 이러한 반복문은 전체 성능에 영향을 주므로 사용하지 않는다. continue를 잘 사용하면 코드를 간결하게 작성할 수 있지만, 과용하면 디버깅 시 개발자의 의도를 파악하기 어렵고 유지 보수가 힘들다.

▶ 해결책
반복문 내부에서 특정 코드의 실행을 건너뛸 때는 조건문을 사용한다.
// Bad
let loopCount = 0;

for (let i = 1; i < 10; i += 1) {
  if (i > 5) {
    continue;
  }
  loopCount += 1;
}

// Good
for (let i = 1; i < 10; i += 1) {
  if (i <= 5) {
    loopCount += 1;
  }
}
