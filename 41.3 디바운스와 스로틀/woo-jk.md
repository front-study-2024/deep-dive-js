- 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

### 41.3.1 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.

```tsx
const debounce = (callback, delay) => {
  let timerId;

  return (...args) => {
    // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
    // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
    if(timerId) clearTimeout(timerId);
    timerId = setTimeout(callack, delay, ...args);
  }
}
```

- 디바운스는 input 이벤트에 자주 사용한다.
- input 입력시 서버 통신과 같은 무거운 처리를 해줘야 하는 경우, 사용자가 입력할 때마다 해당 통신 요청을 하게되면 성능상 좋지않다.
- 디바운스를 적용하여 사용자가 입력을 모두 끝냈을 때 해당 요청을 하는 것이 바람직하다.

### 41.3.2 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다.

```tsx
const throttle = (callback, delay) => {
  let timerId;

  return (...args) => {
    // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
    // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정한다.
    // 따라서 delay 간격으로 callback이 호출된다.
    if(timerId) return;
    timerId = setTimeout(() => {
      callback(...args);
      timerId = null;
    }, delay);
  };
};
```

- scroll 이벤트는 짧은 시간 간격으로 연속해서 발생한다.
- 이처럼 짧은 간격으로 발생하는 과도한 이벤트 핸들러 호출을 방지하기 위해 스로틀을 사용한다.
