# 46.1 제너레이터란?

- 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
- 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
- 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

# 46.2 제너레이터 함수의 정의

- 제너레이터 함수는 function* 키워드로 선언한다.
- 제너레이터 함수는 화살표 함수로 정의할 수 없다.

# 46.3 제너레이터 객체

- 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.

# 46.4 제너레이터의 일시 중지와 재개

- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 호출자로 양도된다.

```tsx
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(generator.next(); // {value: 1, done: false} -> yield 1;까지 실행
console.log(generator.next(); // {value: 2, done: false} -> yield 2;까지 실행
console.log(generator.next(); // {value: 3, done: false} -> yield 3;까지 실행
console.log(generator.next(); // {value: undefined, done: true} -> 함수 끝까지 실행
```

- 또한, next 메서드를 통해 인수를 전달할 수 있고, yield 표현식을 할당받는 변수에 할당된다.

```tsx
function* genFunc() {
  const x = yield 1;
  const y = yield(x + 10);
  
  return x + y;
}

const generator = genFunc(0);

let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10); // 변수 x에 10 할당
console.log(res); // {value: 20, done: false}

res = generator.next(20); // 변수 y에 20 할당
console.log(res); // {value: 30, done: true}
```

# 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

- 무한 피보나치에 대한 이터러블 생성 함수를 제너레이터를 활용하여 구현

```tsx
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre+cur];
      return { value: cur };
    }
  }
}());

// 무한 이터러블을 생성하는 제너레이터 함수
const infinitiFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while(true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());
```

### 46.5.2 비동기 처리

```tsx
const async = generatorFunc => {
  const generator = generatorFunc();

  const onResolved = arg => {
    const result = generator.next(arg);

    return result.done ? result.value : result.value.then(res => onResolved(res));
};

(async(function* fetchTodo() {
  const url = 'https://examlple.com'
  
  const response = yield fetch(url);
  const todo = yield response.json();
  console.log(todo);
})());
```

# 46.6 async/await

- 위의 비동기 처리를 제너레이터로 구현했지만 코드가 무척 장황하고 가독성이 안좋다.
- ES8에서는 더 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입되었다.

### 46.6.1 async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수는 반환 값을 resolve하는 프로미스를 반환한다.

### 46.6.2 await 키워드

- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야한다.

```tsx
const getUserName = async id => {
  const res = await fetch(`https://example.com/users/${id}`);
  const { name } = await res.json();
  console.log(name);
}
```

### 46.6.3 에러 처리

- 콜백을 사용하는 비동기 처리 패턴의 가장 큰 단점 중 하나는 에러 처리이다. 콜백함수에서 throw되는 에러는 try…catch 문으로 에러를 캐치할 수 없기 때문이다.
- 그러나 async/await의 경우 try…catch 문을 사용할수 있다.

```tsx
const getUserName = async id => {
  try {
    const res = await fetch(`https://example.com/users/${id}`);
    const { name } = await res.json();
    console.log(name);
  } catch (err) {
    console.error(err);
  }
}
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.
