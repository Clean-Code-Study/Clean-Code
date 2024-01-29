# 5장 형식 맞추기

### 형식을 맞추는 목적

---

- 코드의 형식은 너무 중요하기 때문에 맹목적으로 따르면 안된다.
- 코드 형식은 의사소통의 일환이고 의사소통은 전문 개발자에게 일차적인 의무이다.
    - 형식을 맞추는게 왜 의사소통이라고 생각하는가?
        - 형식이 안맞으면 다른 사람의 코드를 이해하는데 시간이 오래걸린다.
        - 클래스명, 변수명, 메소드명 등등 컨벤션을 따르지 않는다면 다른 사람의 코드를 이해하는데 시간이 오래걸린다.
- 오늘 구현한 기능이 다음 버전에서 바뀔 확률은 높지만 오늘 구현한 코드의 가독성은 앞으로 바뀔 코드의 품질에 큰 영향을 미친다.
- 원래 코드가 완전히 바뀌어도 처음 잡아 놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미친다.

**원활한 소통을 장려하는 코드형식은 무엇일까?**

### 적절한 행 길이를 유지해라

---

- 코드의 길이가 500줄을 넘지 않고 대부분 200줄 정도인 파일로도 커다란 시스템을 구축할 수 있다.
- 일반적으로 큰 파일보다 작은 파일이 이해하기 쉽기 때문에 이것을 바람직한 규칙으로 삼으면 좋다.

**신문 기사처럼 작성하라**

우리는 기사를 위에서 아래로 읽는다. 최상단에 기사를 몇 마디로 요약하는 표제가 나오는데 이를 보고 기사를 읽을지 말지 결정한다. 첫 문단에는 기사 내용이 요약되어있고 세세한 사실은 숨기고 커다란 그림을 보여준다. 세세한 사실은 글을 읽으면서 조금씩 드러나게 한다.

소스파일 또한 이와 비슷하게 작성한다. 이름은 간단하되 이름만 보고도 올바른 모듈을 살펴보고 있는지 아닌지 판단할 수 있을 정도로 짓는다. 첫 부분은 고차원 개념과 알고리즘을 설명하고 내려갈 수록 의도를 세세하게 묘사한다.

- 코드를 읽을 때 위에서 아래로 읽을 수 있도록 작성하자
- public 메서드를 위에, private 메서드를 아래에 작성하자

**개념은 빈 행으로 분리하라**

각 행은 수식이나 절을 나타내고, 행 묶음은 완결된 생각 하나를 표현한다. 생각 사이는 빈 행을 넣어 분리해야한다. 빈 행은 `새로운 개념을 시작한다는 시각적 단서이기 때문에 빈 행으로 분리하는 것이 마땅하다`

예문

```java
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(,+?)'''",);

	public BoldWidget(parentWidget parent, String text) throws Exception{
		super(parent);
		Matcher match -= pattern.matcher(text);
		match.find();
		addChildWidgets(match.group(1));
	.
	.
	.
	.

}
```

**세로 밀집도**

줄바꿈이 개념을 분리한다면 세로 밀집도는 연관성을 의미한다.

서로 밀접한 코드 행은 세로로 가까이 놓여야한다는 뜻이다.

예문1

```java
public class REporterConfig{
/**
* 리포터 리스너의 클래스 이름
*/
	private String m_className;

/**
* 리포터 리스너의 속성
*/
	private List<Property> m_properties = new ArrayList<Property>();
	public void addProperty(Property property){
		m_properties.add(property);
}
```

예문2

```java
public class REporterConfig{
	private String m_className;
	private List<Property> m_properties = new ArrayList<Property>();
	public void addProperty(Property property){
		m_properties.add(property);
	}
}
```

예문 1보다 예문2가 ‘한눈’에 들어온다.

**수직거리**

밀접한 두 개념은 세로 거리로 연관성을 표현한다. 연관성은 한개념을 이해하는 데 다른 개념이 중요한 정도이다.

연관성이 깊은 두 개념이 떨어져 있으면 코드를 읽는 사람이 소스파일을 여기저기 뒤지게 된다.

1. 변수선언
    - 변수는 사용하는 위치에 최대한 가까이에 선언한다.
    - 지역 변수는 각 함수 맨 처음에 선언한다.
    - 루프를 제어하는 변수는 루프 문 내부에 선언한다.
    - 긴 함수에서는 블록 상단이나 루프 직전에 변수를  선언하기도 한다.
2. 인스턴스 변수
    - 인스턴스 변수는 클래스 맨 처음에 선언한다.
    - 변수 간에 세로로 거리를 두지 않는다.
    - 잘 설계한 클래스는 많은 클래스 메서드가 인스턴스 변수를 사용하기 때문이다.
3. 종속 함수
    - 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이에 배치한다.
    - 호출하는 함수를 호출되는 함수보다 먼저 배치한다.
    - 독자가 방금 호출한 함수가 잠시 후에 정의될 것이라고 예측하기 때문이다.
    - 이로써 호출되는 함수를 찾기 쉬워지고, 모듈의 가독성도 높아진다.
4. 개념적 유사성
    - 개념적 친화도가 높을수록 코드를 가까이에 배치한다.
    - 친화도가 높은 요인
        - 한 함수가 다른 함수를 호출할 때
        - 변수와 그 변수를 사용하는 함수
        - 비슷한 동작을 수행하는 함수


**세로 순서**

함수 호출 종속성은 아래 방향으로 유지한다.

호출되는 함수를 호출하는 함수보다 나중에 배치한다. 이렇게하면 소스 코드 모듈이 고차원에서 저차원으로 자연스럽게 내려오기 때문이다.

가장 중요한 개념을 먼저 표현하고 세세한 사항은 마지막에 표현하도록 한다.

### 가로 형식 맞추기

---

- 행이 짧을 수록 바람직하다.
- 100자나 120자에 달하는 것까지는 나쁘지 않다.
- 그 이상은 주의 부족
- 요즘에는 모니터가 커지고 젊은 프로그래머들은 글꼴 크기를 줄여 200자까지도 한 화면에 들어가게하는데 그렇게 하지 말기를 권한다.

**가로 공백과 밀집도**

```java
private void measureLine(String line) {
	lineCount++;
	int lineSize = line.length();
	totalChars += lineSize;
	lineWidthHistogram.addLine(lineSize, lineCount);
	recordWidestLine(lineSize);
}
```

위의 예문을 보면 할당연산자를 강조하기 위해 앞뒤에 공백을 줬다.

공백을 넣으면 두 가지 주요 요소가 확실하게 나뉜다는 사실이 분명해진다.

반면에, 함수 이름과 이어지는 괄호 사이에는 공백을 넣지 않았는데 이는 함수와 인수는 서로 밀접하기 때문이다.

공백을 사용하면 한 개념이 아닌 별개로 보인다.

연산자 우선순위를 강조하기 위해서도 공백을 사용한다.

```java
private static double determinant(double a, double b, double c){
return b*b - 4*a*c;
}
```

곱셈은 우선순위가 가장 높기 때문에 숫자 사이에 공백을 넣지 않는다. 덧셈과 뺄셈의 경우 우선순위가 곱셈보다 낮기 때문에 공백을 사용한다.

**가로정렬**

```java
public class FitNesseExpediter implements ResponseSender
    {
        private Socket       socket;
        private InputStream  input;
        private OutputStream output;
        private Request      request;
        .
				.
				.

    }
```

위의 예문은 가로정렬을 한 코드이다.

코드가 엉뚱한 부분을 강조해 진짜 의도가 가려지기 때문에 가로정렬은 별로 유용하지 못하다.

선언문과 할당문을 별도로 정렬하지 않으면 오히려 중대한 결함을 찾기 쉽다.

**들여쓰기**

- 들여쓰기를 하면 파일 구조가 한눈에 들어온다. (vscode 맥: shift + command + f)
- 변수, 생성자 함수, 접근자 함수, 메서드가 금방 보인다.
- 들여쓰기가 없으면 코드읽기가 매우 어렵다.(예문 p111-p112)

**가짜범위**

빈 while문이나 for문은 사용하지 않는 것이 좋으나 쓰게 된다면 세미콜론을 새 행에 제대로 들여써서 넣어준다.

```java
while (dis.read(buf, 0, readBufferSize) != -1)
;

// Bad Example
while (dis.read(buf, 0, readBufferSize) != -1)
;

// Good Example
while (dis.read(buf, 0, readBufferSize) != -1) {
  ...
}
```

- 내용이 없는 빈 while문이나 for문은 되도록 사용하지 말자!

### 팀 규칙

---

프로그래머라면 각자 선호하는 규칙이있다. 그럼에도 팀에 속하면 내가 선호해야 하는 규칙은 팀 규칙이다.

개개인이 맘대로 짜대는 코드는 피해야한다. 구현 스타일을 논의해야한다.

- 괄호를 넣을 위치
- 들여쓰기를 몇자로 할 것인지
- 클래스와 변수와 메서드 이름을 어떻게 지을지

좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이루어 지기때문에 스타일을 일관적이고 매끄럽게 해야한다.

그렇게함으로써 독자에게 신뢰감을 주어야한다.

입사하면 개발환경을 셋팅을 하는데 팀에서 사용하는 코드 스타일이 있어 그냥 그걸로 맞추면 돼
