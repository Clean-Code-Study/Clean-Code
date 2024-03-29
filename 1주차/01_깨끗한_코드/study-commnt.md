# 1장 깨끗한 코드

## 나쁜 코드

- 나쁜 코드에 발목이 잡혀 고생한 기억이 있는가?
    - 레거시 코드
    - 지금도 고통받는중
- 왜 나쁜 코드를 짰을까?
    - 급해서, 서두르느라, 시간이 없어서, 지겨워서, 밀린 업무가 많아 후딱 해치우려고..
    - 자신이 짠 쓰레기 코드를 쳐다보며 나중에 손보겠다고 생각한 경험이 있다.
    - 하지만 나중은 결코 오지 않는다.

## 나쁜 코드로 치르는 대가

- 나쁜 코드는 개발 속도와 팀 생산성을 크게 떨어뜨린다
- 코드를 고칠 때마다 엉뚱한 곳에서 문제가 생긴다
- 얽히고설킨 코드를 ‘해독’해서 얽히고설킨 코드를 더한다
- 나쁜 코드가 많은 팀은 생산성을 증가시키기 위해 새로운 인력을 투입한다. 하지만 새로운 인력은 시스템 설계에 대한 조예가 깊지 않다. 사이드 이펙트를 생각하기 어렵다. 결국 더 많은 나쁜 코드를 생산한다.
- 결국 새로운 시스템을 구성하는 재설계를 시작한다.
- 시간을 들여 깨끗한 코드를 만드는 노력이 비용을 절감하는 방법일 뿐만 아니라 전문가로서 살아남는 길이다

## 태도

- 코드가 왜 그렇게 되었을까?
    - 요구사항이 변경돼서?
    - 관리자가 멍청해서?
    - 고객이 닦달해서?
    - 마케팅 부서가 잘못해서?
- 인정하기 어렵겠지만 잘못은 전적으로 프로그래머에게 있다. 우리가 전문가답지 못했기 때문이다.
- 관리자나 다른 팀에서 우리에게 정보를 구하지 않더라도 우리가 적극적으로 정보를 제공해야 마땅하다.
- 사용자는 요구사항을 내놓으며 **우리에게** 현실성을 자문한다.
- 관리자는 일정을 잡으며 **우리에게** 도움을 청한다.
- 그러므로 프로젝트 실패는 우리에게도 커다란 책임이 있다.
- 관리자들이 일정과 요구사항을 강력하게 밀어붙이는 이유는 그것이 그들의 책임이기 때문이고, 좋은 코드를 사수하는 일은 바로 우리 프로그래머들의 책임이다.
- 나쁜 코드의 위험을 이해하지 못하는 관리자의 말을 그대로 따르는 행동은 전문가답지 못하다.
- 기한을 맞추는 유일한 방법은, 그러니까 빨리가는 유일한 방법은, 언제나 코드를 최대한 깨끗하게 유지하는 습관이다.
- 깨끗한 코드가 무엇인지 모르면 깨끗한 코드를 만들려고 애써봤자 소용이 없다.

### **깨끗한 코드에 대한 유명하고 노련한 프로그래머들의 의견**

- 비야네 스트롭스트룹 (C++ 창시자)
    - 보기에 즐거운
    - 효율적인
    - 의존성을 최대한 줄인
    - 오류를 철저히 처리한 - 메모리 누수, 경쟁 상태, 일관성 없는 명명법
    - 세세한 사항까지 꼼꼼하게 처리한
    - 성능을 최적으로 유지한
    - 깨끗한 코드는 한가지에 집중한다. 각 함수와 클래스와 모듈은 주변에 현혹되거나 오염되지 않은 채 한길만 걷는다
- 그래디 부치
    - 단순하고 직접적인
    - **잘 쓴 문장처럼 읽히는 - 가독성이 좋은**
    - 설계자의 의도를 숨기지 않은
    - 명쾌한 추상화와 단순한 제어문으로 가득한
- 큰 데이브 토마스
    - 가독성이 좋은
    - **다른 사람도 고치기 쉬운** - 읽기 쉬운 코드와 고치기 쉬운 코드는 엄연히 다르다!
    - 단위 테스트와 인수 테스트가 존재한 - TDD, 아무리 가독성이 높아도 테스트가 없으면 깨끗하지 않다. `왜 그렇게 말할까?`
    - 의미있는 이름이 붙은
    - 의존성이 최소인
- 마이클 페더스
    - 모든 사항을 고려한 **주의 깊게 작성한** 코드
    - 이 책의 부제, 코드를 주의 깊게 짜는 방법
- 론 제프리스
    - 모든 테스트를 통과한
    - **중복이 없는**
    - 시스템 내 모든 설계 아이디어를 표현한
    - 클래스, 메서드 등을 최대한 줄인
    - 중복과 표현력만 신경 써도 코드가 크게 나아진다. 한 가지 더 고려하자면, 추상 클래스나 추상 메서드를 만들어 실제 구현을 감싸면 여러 가지 장점이 생긴다. (중복 줄이기, 표현력 높이기, 초반부터 간단한 추상화 고려하기)
- 워드 커닝햄
    - 읽으면서 짐작한 대로 돌아가는

- 코드를 읽는 시간과 코드를 짜는 시간 중 어느 것이 더 비율이 높을까?
    - 10:1이 훌쩍 넘는다.
    - 새 코드를 짜면서 우리는 끊임없이 기존 코드를 읽는다.

## 보이스카우트 규칙

- 캠핑장은 처음 왔을 때보다 더 깨끗하게 해놓고 떠나라.
- 잘 짠 코드가 전부가 아니다. 시간이 지나도 언제나 깨끗하게 유지해야 한다. 오늘 작성한 코드는 한 달 뒤에 레거시 코드가 되어 있다.

- 이 책에서는
    - 좋은 코드와 나쁜 코드를 소개할 것이다.
    - 나쁜 코드를 좋은 코드로 바꾸는 방법을 소개할 것이다.
    - 다양한 경험적 교훈과 체계와 절차와 기법도 열거한다.
    - 예제도 무수히 많이 보여준다.
