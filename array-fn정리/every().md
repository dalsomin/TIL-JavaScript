### every()

```javascript
const isBelowThreshold = (currentValue) => currentValue ;

const array1 = [true, false, true, true];

console.log(array1.every((arr)=>arr));
```

true, false인것 골라내기 유용하다. 

원래 예시는 다음과 같다.

```javascript
const isBelowThreshold = (currentValue) => currentValue < 40;

const array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
```

---

### user_userRoleList.json

```json
[
  {
    "user": {
      "id": "333",
      "name": "user333",
    },
    "userRoles": {
      "id": "333",
      "roles": [
        {
          "id": "mmm",
          "name": "member"
        },
        {
          "id": "rrr",
          "name": "read",
        },
        {
          "id": "aaaa",
          "name": "admin",
        }
      ]
    }
  },
  {
    "user": {
      "id": "111",
      "name": "user111",
    },
    "userRoles": {
      "id": "349b7771c6b44cf7bbb8ce76273a84d5",
      "roles": [
        {
          "id": "mmm",
          "name": "member"
        },
        {
          "id": "aaa",
          "name": "admin",
        }
      ]
    }
  },
  {
    "user": {
      "id": "222",
      "name": "user222",
    },
    "userRoles": {
      "id": "222",
      "roles": [
        {
          "id": "roleid_111",
          "name": "role111"
        }
      ]
    }
  }
]

```

위의 데이터를 이용해서 가공해야하는 문제가 있었다. ...
문제의 그 함수

```javascript

        const handleValidateCheck = () => {
            console.log(
                '유효체크'!,
                user_userRoleList,
                user_userRoleList.length,
            );

            if (user_userRoleList.length === 0) {
                return false;
            } else {
                return user_userRoleList
                    .map((result) => {
                        return result.userRoles.roles.length === 0
                            ? false
                            : true;
                    })
                    .every((arr) => arr);
            }
        };

```
