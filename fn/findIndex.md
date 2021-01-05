# Array.prototype.findIndex()

* 오늘 앵귤러를 하면서 나왔는데 쉬운건데 헷갈렸다...ㅡㅡ
* **`findIndex()`** 메서드는 주어진 판별 함수를 만족하는 배열의 **첫 번째 요소**에 대한 **인덱스**를 반환한다. 만족하는 요소가 없으면 **-1을 반환 **
* `find()`는 인덱스 대신 **값을 반환**한다. 

```javascript
$scope.remove = function (todo) {
    //find todo index in todos
    var idx = $scope.todos.findIndex(function (item) {
        return item.title === todo.title;
    })
    //if문을 쓰지 않고 그냥 이렇게 return 에다가 넣어버린다.

    //remove from todos
    if (idx > -1) {
        $scope.todos.splice(idx, 1)
    }
}
```



```javascript
//이 또한 가능
const array1 = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13; 
//13이상의 값만 반환하는 함수 

console.log(array1.findIndex(isLargeNumber));
```

```javascript
//소수찾기
function isPrime(element, index, array) {
  var start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].findIndex(isPrime)); // -1, not found
console.log([4, 6, 7, 12].findIndex(isPrime)); // 2
```

