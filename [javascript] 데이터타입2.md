### 불변객체 (Immutable object)

불변객채는 최근에 Recact, vue.js, Angular 등의 라이브러리나 프레임워크에서뿐만아니라 함수형 프로그래밍, 디자인 패턴 등에서도 매우 중요한 기초가 되는 개념이다.

<객체의 가변성에 따른 문제점>

```javascript
var user = {
  name: "Kato",
  gender: "male",
};

var changeName = function (user, newName) {
  var newUser = user;
  newUser.name = newName;
  return newUser;
};

var user2 = changeName(user, "Ukato");

if (user !== user2) {
  consolo.log("유저 정보가 변경되었습니다.");
}
consolo.log(user.name, user2.name); // Ukato Ukato
consolo.log(user === user2); // true
```

---

만약 바뀌기 전의 정보와 바뀐 후의 정보의 차이를 가시적으로 보여줘야 하는 등의 기능을 구현하려면 불변객체의 속성을 사용해야 한다. 아래의 코드처럼 변경해야 한다.

```javascript
var user = {
  name: "Kato",
  gender: "male",
};

var changeName = function (user, newName) {
  return {
    name: newName,
    gender: user.gender,
  };
};

var user2 = changeName(user, "Ukato");

if (user !== user2) {
  consolo.log("유저 정보가 변경되었습니다.");
}
consolo.log(user.name, user2.name); // kato Ukato
consolo.log(user === user2); // false
```

changeName 함수가 새로운 객체를 반환하도록 수정했다. 이제 user와 user2는 서로 다른 객체이므로 안전하게 변경 전과 후를 비교할 수 있다.

---

얕은 복사(shallow copy)는 바로 아래 단계의 값만 복사하는 방법이고, 깊은 복사(deep copy)는 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법이다.

중첩된 객체에 대한 얕은 복사

```javascript
var user = (
  name: 'kato',
  urls: {
    portfolio: '',
    blog: '',
    fackbook: ''
  }
);
var user2 = copyObject(user);

user2.name = 'Lee'
console.log(user.name === user2.name) // false

user.urls.portfolio = '';
console.log(user.urls.portfolio === user2.urls.portfolio); // true

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog); // true
```
