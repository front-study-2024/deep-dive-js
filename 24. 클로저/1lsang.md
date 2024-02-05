# 클로저
- 함수와 그 함수가 선언된 렉시컬 환경의 조합
## 렉시컬 스코프
- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정하는데, 이를 렉시컬 스코프(정적 스코프)라 한다. 
- 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못하며, 함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않는다.
- **렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값(상위 스코프에 대한 참조)은 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.**
## 함수 객체의 내부 슬롯 `[[Environment]]`
- 함수가 정의된 환경(위치)과 호출되는 환경(위치)은 다를 수 있다. 따라서 호출 환경과 관계 없이 정의된 환경(상위 스코프)을 기억해야 한다. 이를 위해 함수는 내부 슬롯 `[[Environment]]`에 자신이 정의된 환경(상위 스코프의 참조)를 저장한다.
- 함수 객체의 내부 슬롯 `[[Environment]]`에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 상위 스코프이다. 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에는 이 값이 저장된다. 즉, 함수 객체는 내부 슬롯 `[[Environment]]`에 저장한 렉시컬 환경의 참조(상위 스코프)를 자신이 존재하는 한 기억한다. 
- 함수가 호출되면 아래 순서로 코드 평가가 진행되는데, 이 때 함수 렉시컬 환경의 구성요소인 "외부 렉시컬 환경에 대한 참조"에는 함수 객체의 내부 슬롯 `[[Environment]]`에 저장된 렉시컬 환경의 참조가 할당된다. 
	1. 함수 실행 컨텍스트 생성
	2. 함수 렉시컬 환경 생성
		1. 함수 환경 레코드 생성
		2. this 바인딩
		3. 외부 렉시컬 환경에 대한 참조 결정
## 클로저와 렉시컬 환경

```javascript
const x = 1;

function outer(){
  const x = 10;
  const inner = function () { console.log(x); };
  return inner;
}

const innerFunc = outer();
innerFunc();  // 10
```

- `outer` 함수의 생명주기가 마감되었지만, `innerFunc`의 실행 결과는 `outer` 함수의 지역 변수의 값을 출력한다.
- **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조할 수 있고, 이러한 중첩 함수를 클로저라고 부른다.**
- `outer` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만, `outer` 함수의 렉시컬 환경은 `inner` 함수의 내부 슬롯 `[[Environment]]`에 의해 참조되기 때문에 소멸하지 않는다. 가비지 컬렉터는 누군가 참조하고 있는 메모리 공간을 해제하지 않기 때문이다. 
- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저이지만, 일반적으로 모든 함수를 클로저라고 하지 않는다. 
- 상위 스코프의 어떤 식별자도 참조하지 않는 경우, 참조하지 않는 식별자를 기억하는 것은 메모리 낭비이기 때문에 모던 브라우저에서는 최적화를 통해 상위 스코프를 기억하지 않는다. 이런 함수는 클로저라고 할 수 없다.
```javascript
function foo() {
	const x = 1;
	function bar() {
		const y = 2;
		console.log(y);
	}
	return bar;
}
foo();
```

- 중첩 함수가 상위 스코프의 식별자를 참조하지만 외부 함수보다 중첩 함수의 생명 주기가 짧은 경우, 외부 함수보다 일찍 소멸되기 때문에 생명주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다. 따라서 일반적으로 이런 함수를 클로저라고 부르지 않는다.
```javascript
function foo() {
	const x = 1;
	function bar() {
		console.log(x);
	}
	bar();
}
foo();
```

- 다음과 같이 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다. 
```javascript
function foo() {
	const x = 1;
	function bar() {
		console.log(x);
	}
	return bar;
}
const bar = foo();
bar();
```

- 클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수**라고 부른다. 클로저라는 의미는 함수가 자유 변수에 대해 닫혀있다는 의미이며, 자유 변수에 묶여있는 함수라고 할 수 있다. 
- 모던 자바스크립트 엔진은 참조하고 있지 않은 식별자는 기억하지 않도록 최적화하기 때문에 불필요한 메모리 점유는 발생하지 않는다.

## 클로저의 활용
- **클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.** 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.
```javascript
const Counter = (function() {
  let num = 0;
  function Counter() {};
  Counter.prototype.increase = function(){ return ++num };
  Counter.prototype.decrease = function(){ return num > 0 ? --num : 0 };
  return Counter;
})();

const counter = new Counter();
```
- 변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적인 원인이 될 수 있다. 외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.
- 함수형 프로그래밍에서는 아래와 같이 클로저를 사용할 수 있다. **이 때 주의해야할 것은 `increaser`, `decreaser`와 같이 반환된 함수는 자신만의 독립된 렉시컬 환경을 가진다는 점이다.** 내부 슬롯 `[[Environment]]`로 상위 스코프인 `makeCounter`의 렉시컬 환경을 참조하므로 클로저이다. 하지만 makeCounter 함수 실행시마다 실행 컨텍스트가 새로 생기기 때문에 `increaser`와 `decreaser`는 각각 다른 렉시컬 환경을 가진다. 
```javascript
function makeCounter(aux){
  let counter = 0;
  return function() {
	  counter = aux(counter);
	  return counter;
  };
}

function increase(n) { return ++n; }
function decrease(n) { return --n; }

const increaser = makeCounter(increase);
const decreaser = makeCounter(decrease);
```
- 따라서 독립된 카운터가 아닌 연동하여 증감이 가능한 카운터를 만드려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다. 이를 위해 `makeCounter` 함수를 두 번 호출하지 말아야 한다.
```javascript
const counter = (function (){
  let counter = 0;
  return function() {
	  counter = aux(counter);
	  return counter;
  };
}());

function increase(n) { return ++n; }
function decrease(n) { return --n; }

counter(increase);
counter(decrease);
```

## 캡슐화와 정보 은닉
- 캡슐화: 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 있는 동작인 메서드를 하나로 묶는 것
- 정보 은닉: 캡슐화를 사용하여 객체의 특정 프로퍼티나 메서드를 감추는 것
- 정보 은닉을 통해 외부에 공개할 필요 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체간의 상호 의존성(결합도)을 낮춘다. 
- 자바스크립트에서는 정보 은닉을 완벽하게 지원하지 않는다.

## 자주 발생하는 실수
- for문으로 클로저를 만들 때, `var` 키워드를 사용할 경우 전역 변수에 추가되고 이에 따라 전역 변수를 참조하는 클로저가 만들어져 원하는 대로 동작하지 않을 수 있다. 