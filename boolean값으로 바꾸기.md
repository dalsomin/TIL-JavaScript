1. !! 연산자를 사용해 불리언 값으로 바꾸기
종종 우리는 어떤 변수가 존재하거나 유효한 값을 가지는지를 참/거짓 여부로 판별해야 할 때가 있다. !!(이중부정 연산자)를 이용해 이런 검증을 할 수 있다.

간단히 !!variable 이라는 형태로 표현하는 것 만으로도 어떤 형태의 데이터도 불리언 값으로 변화시킬 수 있고, 0, null, "", undefined, NaN 등의 경우엔 false 를, 이외의 경우엔 true 를 반환하게 할 수 있다.

아래 예제를 보며 사례를 통해 이해해보자.

function Account(cash) {
	this.cash = cash;
	this.hasMoney = !!cach;
}

var account = new Account(100.50);
console.log(account.cash); // 100.50
console.log(account.hasMoney); // true

var emptyAccount = new Account(0);
console.log(emptyAccount.cash); // 0
console.log(emptyAccount.hasMoney); // false
이 경우, account.cash가 0이 아니라면 account.hasMoney는 true 가 된다.
