# 15장 JUnit 들여다보기

### JUnit 프레임워크

- ComparisonCompactor라는 모듈이 있는데 이 모듈은 두 문자열을 받아서 차이를 반환한다.
- 예를 들어 ABCDE와 ABXDE를 받으면 <…B[X]D…>를 반환한다.
- p.327 ComparisonCompactor 모듈에 대한 코드 커버리지 분석을 수행했을 때 100%가 나온다.
  테스트 케이스가 모든 행, 모든 if문, 모든 for문을 실행한다는 뜻이다.
- 코드 커버리지?

  테스트 코드가 프로덕션 코드를 얼마나 실행했는지를 백분율로 나타내는 지표이다. 즉, 테스트 코드가 실제로 프로덕션 코드를 얼마나 몇 퍼센트 검증하고 있는지를 나타낸다. 코드 커버리지를 통해 현재 작성된 테스트 코드의 수가 충분한것인지 논의할 수 있다.

  예를 들어 코드 커버리지 측정 기준이 실행된 함수 개수라고 하자. 프로덕션 코드에 총 100개의 함수가 있고, 테스트 코드가 그 중 50개를 실행했다면 코드 커버리지는 50%가 된다.


- p.329 디팩터링 코드를 보면 모듈을 아주 좋은 상태로 남겨두었으나 보이스카우트 규칙에 따라 우리가 처음 봤을 때보다 더 깨끗하게 해놓고 떠나야 한다.
- 어떻게 개선할 수 있는가?
  - 멤버 변수 앞에 접두어가 붙어 있는데 이는 중복되는 정보이기 때문에 접두어를 제거하는 것이 좋다.
  - 캡슐화되지 않은 조건문을 캡슐화 해준다.

    ```java
    //캡슐화 전 코드
    
    public String compact(String message) {
    	if (expected == null || actual == null || areStringsEqual())
    		return Assert.format(message, expected, actual);
    
    	findCommonPrefix();
    	findCommonSuffix();
    	String expected = compactString(this.expected);
    	String actual = compactString(this.actual);
    	return Assert.format(message, expected, actual);
    }
    
    //캡슐화 후 코드
    
    public String compact(String message) {
    	if (shouldNotCompact())
    		return Assert.format(message, expected, actual);
    
    	findCommonPrefix();
    	findCommonSuffix();
    	String expected = compactString(this.expected);
    	String actual = compactString(this.actual);
    	return Assert.format(message, expected, actual);
    }
    
    private boolean shouldNotCompact() {
    	return expected == null || actual == null || areStringsEqual();
    }
    ```

  - 위 예문을 보면 compact 함수에서 사용하는 this.expected와 this.actual도 눈에 거슬리는데 함수에 이미 지역 변수 expected가 있는데 앞에 접두어를 제거했기 때문이다.
  - 서로 다른 의미의 변수이기 때문에 이름을 명확하게 바꿔 붙여준다.
  - 또한 부정문은 긍정문보다 이해하기 어렵기 때문에 긍정문으로 만든다.

    ```java
    public String compact(String message) {
    	if (canBeCompacted())
    		findCommonPrefix();
    		findCommonSuffix();
    		String compactExpected = compactString(expected);
    		String compactActual = compactString(actual);
    		return Assert.format(message, compactExpected, compactActual);
    	} else {
    		return Assert.format(message, expected, actual);
    	}
    private boolean canBeCompacted() {
    	return expected != null && actual != null || !areStringsEqual();
    }
    ```

  - 함수의 효과를 이름으로 명확하게 설명해야한다. (compact → formatCompactedComparison)

  - 함수는 한가지 일만 해야하기 때문에 압축하는 과정을 메서드로 빼서 만든다.

    ```java
    ...
    private String compactExpected;
    private String compactActual;
    
    ...
    public String formatCompactedComparison(Stirng message) {
    	if (canBeCompacted()) {
    	compactExpectedAndActual();
    	return Assert.format(message, compactExpected, compactActual);
    	} else {
    		return Assert.format(message, expected, actual);
    	}
    }
    
    private void compactExpectedAndActual() {
    	findCommonPrefix();
    	findCommonSuffix();
    	compactExpected = compactString(expected);
    	compactActual = compactString(actual);
    }
    ```

- 숨겨진 시각적인 결합이 존재한다. 아래 코드에서 findCommandSuffix는 findCommonPrefix가 prefixIndex를 계산한다는 사실에 의존하기 때문에 순서가 잘못되면 밤샘 디버깅을 하게 된다.

  ```java
    
  private compactExpectedAndActual() {    
      prefixIndex = findCommonPrefix();
      suffixIndex = findCommonSuffix(prefixIndex);    
      String compactExpected = compactString(expected);    
      String compactActual = compactString(actual);
  } 
    
  private int findCommonSuffix(int prefixIndex) {    
          int expectedSuffix = expected.length() - 1;    
          int actualSuffix = actual.length() - 1;    
          for (actualSuffix >= prefixIndex && expectedSuffix >= 
              prefix; actualSuffix--, expectedSuffix--) {
          if (expected.charAt(expectedSuffix) !=
                       actual.charAt(actualSuffix)) {            
                          break;        
                  }    
          }    
          return expected.length() - expectedSuffix;
  }
    
  ```


---

p.338의 최종 버전 코드를 보면 보이스카우트 규칙도 지키면서 처음보다 조금 더 코드가 깔끔해진 것을 볼 수 있다.
