\Promise API

## **resolve/reject**



```
const promise = new Promise((resolve, reject) => {
  getData(
    response => resolve(response.data), 
    error => reject(error.message)
  )
})
```

## **then/catch**



```
promise
  .then(data => console.log(data))
  .catch(err => console.error(err))
```

## **all**

**Promise.all**은 모두 **이행**상태일 때 **then**을 통해 결과를 받게 된다.



```
Promise
  .all([
    getPromise(),
    getPromise(),
    getPromise()
  ])
  //response all data
  .then(data => console.log(data))
  .catch(err => console.error(err))
```

**하나**라도 **거부**상태가 되면 **catch**가 실행되게 된다.



```
Promise
  .all([
    Promise.resolve(1),
    Promise.reject(2),
  ])
  .catch(err => console.error(err))
```
