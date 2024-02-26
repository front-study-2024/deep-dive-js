## 실행 컨텍스트

<details>
  <summary><h3>실행컨텍스트란 무엇이고 어떻게 구성되어있는지 설명해주세요.</h3></summary>
</details>

<details>
  <summary><h3>호이스팅과 실행컨텍스트에는 무슨 연관이 있나요? </h3></summary>
</details>

<details>
  <summary><h3>스코프 체인의 탐색 과정에 대해서 설명해주세요.</h3></summary>
</details>

<details>
  <summary><h3>전역 렉시컬 환경의 상위 스코프는 무엇인가요?</h3></summary>
</details>

<details>
  <summary>
    <h3>이 코드의 실행과정을 실행 컨텍스트 입장에서 설명해주세요.</h3>
    
  ```js
  const x = 1;
  
  funtion foo(){
      const y = 2;
  
      function bar(){
          const z = 3;
          console.log(x+y+z);
      }
      bar();
  }
  
  foo();
  ```

  </summary>
  
  <ol>
    <li>전역 실행 컨텍스트가 생성된다.</li>
    <li>x가 선언되고 1로 초기화된다, 또한 함수 foo 도 선언된다</li>
    <li>foo 함수가 실행되면 foo 함수의 실행 컨텍스트가 콜스택에 푸쉬된다.</li>
    <li>y 변수가 선언과 동시에 2가 할당되고 bar 함수가 선언된다.</li>
    <li>bar 함수가 실행되면 bar 함수의 실행 컨텍스트가 콜스택에 푸쉬된다.</li>
    <li>bar 함수 컨텍스트에 z값이 선언 후 3이 할당된다.</li>
    <li>콜스택에 console.log 함수가 푸쉬되고 실행 종료 후 pop 된다.</li>
  </ol>
  
</details>
<details>
  <summary>
    <h3>렉시컬 환경이 무엇인지 설명하고 언제 이 환경이 어떻게 설정되는지 설명해주세요.</h3>
  </summary>
  <p>식별자(변수 이름, 함수 이름 등)와 그들의 바인딩된 값 또는 참조가 저장되는 추상적인 환경이다. 이 환경은 함수가 정의된 시점에 결정된다.
  </p>
</details>
