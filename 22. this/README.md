## this

<details>
  <summary><h3>자바스크립트의 this에 대해서 설명해주세요.</h3></summary>
  <p></p>
</details>
<details>
  <summary><h3>일반 함수의 경우 this가 필요없는 이유를 설명해주세요.</h3></summary>
  <li>함수가 객체의 메서드로 호출되지 않고, 독립적으로 실행되기 때문입니다. </li> 
  <li>함수가 객체의 메서드로 호출되면 this는 해당 객체를 참조하게 되어 객체의 속성이나 메서드에 접근할 수 있습니다.</li>
  <p></p>
</details>
<details>
  <summary><h3>콜백 함수 내부에서 this를 사용할 경우 어떤 문제가 발생할까요? 어떻게 해결할 수 있을까요?</h3></summary>
  <li>콜백함수에서 this는 의도하지 않은 객체를 가리킬 수 있습니다.</li>
  <li>이를 해결하기 위해 화살표 함수를 사용하거나 bind 함수를 통해서 this를 지정해 줄 수 있습니다.</li>
  <p></p>
</details>
<details>
  <summary><h3>bind, apply, call의 차이는 무엇인가요?</h3></summary>
  <li>bind, apply, call은 모두 자바스크립트에서 함수에 this를 바인딩하는 메서드입니다.</li>
  <li>apply와 call은 this 지정과 동시에 함수를 호출한다는 공통점이 있지만, 함수에 전달하는 매개변수의 전달 방식에 차이가 있습니다. call은 인자를 나열해서 함수의 매개변수로 전달하고, apply는 배열 형태로 매개변수를 묶어 전달합니다.</li>
  <li>bind의 경우 apply, call과 달리 함수를 즉시 호출하지 않고 해당 함수의 복사본을 반환합니다. 반환된 함수의 복사본은 나중에 호출할 수 있습니다.</li>
</details>
