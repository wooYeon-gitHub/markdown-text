#### 실행 컨텍스트가 뭐지?

**실행 컨텍스트(Execution context)**는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념입니다.

> 자바스크립트는 어떤 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 위로 끌어올리고(**호이스팅**), 외부 환경 정보를 구성하고, `this`값을 설정하는 등의 동작을 수행하는데, 이로인해 다른언어에서는 발견할 수 없는 특이한 현상들이 발견됩니다.

#### **실행 컨텍스트 유형**

JavaScript에는 세 가지 유형의 실행 컨텍스트가 있습니다.

- **Global Execution Context**
  이것은 기본 또는 기본 실행 컨텍스트입니다. 함수 내부에 없는 코드는 전역 실행 컨텍스트에 있습니다. 두 가지 작업을 수행합니다. 창 개체인 전역 개체(브라우저의 경우)를 만들고 의 값을 this전역 개체와 동일하게 설정합니다. 프로그램에는 하나의 전역 실행 컨텍스트만 있을 수 있습니다.
- **Functional Execution Context**
  함수가 호출될 때마다 해당 함수에 대한 새로운 실행 컨텍스트가 생성됩니다. 각 함수에는 자체 실행 컨텍스트가 있지만 함수가 호출되거나 호출될 때 생성됩니다. 함수 실행 컨텍스트는 얼마든지 있을 수 있습니다. 새 실행 컨텍스트가 생성될 때마다 이 문서의 뒷부분에서 설명할 정의된 순서로 일련의 단계를 거칩니다.
- **Eval Function Execution Context**
  함수 내에서 실행되는 코드 eval 도 고유한 실행 컨텍스트를 가지지만 eval 일반적으로 JavaScript 개발자가 사용하지 않으므로 여기서는 다루지 않겠습니다.

#### Execution Stack

다른 프로그래밍 언어에서 "호출 스택"이라고도 하는 실행 스택은 코드 실행 중에 생성되는 모든 실행 컨텍스트를 저장하는 데 사용되는 LIFO(Last in, First out) 구조의 스택입니다.

JavaScript 엔진이 스크립트를 처음 발견하면 전역 실행 컨텍스트를 생성하고 현재 실행 스택으로 푸시합니다. 엔진이 함수 호출을 찾을 때마다 해당 함수에 대한 새 실행 컨텍스트를 만들고 스택 맨 위로 푸시합니다.

엔진은 실행 컨텍스트가 스택의 맨 위에 있는 함수를 실행합니다. 이 함수가 완료되면 실행 스택이 스택에서 제거되고 현재 스택에서 그 아래에 있는 컨텍스트에 제어가 도달합니다.

---

아래 코드 예제를 통해 이를 이해해 보겠습니다.

```javascript
let a = "안녕하세요!";
function first() {
  console.log("첫 번째 함수 내부");
  second();
  console.log("다시 첫 번째 함수 내부");
}
function second() {
  console.log("두 번째 함수 내부");
}
first();
console.log("글로벌 실행 컨텍스트 내부");
```

![](https://velog.velcdn.com/images/kato/post/77422841-5938-467b-b9cb-ef88b2ca1037/image.webp)

위의 코드가 브라우저에 로드되면 **Javascript엔진**은 전역 실행 컨텍스트를 생성하고 이를 현재 실행 스택으로 푸시합니다. 첫 번째`first()`호출되면 Javascript 엔진은 해당 함수에 대한 새 실행 컨텍스트를 생성하고 이를 현재 실행 스택의 맨 위로 푸시합니다.

`second()`함수 내에서 함수가 호출 되면 `first()`Javascript 엔진은 해당 함수에 대한 새 실행 컨텍스트를 생성하고 이를 현재 실행 스택의 맨 위로 푸시합니다. 함수가 완료 되면 `second()`실행 컨텍스트가 현재 스택에서 제거되고 컨트롤은 그 아래의 실행 컨텍스트, 즉 함수 `first()`실행 컨텍스트에 도달합니다.

완료되면 `first()`실행 스택이 스택에서 제거되고 제어가 전역 실행 컨텍스트에 도달합니다. 모든 코드가 실행되면 JavaScript 엔진은 현재 스택에서 전역 실행 컨텍스트를 제거합니다.

#### **실행 컨텍스트는 어떻게 생성됩니까?**

지금까지 JavaScript 엔진이 실행 컨텍스트를 관리하는 방법을 살펴보았다. 이제 JavaScript 엔진이 실행 컨텍스트를 생성하는 방법을 알아보자.

실행 컨텍스트는 1) 생성 단계 와 2) 실행 단계의 두 단계로 생성됩니다.

**The Creation Phase**
실행 컨텍스트는 생성 단계에서 생성됩니다. 생성 단계에서 다음과 같은 일이 발생합니다.

1. LexicalEnvironment 구성 요소가 생성됩니다.
2. VariableEnvironment 컴포넌트가 생성됩니다.

따라서 실행 컨텍스트는 개념적으로 다음과 같이 나타낼 수 있습니다.

```
ExecutionContext = {
  LexicalEnvironment = <ref. LexicalEnvironment in memory>,
  VariableEnvironment = <ref. VariableEnvironment in memory>,
}
```

**Lexical Environment**
공식 ES6 문서는 Lexical Environment를 다음과 같이 정의합니다.

> 어휘 환경은 ECMAScript 코드의 어휘 중첩 구조를 기반으로 특정 변수 및 함수에 대한 식별자 의 연결을 정의하는 데 사용되는 사양 유형입니다 . 어휘 환경은 환경 레코드와 외부 어휘 환경 에 대한 null 참조로 구성됩니다 .

간단히 말해서 어휘 환경은 식별자-변수 매핑을 보유하는 구조입니다 . (여기서 식별자는 변수/함수의 이름을 의미하며, 변수는 실제 객체[함수 객체 및 배열 객체 포함] 또는 기본 값에 대한 참조)입니다.

예를 들어 다음 스니펫을 고려하십시오.

```javascript
let a = 20;
let b = 40;
function foo() {
  console.log("bar");
}
```

따라서 위 스니펫의 어휘 환경은 다음과 같습니다.

```javascript
lexicalEnvironment = {
  a: 20,
  b: 40,
  foo: <ref. foo function>
}
```

각 어휘 환경에는 세 가지 구성 요소가 있습니다.

1. Environment Record
2. Reference to the outer environment,
3. This binding.

## **Environment Record**

환경 레코드는 어휘 환경 내에 변수 및 함수 선언이 저장되는 장소입니다.

또한 두 가지 유형의 환경 레코드가 있습니다 .

- **Declarative environment record**
  이름에서 알 수 있듯이 변수 및 함수 선언을 저장합니다. 함수 코드의 어휘 환경에는 선언적 환경 레코드가 포함되어 있습니다.
- **Object environment record**
  전역 코드에 대한 어휘 환경에는 객관적인 환경 레코드가 포함되어 있습니다. 변수 및 함수 선언과 별도로 개체 환경 레코드는 전역 바인딩 개체(브라우저의 창 개체)도 저장합니다. 따라서 바인딩 개체의 각 속성(브라우저의 경우 브라우저가 창 개체에 제공하는 속성 및 메서드 포함)에 대해 레코드에 새 항목이 생성됩니다.

- **Note**
  함수 코드 의 경우 환경 레코드에는arguments 함수에 전달된 인덱스와 인수 사이의 매핑과 함수에 전달된 인수의 길이(숫자)를 포함하는 개체 도 포함됩니다 . 예를 들어 아래 함수의 인수 객체는 다음과 같습니다.

```javascript
function foo(a, b) {
  var c = a + b;
}
foo(2, 3);
// argument object
Arguments: {0: 2, 1: 3, length: 2},
```

## Reference to the Outer Environment

외부 환경에 대한 참조는 외부 어휘 환경에 액세스할 수 있음을 의미합니다. 즉, JavaScript 엔진은 현재 어휘 환경에서 변수를 찾을 수 없는 경우 외부 환경 내부에서 변수를 찾을 수 있습니다.

## This binding.

이 구성 요소에서 의 값이 this결정되거나 설정됩니다.

전역 실행 컨텍스트에서 값은 this전역 개체를 나타냅니다. (브라우저에서는 thisWindow 객체를 가리킴).

함수 실행 컨텍스트에서 값은 this함수가 호출되는 방식에 따라 다릅니다. 객체 참조에 의해 호출되면 의 값이 this해당 객체로 설정되고, 그렇지 않으면 의 값이 전역 객체 또는 (엄격 모드에서) this로 설정됩니다 . undefined예를 들어:

```javascript
const person = {
  name: "peter",
  birthYear: 1994,
  calcAge: function () {
    console.log(2018 - this.birthYear);
  },
};
person.calcAge();
// 'calcAge'가 //'person' 개체 참조로 호출되었기 때문에 'this'는 'person'을 참조합니다.
constcalculateAge = person.calcAge;
calculateAge();
// 'this'는 개체 참조가 제공되지 않았기 때문에 전역 창 개체를 참조합니다.
```

추상적으로 어휘 환경은 의사 코드에서 다음과 같이 보입니다.

```javascript
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 식별자 바인딩은 여기로 이동
     }
    outer: <null>,
    this: <global object>
  }
}
FunctionExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 식별자 바인딩은 여기로 이동
     }
    outer: <Global or outer function environment reference>,
    this: <depends on how function is called>
  }
}
```

## Variable Environment:

또한 EnvironmentRecord가 이 실행 컨텍스트 내에서 VariableStatements 에 의해 생성된 바인딩을 보유하는 어휘 환경이기도 합니다 .

위에서 설명한 것처럼 환경 변수도 어휘 환경이므로 위에서 정의한 어휘 환경의 모든 속성과 구성 요소를 갖습니다.

ES6에서 LexicalEnvironment 구성 요소와 VariableEnvironment 구성 요소의 한 가지 차이점은 전자는 함수 선언 및 변수( let 및 const) 바인딩을 저장하는 데 사용되고 후자는 변수 (var)바인딩만 저장하는 데 사용된다는 것입니다.

#### Execution Phase

이 단계에서 모든 변수에 대한 할당이 완료되고 코드가 최종적으로 실행됩니다.

#### Example

위의 개념을 이해하기 위해 몇 가지 예를 살펴보겠습니다.

```javascript
let a = 20;
const b = 30;
var c;
function multiply(e, f) {
  var g = 20;
  return e * f * g;
}
c = multiply(20, 30);
```

위의 코드가 실행되면 JavaScript 엔진은 전역 코드를 실행하기 위해 전역 실행 컨텍스트를 생성합니다. 따라서 전역 실행 컨텍스트는 생성 단계에서 다음과 같이 표시됩니다.

```javascript
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 식별자 바인딩은 여기
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 식별자 바인딩은 여기
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

실행 단계에서 변수 할당이 완료됩니다. 따라서 전역 실행 컨텍스트는 실행 단계에서 이와 같이 보일 것입니다.

```javascript
GlobalExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 식별자 바인딩은 여기
      a: 20,
      b: 30,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 식별자 바인딩은 여기
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

함수 호출이 multiply(20, 30)발생하면 함수 코드를 실행하기 위해 새 함수 실행 컨텍스트가 생성됩니다. 따라서 함수 실행 컨텍스트는 생성 단계에서 다음과 같이 표시됩니다.

```javascript
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 식별자 바인딩은 여기에 갑니다.
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined> ,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 식별자 바인딩은 여기
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

그런 다음 실행 컨텍스트는 함수 내부의 변수에 대한 할당이 완료되었음을 의미하는 실행 단계를 거칩니다. 따라서 함수 실행 컨텍스트는 실행 단계에서 다음과 같이 표시됩니다.

```javascript
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 식별자 바인딩은 여기에 갑니다.
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined> ,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 식별자 바인딩은 여기
      g: 20
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

함수가 완료된 후 반환된 값은 내부에 저장됩니다. 따라서 전역 어휘 환경이 업데이트됩니다. 그런 다음 전역 코드가 완료되고 프로그램이 종료됩니다.
