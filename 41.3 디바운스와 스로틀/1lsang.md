# 디바운스와 스로틀
`scroll`, `resize`, `input`, `mousemove`와 같은 이벤트는 짧은 시간 간격으로 연속해서 발생하고, 이런 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능 문제가 발생할 수 있음. 디바운스와 스로틀은 짧은 시간 간격으로 발생하는 이벤트를 그룹화하며 과도한 이벤트 호출을 방지하는 기법.

## 디바운스(Debounce)
짧은 시간 간격으로 이벤트 연속 발생시, 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 하는 기법
짧은 시간 간격으로 발생하는 이벤트를 그룹화하여 마지막에 한 번만 이벤트 핸들러가 호출된다.

**예시**
```javascript
const debounce = (callback, delay) => {
  let timerId;
  return (...args) => {
    if (timerId) clearTimeout(timerId);
    timerId = setTimeout(callback, delay, ...args);
  };
};
```

- 일정 시간동안 이벤트가 발생하지 않을 경우, 이벤트가 완료된 것으로 간주한다.
- `debounce`가 반환한 함수는 `delay`보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정하고, 이에 따라 `callback` 함수는 `delay`보다 짧은 간격으로 이벤트가 발생하는 동안 호출되지 않다가 더 이상 이벤트가 발생하지 않으면 호출된다.
- 사용 예시: `resize` 이벤트 처리, `input` 요소의 네트워크 요청 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등
- Underscore의 `debounce`, Lodash의 `debounce` 함수 사용 권장

## 쓰로틀(throttle)
짧은 시간 간격으로 이벤트 연속 발생시, 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 하는 기법
짧은 시간 간격으로 발생하는 이벤트를 그룹화하여 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

**예시**
```javascript
const throttle = (callback, delay) => {
  let timerId;
  return (...args) => {
    if (timerId) return;
    timerId = setTimeout(() => {
	  callback(...args);
	  timerId = null;
    }, delay);
  };
};
```

- 짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러 호출을 방지하기 위해, 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- `throttle`이 반환한 함수는 `delay` 시간이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 `delay` 시간이 경과하면 `callback` 함수를 호출하고 새로운 타이머를 설정하는 것으로 `delay` 간격으로 콜백 함수가 호출된다.
- 사용 예시: `scroll` 이벤트 처리, 무한 스크롤 UI 구현 등
- Underscore의 `throttle`, Lodash의 `throttle` 함수 사용 권장