Closure는 우리 의지와 상관없이 함수가 선언되는 시점에 생성되고, 함수가 실행되는 도중에 사용될 수 있습니다. 개발자 입장에서 Closure를 사용하는 이유는

#### Closure로 정보은닉과 캡슐화를 제공할 수 있다.

입니다.

Java나 C++로 OOP (객체지향 프로그래밍)를 해보신 분들은 public과 private 키워드를 기억할 것입니다. 일반적으로 외부에서 접근 가능한 변수/함수들은 public으로 선언하고, 외부에서 사용할 필요가 없거나 사용해서 안 되는 변수/함수들은 private를 사용합니다. 아래 코드를 보면



```
a = (function () {
    var privatefunction = function () {
        alert('hello');
    }

    return {
        publicfunction : function () {
            privatefunction();
        }
    }
})();
```

a 내부의 publicfunction 함수는 privatefunction 함수를 호출합니다. publicfunction Scope에는 privatefunction이 없지만 Closure에서 접근할 수 있기 때문에 호출할 수 있습니다. 여기서 **중요한 점**은, 우리가 privatefunction을 직접 호출할 수 없고 공개 된 publicfunction 을 통해 호출할 수 있다는 것입니다. 이는 OOP의 public/private 키워드 처럼 공개할 것과 공개하지 않을 것을 정할 수 있고 이를 통해 정보은닉과 캡슐화가 가능하다는 말입니다.

closure를 사용하는 **한 가지 시나리오**를 보여주고 글을 마무리 하겠습니다.

사용자가 버튼을 클릭할 때, count를 세는 기능을 만든다고 가정하겠습니다.

버튼을 클릭할 때, onclick 이벤트가 발생 해 updateClickCount 함수가 호출 되도록 코드를 만들었습니다.

```
<button onclick="updateClickCount()">click me</button>
```

여기서 updateClickCount 함수를 만드는 여러 방법들이 있습니다.

1. global 변수를 선언하고 함수에서 counter를 증가시킨다.

```
var counter = 0;

function updateClickCount() {
    ++counter;
}
```

이 방법의 문제는 updateClickCount 함수의 호출 없이 페이지에 있는 다른 스크립트에 의해 counter 값이 0으로 초기화 될 수 있습니다.

\2. 변수를 함수안에 넣는다면.

```
function updateClickCount() {
    var counter = 0;
    ++counter;
}
```

함수 호출 할 때마다 couter 값이 0 이 되버리는 문제가 있네요 ^^;

\3. 중첩 함수를 이용해볼까?

```
function countWrapper() {
    var counter = 0;
    function updateClickCount() {
    ++counter;
    }
    updateClickCount();    
    return counter; 
}
```

위 코드에서 updateClickCount는 중첩 된 함수 countWrapper Scope에 선언 된 변수 counter에 대해 접근할 수 있습니다. 이 방법은 counter에 대한 문제를 해결하지만, 외부에서 updateClickCount함수를 직접 호출하면 counter=0을 단 한번 호출하도록 방법을 찾아야 합니다.

\4. Closure가 살길이네여 ㅇ.ㅇ

```
var updateClickCount=(function(){
    var counter=0;

    return function(){
     ++counter;
    }
})(); // 즉시 호출
```

위에서 updateClickCount는 선언과 동시에 딱 한번 실행됩니다. 내부에서 counter 값을 0으로 set하고 익명의 함수를 리턴합니다. 리턴되는 익명의 함수는 Closure를 통해 counter의 값에 접근할 수 있습니다. Closure를 통해 리턴되는 함수가 private 변수 counter를 가지도록 해주는 셈이죠. 변수 counter는 익명 함수의 Scope에서 안전하게 보호되고 함수를 통해서만 값의 변경을 허용합니다.

