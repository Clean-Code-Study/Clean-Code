# 9장 단위 테스트

1997년만 해도 TDD(Test Driven Development)라는 개념을 아무도 몰랐다. 대다수에게 단위 테스트란 자기 프로그램이 ‘돌아간다’는 사실만 확인하는 일회성 코드에 불과했다.

현재는 애자일과 TDD 덕분에 단위 테스트를 자동화하는 프로그래머들이 이미 많아졌다. 하지만, 우리 분야에 테스트를 추가하려고 급하게 서두르는 와중에 많은 프로그래머들이 제대로 된 테스트 케이스를 작성해야 한다는 중요한 사실을 놓쳐버린다.

### TDD 법칙 세 가지

- 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
- 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

위 세 가지 규칙을 따르면 개발과 테스트가 거의 동시에 나온다. 이렇게 하면 아주 많은 테스트 케이스가 나온다.

하지만, 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 문제를 유발한다.

### 깨끗한 테스트 코드 유지하기

지저분한 테스트 코드는 테스트를 안하는 것과 오십보 백보, 아니 오히려 더 못하다. 테스트 코드가 복잡할수록 실제 코드를 짜는 시간보다 테스트 케이스를 추가하는 시간이 더 걸리기 쉽다. 즉, 테스트 코드는 실제 코드 못지 않게 중요하고 깨끗하게 짜야 한다.

코드에 유연성, 유지보수성, 재사용성을 제공하는 버팀목이 바로 단위 테스트이다. 테스트 케이스가 있으면 변경이 두렵지 않으니까! 테스트 케이스가 없다면 모든 변경이 잠정적인 버그나 마찬가지다.

그런데 만약 테스트 코드가 지저분하다면? 코드를 변경하는 능력이 떨어지고 코드 구조를 개선하는 능력도 떨어질 것이다.

### 깨끗한 테스트 코드

‘가독성’. 깨끗한 테스트 코드를 위해 필요한 것은 가독성이다. 가독성을 높이려면 최소의 표현으로 많은 것을 나타내야 한다. 잡다하고 세세한 코드는 필요 없다. 바로 본론으로 들어가 자료 유형과 함수만 사용하도록 한다.

**DSL(도메인 특화 테스트 언어)**

- DSL(Domain Specific Language)이란?

    <aside>
    💡 특정 영역을 타켓하고 있는 언어를 말한다. 예를 들어, SQL은 ‘DB에 데이터를 참조하기 위한 목적’으로만 사용된다. JAVA처럼 서버를 만들거나 그 외 원하는 모든 것을 만들어 내는 언어와는 다르다. 
    즉, **특정 기능을 위한 구현&구성한 언어**이다.

    </aside>


DSL로 테스트 코드를 구현하면 테스트를 구현하는 당사자와 나중에 테스트를 읽어볼 독자를 도와준다.????

**이중 표준**

테스트 코드에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다. 실제 환경이 아니라 테스트 환경에서 돌아가는 코드이기 때문에, 실제 코드만큼 효율적일 필요는 없다.

즉, 실제 환경에서는 절대로 안 되지만 테스트 환경에서는 전혀 문제없는 방식이 있다. 메모리가 CPU 효율과 관련 있는 경우가 대부분이다. 코드의 깨끗함과는 무관하다.

### 테스트 당 assert 하나

- assert문이란?

    <aside>
    💡 코드 개발 간에 하나의 ‘검토’를 담당한다. assert문은 일정 조건이 사용자의 의도에 부합하는지 검사하는 역할을 하는 구문이라 생각하면 된다.
    예) int형이 매개변수인 함수에 인자로 string이 들어오는지 검사

    </aside>


테스트 코드를 짤 때는 함수마다 assert문을 단 하나만 사용해야 한다고 주장하는 학파가 있다. assert문이 단 하나인 함수는 결론이 하나라서 코드를 이해하기 쉽고 빠르다.

‘단일 assert문’이라는 규칙은 책의 저자는 훌륭한 지침이라고 생각한다고 한다. 하지만 상황에 따라 함수 하나에 여러 assert문을 넣기도 한다.

무조건 하나!가 아닌 assert 문 개수는 최대한 줄여야 좋다는 뜻이다.

**테스트 당 개념 하나**

테스트 함수마다 한 개념만 테스트하도록 한다. 한 함수에 여러 개념을 연속으로 테스트하지 말아야한다.

결론적으로 종합해보면 두 규칙이다.

- 개념 당 assert문 수를 최소로 줄여라
- 테스트 함수 하나는 개념 하나만 테스트하라

### F.I.R.S.T

- Fast (빠르게) : 테스트는 빨리 돌아야한다.
  - 테스트가 느리다면? → 자주 돌리지 못함 → 문제를 찾아내 고치지 못함
- Independent (독립적으로) : 각 테스트는 서로 의존하면 안 된다. 한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안 된다.
  - 만약 테스트가 서로 의존할 때, 하나가 실패한다면? → 나머지도 잇달아 실패 → 원인 진단 어려움
- Repeatable (반복 가능) : 테스트는 어떤 환경에서도 반복 가능해야 한다. 실제 환경, 네트워크 연결 안 된 노트북 환경 등 모든 환경에서.
  - 테스트가 돌아가지 않는 환경이 하나라도 있다면? → 테스트가 실패한 이유를 둘러댈 변명이 생김 / 환경이 지원되지 않아 테스트를 수행하지 못하는 상황 발생
- Self-Validating (자가 검증) : 테스트는 bool값으로 결과를 내야 한다. 성공 아니면 실패다.
- Timely (적시에) : 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다.
