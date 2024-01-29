# this
## `this` 키워드
- `this` 키워드는 **자신이 속한 객체를 가리키는 식별자**
- 객체에서 메서드는 자신이 속한 객체의 프로퍼티를 참조하고 변경할 수 있어야 하고, 이를 위해 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
- 생성자 함수를 정의하는 시점은 인스턴스를 생성하기 이전이므로, 인스턴스를 가리키는 식별자를 알 수 없다. 하지만 프로퍼티 또는 메서드를 추가하기 위해서는 자신이 생성할 인스턴스를 가리키는 식별자가 필요하다.
- 메서드에서 자신이 속한 객체를 가리키는 식별자나 생성자 함수에서 자신이 생성할 인스턴스를 가리키는 식별자가 필요하며, 이를 위해 `this`라는 식별자를 사용한다.
- `this`는 자기 참조 변수로 **자신이 속한 객체** 혹은 **자신이 생성할 인스턴스**를 가리킨다. 
- 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 `arguments` 객체와 `this`가 함수 내부에 전달된다. **단, `this` 바인딩(`this`가 가리키는 값)은 함수 호출 방식에 의해 동적으로 결정된다.** 
- 클래스 기반 언어에서 `this`는 항상 클래스가 생성할 인스턴스를 가리키지만, 자바스크립트는 함수가 호출되는 방식에 따라 `this`에 바인딩될 값이 동적으로 결정된다. 또한, strict mode 역시 `this` 바인딩에 영향을 준다. 

> [!Note]
> 바인딩?
> 
> 바인딩이란 식별자와 값을 연결하는 과정을 말한다. 


### Case: 객체 리터럴
객체 리터럴 내부에서의 `this`는 메서드를 호출한 객체를 가리킨다.
```javascript
const me = {
	name: 'ilsang',
	getName() {
		return this.name;
	}
}

console.log(me.getName()); // 'ilsang'
```

### Case: 생성자 함수
생성자 함수 내부의 `this`는 생성자 함수가 생설할 인스턴스를 가리킨다. 
```javascript
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
}

const me = new Person('ilsang');
console.log(me.getName()); // 'ilsang'
```

### Case: 일반 함수
일반 함수 내부에서 `this`는 전역 객체(브라우저에서는 `window`)를 가리킨다. 단, strict mode 사용 시, 일반 함수에서 `this`는 사용할 필요가 없기 때문에 `undefined` 값을 가진다. 
```javascript
function Person() {
	console.log(this);
	return 'Hello';
}

Person(); // window
```

### Case: 전역
전역에서 `this`는 전역 객체(브라우저에서는 `window`)를 가리킨다.
```javascript
console.log(this);
```

## 함수 호출 방식과 `this` 바인딩
`this`에 바인딩될 값은 함수 호출 방식에 따라 동적으로 결정되며 함수 호출 시점에 결정된다. 

### Case: 일반 함수 호출

- 기본적으로 전역 객체가 바인딩되며, strict mode 사용 시 `undefined`가 바인딩된다. 
- 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 전역 객체가 바인딩 된다. 
- 콜백 함수도 일반 함수로 호출된다면 전역 객체가 바인딩된다. 
즉, **일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 `this`에는 전역객체가 바인딩 된다.**

```javascript
function foo() {
	console.log(this);
}

foo(); // window

const obj1 = {
	value: 1,
	foo() {
		function bar() {
			console.log(this);
		}
		bar();
	}
};

obj1.foo(); // window

const obj2 = {
	value: 1,
	foo() {
		setTimeout(function () {
			console.log(this); // window
		}, 100);
	}
};

obj2.foo();
```

> [!Note] 
> `this` 바인딩 변경
> 
> 중첩 함수 나 콜백 함수는 일반적으로 헬퍼 함수 역할을 한다. 하지만 외부 함수와 `this`가 일치하지 않는다면 헬퍼 함수로서 동작하기 어렵다. 따라서 변수에 this를 할당해 사용하거나 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드를 사용하여 `this`를 명시적으로 바인딩할 수 있다. 또한, 화살표 함수의 `this`는 상위 스코프의 `this`를 가리키므로 이를 사용하여 `this` 바인딩을 일치시킬 수 있다.

### Case: 메서드 호출
- 메서드 내부의 `this`에는 메서드를 호출한 객체(메서드 이름 앞의 `.`연산자 앞에 기술한 객체)가 바인딩된다. 
- **메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다는 점을 주의해야한다.** 
- 즉, 프로토타입 메서드 내부에 사용된 `this`도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

### Case: 생성자 함수 호출
- 생성자 함수 내부의 `this`에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

### Case: 명시적 바인딩
- `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드를 사용하면 해당 메서드들의 첫번째 인자로 전달된 객체를 `this`로 사용할 수 있다.
- `apply`와 `call`의 경우 함수를 실행하고, `bind`의 경우 바인딩된 함수를 반환한다.

