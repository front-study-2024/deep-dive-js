# 33.1 심벌이란?

- 자바스크립트에는 6개의 타입(문자열, 숫자, 불리언, undefined, null, 객체)이 있었다.
- 심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.
- 심벌은 다른 값과 중복되지 않는 유일무이한 값이다.

# 33.2 심벌 값의 생성

### 33.2.1 Symbol 함수

- 심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 타입과 달리 리터럴 표기법이 없다.

```jsx
const mySymbol = Symbol();
```

- 다른 생성자 함수와 달리 new 연산자를 사용하지 않는다. new 연산자와 함께 호출하면 객체(인스턴스)가 생성되지만 심벌 값은 변경 불가능한 원시값이다.
- 심벌의 인자로 문자열을 넣을 수 있는데, 이 문자열은 생성된 심벌  값에 대한 설명이다.

```jsx
const mySymbol = Symbol('mySymbol');
```

### 33.2.2 Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 해당 키와 일치하는 심벌 값을 검색한다.

```jsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbor.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const s2 = Symbor.for('mySymbol');

console.log(s1 === s2); // true
```

- Symbol.keyFor 메서드는 반대로 심벌 값의 키를 추출한다.

```jsx
const s1 = Symbor.for('mySymbol');

Symbol.keyFor(s1) // -> mySymbol
Symbol.keyFor(s2) // -> undefined
```

# 33.3 심벌과 상수

- 4방향에 대한 상수를 정의한다고 생각해 보자.

```jsx
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
}
```

- 위 예제와 같이 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다. 이때 문제는 상수값 1, 2, 3, 4가 변경될 수 있고, 다른 변수 값과 중복될 수도 있다는 문제가 있다.
- 이러한 경우 무의미한 상수 대신 심벌 값을 사용할 수 있다.

```jsx
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
}
```

# 33.4 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 심벌 값으로 만들 수 있다.

```jsx
const obj = {
  [Symbol.for('mySymbol']: 1
}

obj[Symbor.for('mySymbol')] // -> 1
```

- 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

# 33.5 심벌과 프로퍼티 은닉

- 프로퍼티 키로 심벌 값을 사용했다면, for … in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 해당 프로퍼티를 찾을 수 없다.
- ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 해당 프로퍼티를 찾을 수 있다.

# 33.6 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체를 확장하여 사용하는 것은 권장하지 않는다.
- 예를 들어 ES6 이전에 Array.prototype에 find 라는 메서드를 직접 만들어서 사용했다면, ES6 이후 도입된 Array.prototype.find와 이름이 겹쳐서 메서드를 덮어 쓰게된다.
- 하지만 심벌을 사용한다면 중복될 가능성이 없으므로 위와 같은 문제가 발생하지 않아 안전하게 표준 빌트인 객체를 확장할 수 있다.

# 33.7 Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol 이라 부른다.
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.
