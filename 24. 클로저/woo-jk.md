# 24.1 렉시컬 스코프

- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.

# 24.2 함수 객체의 내부 슬롯 [[Environment]]

- 함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.
- 상위 함수(혹은 전역 코드)가 평가되는 시점에 내부 함수 객체를 생성한다. 이때, 생성된 함수 객체의 내부 슬롯 [[Environment]]에는 평가중인 외부 함수 코드의 렉시컬 환경을 저장한다.
- 함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

# 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

function outer() {
  const x = 10;
  const inner = function() { console.log(x); };
  return inner;
}

const innerFn = outer();
innerFn(); // 10
```

- outer 함수는 중첩 함수 inner를 반환하고 생명 주기를 마감한다. 즉, outer 함수의 실행 컨텍스트는 스택에서 제거된다.
- 그러나 inner의 내부 슬롯 [[Environment]]에 참조한 outer의 렉시컬 환경은 제거되지 않고 남아있다.
- outer의 실행 컨텍스트는 소멸되어도 렉시컬 환경은 아직 살아있다.
- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.
- 이러한 중첩 함수를 클로저라고 부른다.

### 클로저의 정의

- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다.
- 그러나 일반적으로는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하여 클로저라 부른다.

### 브라우저의 클로저

- 브라우저는 중첩 함수에서 참조하고있는 식별자만 메모리에 남기고 참조하지 않는 다른 식별자들은 모두 소멸시켜서 최적화한다.

# 24.4 클로저의 활용

- 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.
- 상태를 안전하게 은닉하고 특정함수에게만 상태 변경을 허용하기 위해 사용한다.

```jsx
const counter = (function() {
  let num = 0;

  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  }
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

- 변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있다.
- 외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 활용된다.

# 24.5 캡슐화와 정보 은닉

- 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.
- 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 한다.
- 정보 은닉은 프로퍼티를 외부에 공개되지 않도록 감추어 적절치 못하게 상태가 변경되는 것을 방지하여 정보를 보호하고, 객체 간의 결합도를 낮추는 효과가 있다.
- 일반적인 객체지향 언어는 클래스를 정의하고 클래스를 구성하는 멤버에 대하여 public, private, protected 같은 접근 제한자로 공개 범위를 한정할 수 있다.
- 그러나 자바스크립트는 이러한 접근 제한자를 제공하지 않기 때문에 모든 프로퍼티와 메서드는 기본적으로 public하다.
- 자바스크립트에서는 클로저를 사용한다면 값을 은닉할 수 있다.

```jsx
function Person(name, age) {
  this.name = name;
  let _age = age;

  this.sayHi = function() {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  }
}

const me = new Person("Woo", 27);
me.sayHi(); // Hi! My name is Woo. I am 27.
console.log(me.name); // Woo
console.log(me._age); // undefined
```

- 위 예제의 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성된다.
- 아래와 같이 sayHi 메서드를 프로토타입 메서드로 변경할 수 있다.

```jsx
const Person = (function() {
  let _age = 0; // private
  
  function Person(name, age) {
    this.name = name; // public
    let _age = age;
  }

  Person.prototype.sayHi = function() {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  }

  return Person;
}());

const me = new Person("Woo", 27);
me.sayHi(); // Hi! My name is Woo. I am 27.
console.log(me.name); // Woo
console.log(me._age); // undefined
```

- 위와 같이 public한 값은 생성자 함수의 프로퍼티로, private한 값은 클로저 변수로 선언하여 사용할 수 있다.

# 24.6 자주 발생하는 실수

```jsx
var funcs = [];

for(var i = 0; i < 3; i++) {
  funcs[i] = function() { return i; };
}

for(var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

- 위 코드에선 0 1 2가 출력되는 것이 아닌, 3 3 3이 출력된다.
- var 키워드는 블록 레벨이 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수이다. 전역 변수 i에는 0, 1, 2, 3이 순차적으로 할당되기 때문에 함수를 호출하면 전역 변수 i를 참조하여 3만 출력된다.
- 이를 방지하기 위해서 i의 값을 매개변수로 중간에 저장하는 방식으로 해결할 수 있다.

```jsx
var funcs = [];

for(var i = 0; i < 3; i++) {
  funcs[i] = (function(id) {
    return functions() {
      return id;
    };
  }(i));
}

for(var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 0 1 2
}
```

- 혹은 가장 간단한 해결법은 let, const 키워드를 사용하는 것이다.

```jsx
const funcs = [];

for(let i = 0; i < 3; i++) {
  funcs[i] = function() { return i; };
}

for(let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]());
}
```

- let, const 키워드는 블록 레벨 스코프를 갖기 때문에 for문 코드 블록이 실행 될때마다 새로운 렉시컬 환경이 생성된다.
