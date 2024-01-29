# 4장 주석

주석은 가능한 줄이도록 하자. 좋은 주석과 나쁜 주석의 예시를 보여준다.

---

```
나쁜 코드에 주석을 달지 마라. 새로 짜라.
- 브라이언 W. 커니핸, P.J. 플라우거
```

- 잘 달린 주석은 그 어떤 정보보다 유용하다. 경솔하고 근거 없는 주석은 코드를 이해하기 어렵게 만든다. 오래되고 조잡한 주석은 거짓과 잘못된 정보를 퍼뜨리게 된다.
- 우리가 주석을 다는 이유?
    - 우리는 코드로 의도를 표현하지 못하고 `실패를 만회하기 위해` 주석을 사용한다. 주석은 언제나 실패를 의미한다. 때때로 주석 없이는 표현할 방법을 찾지 못해 할 수 없이 주석을 사용한다.
    - 그러므로 주석이 필요한 상황에는 다시 한 번 코드로 의도를 표현할 수 없을지 생각해봐야 한다. 코드로 의도를 표현할 때마다 스스로를 칭찬해주고, 주석을 달 때마다 자신에게 표현력이 없다는 사실을 푸념해야 마땅하다.
- 주석은 오래될수록 코드에서 멀어지고 완전히 그릇될 가능성도 커진다.
    - 왜? 프로그래머들이 주석을 유지하고 보수하기란 현실적으로 불가능하니까.
    - 코드는 변화하고 진화하기 때문에 주석이 언제나 코드를 따라가지는 못한다.
- 주석을 엄격하게 관리해야 한다고 주장할 수도 있다. 하지만 나라면 코드를 깔끔하게 정리하고 표현력을 강화하는 방향으로, 애초에 주석이 필요 없는 방향으로 에너지를 쏟겠다.
- 진실은 코드에만 존재한다. 코드만이 자신이 하는 일을 진실되게 말하고, 정확한 정보를 제공하는 유일한 출처다. 그러므로 우리는 주석을 가능한 줄이도록 꾸준히 노력해야 한다.

## 주석은 나쁜 코드를 보완하지 못한다.

---

- 코드에 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문이다.
- 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

## 코드로 의도를 표현하라!

---

- 다음의 예제를 살펴보자. 어느 쪽이 더 나은가?

    ```java
    // 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
    if ((emplotee.flags & HOURLY_FLAG) && (employee.age > 65)
    ```

- 다음 코드는 어떤가?

    ```java
    if (employee.isEligibleForFullBenefits())
    ```

- 몇 초만 더 생각하면 코드로 대다수의 의도를 표현할 수 있다. 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

## 좋은 주석

어떤 주석은 필요하거나 유익하다.

---

### 법적인 주석

- 각 소스 파일 첫머리에 들어가는 저작권 정보와 소유권 정보 등

    ```java
    // Copyright (C) 2003, 2004, 2005 by Object Montor, Inc. All right reserved.
    // GNU General Public License 버전 2 이상을 따르는 조건으로 배포한다.
    ```


### 정보를 제공하는 주석

- 다음 주석은 추상 메서드가 반환할 값을 설명한다.

    ```java
    // 테스트 중인 Responder 인스턴스를 반환
    protected abstract Responder responderInstance();
    ```

    - 위와 같은 주석이 유용할지라도, 가능하다면 함수 이름(responderBeingTested)에 정보를 담는 편이 더 좋다.
- 더 나은 예

    ```java
    // kk:mm:ss EEE, MMM dd, yyyy 형식이다.
    Pattern timeMatcher = Pattern.compile("\\d*:\\d*\\d* \\w*, \\w*, \\d*, \\d*");
    ```

    - 위의 주석은 코드에서 사용한 정규표현식이 시각과 날짜를 뜻한다고 설명한다. 하지만 시각과 날짜를 변환하는 클래스를 만들어 코드를 옮겨주면 더 깔끔하고 주석이 필요 없어진다.

### 의도가 분명하게 드러나는 주석

```java
// 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다. 
for (int i = 0; i > 2500; i++) {
    WidgetBuilderThread widgetBuilderThread = 
        new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
    Thread thread = new Thread(widgetBuilderThread);
    thread.start();
}
```

### 결과를 경고하는 주석

```java
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {
}
```

- 요즘에는 @Ignore 속성을 이용해 테스트 케이스를 꺼버린다.

### TODO 주석

- 앞으로 할 일을 //TODO 주석으로 남기면 편하다. 다음은 함수를 구현하지 않은 이유와 미래 모습을 설명한 예제다.

    ```java
    // TODO-MdM 현재 필요하지 않다.
    // 체크아웃 모델을 도입하면 함수가 필요 없다.
    protected VersionInfo makeVersion() throws Exception {
        return null;
    }
    ```

- TODO 주석은 프로그래머가 필요하다 여기지만 당장 구현하기 어려운 업무를 기술한다.
- 더이상 필요 없는 기능을 삭제하라는 알림, 누군가에게 문제를 봐달라는 요청, 더 좋은 이름을 떠올려달라는 부탁, 앞으로 발생할 이벤트에 맞춰 코드를 고치라는 주의 등에 유용하다.
- 요즘은 IDE를 통해 남은 TODO를 쉽게 볼 수 있으므로 편리하게 이용할 수 있다. 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애라고 권한다.

### 중요성을 강조하는 주석

레거시 코드 > 복잡하고 오래된 코드

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다. 
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

### 공개 API에서 Javadocs

- 설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다. 공개 API를 구현한다면 반드시 훌륭한 Javadocs 작성을 추천한다. 하지만 여느 주석과 마찬가지로 Javadocs 역시 독자를 오도하거나, 잘못 위치하거나, 그릇된 정보를 전달할 가능성이 존재한다.

## 나쁜 주석

대다수의 주석이 이 범주에 속한다. 일반적으로 대다수 주석은 허술한 코드를 지탱하거나, 엉성한 코드를 변명하거나, 미숙한 결정을 합리화하는 등 프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.****

---

### 주절거리는 주석

- 특별한 이유없이 의무감으로 마지못해 다는 주석이다.

    ```java
    public void loadProperties() {
        try {
            String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
            FileInputStream propertiesStream = new FileInputStream(propertiesPath);
            loadedProperties.load(propertiesStream);
        } catch (IOException e) {
            // 속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미다. 
        }
    }
    ```

- 이해가 안 되어 다른 모듈까지 뒤져야 하는 주석은 독자와 제대로 소통하지 못하는 주석이다.

### 같은 이야기를 중복하는 주석

- 코드 내용을 그대로 중복으로 작성한 주석이다

    ```java
    // this.closed가 true일 때 반환되는 유틸리티 메서드다.
    // 타임아웃에 도달하면 예외를 던진다. 
    public synchronized void waitForClose(final long timeoutMillis) throws Exception {
        if (!closed) {
            wait(timeoutMillis);
            if (!closed) {
                throw new Exception("MockResponseSender could not be closed");
            }
        }
    }
    ```


### 오해할 여지가 있는 주석

- this.closed가 true로 변하는 순간에 메서드는 반환되지 않는다. this.closed가 true**여야** 메서드는 반환된다. 아니면 무조건 타임아웃을 기다렸다 this.closed가 그래도 true가 **아니면** 예외를 던진다.
- 주석에 담긴 '살짝 잘못된 정보'로 인해 맞지 않는 상황해서 함수를 호출할지도 모른다.

### 의무적으로 다는 주석

- 모든 함수에 Javadocs를 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석기 그지없다. 이런 주석은 코드를 복잡하게 만들며, 거짓말을 퍼뜨리고, 혼동과 무질서를 초래한다. 아래와 같은 주석은 아무 가치도 없다.

    ```java
    /**
     *
     * @param title CD 제목
     * @param author CD 저자
     * @param tracks CD 트랙 숫자
     * @param durationInMinutes CD 길이(단위: 분)
     */
    public void addCD(String title, String author, int tracks, int durationInMinutes) {
        CD cd = new CD();
        cd.title = title;
        cd.author = author;
        cd.tracks = tracks;
        cd.duration = durationInMinutes;
        cdList.add(cd);
    }
    ```


### 이력을 기록하는 주석

- 예전에는 모든 모듈 첫머리에 변경 이력을 기록하고 관리했지만 지금은 소스 코드 관리 시스템이 있으니 완전히 제거하는 편이 좋다.

    ```java
    * 변경 이력 (11-Oct-2001부터)
    * ------------------------------------------------
    * 11-Oct-2001 : 클래스를 다시 정리하고 새로운 패키징
    * 05-Nov-2001: getDescription() 메소드 추가
    ```


### 있으나 마나 한 주석

- 너무 당연한 사실을 언급하며 새로운 정보를 제공하지 못하는 주석이다.

    ```java
    /*
     * 기본 생성자
     */
    protected AnnualDateRule() {
    
    }
    ```


### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (module.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

- 주석을 없애고 다시 표현하면 다음과 같다.

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### 위치를 표시하는 주석

- 때때로 프로그래머는 소스 파일에서 특정 위치를 표시하려 주석을 사용한다. 예를 들어, 최근에 살펴보던 프로그램에서 다음 행을 발견했다.

    ```java
    // Actions ////////////////////////////////////////
    ```


### 닫는 괄호에 다는 주석

- 닫는 괄호에 주석을 달아야겠다는 생각이 든다면 대신에 함수를 줄이려 시도하자.

    ```java
    try{
    	while ((line = in.readLine()) != null) {
    		...
    	} //while
    } //try
    ```


### 공로를 돌리거나 저자를 표시하는 주석

- 소스 코드 관리 시스템은 누가 언제 무엇을 추가했는지 기억한다.

    ```java
    /* 릭이 추가함 */
    ```


### 주석으로 처리한 코드

- 1960년대 즈음에는 주석으로 처리한 코드가 유용했었지만 우리는 우수한 소스 코드 관리 시스템을 사용하기 때문에 우리를 대신에 코드를 기억해준다.

    ```java
    this.bytePos = writeBytes(pngIdBytes, 0);
    //hdrPos = bytePos;
    writeHeader();
    writeResolution();
    //dataPos = bytePos;
    if (writeImageData()) {
        wirteEnd();
        this.pngBytes = resizeByteArray(this.pngBytes, this.maxPos);
    } else {
        this.pngBytes = null;
    }
    return this.pngBytes;
    ```


### HTML 주석

- 소스 코드에서 HTML 주석은 혐오 그 자체다.
- HTML 주석은 IDE에서도 읽기 힘들다. 쓰지 말자.

### 전역 정보

- 주석을 달아야 한다면 근처에 있는 코드만 기술하라. 코드 일부에 주석을 달면서 시스템의 전반적인 정보를 기술하지 마라.

    ```java
    /**
     * 적합성 테스트가 동작하는 포트: 기본값은 <b>8082</b>.
     *
     * @param fitnessePort
     */
    public void setFitnessePort(int fitnessePort) {
        this.fitnewssePort = fitnessePort;
    }
    ```


### 너무 많은 정보

- 주석에다 흥미로운 역사나 관련 없는 정보를 장황하게 늘어놓지 마라.

### 모호한 관계

- 주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.

### 함수 헤더

- 짧은 함수는 긴 설명이 필요 없다. 짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훨씬 좋다.

### 비공개 코드에서 Javadocs

- 공개 API는 Javadocs가 유용하지만 공개하지 않을 코드라면 Javadocs는 쓸모가 없다.
- 시스템 내부에 속한 클래스와 함수에 Javadocs를 생성할 필요는 없다. 코드만 보기싫고 산만해질 뿐이다.

레거시 코드 분석할 때는 주석을 많이 달지만 실제로 개발할 때는 거의 안달아요~
