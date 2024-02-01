## Symbol

<details>
  <summary><h3>Symbol 자료형에 대해서 설명해주세요.</h3></summary>
  <p></p>
</details>
<details>
  <summary><h3>Symbol로 프로퍼티의 키값으로 설정하면 어떠한 이점이 있을까요?</h3></summary>
  <p></p>
</details>
<details>
  <summary><h3>Symbol을 실제로 사용할 수 있는 예시로 무엇이 있나요?</h3></summary>
  <li>객체의 프로퍼티 키를 Symbol로 설정한다면 프로퍼티의 유일성을 보장할 수 있고, 반복문이나 Object.keys()와 같은 메서드에 노출되지 않아 private한 프로퍼티를 지정할 수 있습니다.</li>
  <li>또한, Symbol을 사용해서 상수를 정의하기에도 좋습니다. 마찬가지로 유일성을 보장할 수 있고, 상수의 의미 전달도 명확해집니다.</li>
</details>
<details>
  <summary><h3>Symbol의 인자로 들어가는 값의 의미는 무엇인가요?</h3></summary>
  <li>Symbol의 인자로 문자열을 넣을 수 있는데, 이 문자열은 생성된 Symbol에 대한 description을 의미합니다.</li>
  <li>Symbol에 대한 description은 Symbol의 key와 무관하며, 여러 Symbol 마다 중복된 description을 가질 수 있습니다.</li>
</details>
