# 8장 경계


---

시스템에 들어가는 모든 소프트웨어를 직접 개발하는 경우는 드물다.

패키지를 사고, 오픈 소스를 이용한다. 때로는 사내 다른 팀이 제공하는 컴포넌트를 사용해야만한다.

그래서 우리는 이 외부코드를 어떤 식으로든 우리 코드에 깔끔하게 통합할 수 있어야한다.

### 외부 코드 사용하기

---

- 인터페이스 제공자는 적용성을 최대한 넓히고자 한다.
- 인터페이스 사용자는 자신의 요구에 집중하기를 바란다.

```java
clear() void - Map
containsKey(Object key) boolean - Map
containsValue(Object value) boolean - Map
entrySet() Set - Map
equals(Object o) boolean - Map
get(Object key) Object - Map
getClass() Class<? extends Object> -Object
.
.
.
```

- Map이 제공하는 기능성과 유연성은 확실히 유용하지만 그만큼 위험하기도 하다.
- 프로그램에서 Map을 만들어 여기저기 넘긴다고 가정하자. 넘기는 쪽에서는 아무도 Map 내용을 삭제하지 않을 것이라고 믿을 지도 모르겠다.
- 하지만 Map이 제공하는 메소드 중 첫번째 clear() 메서드가 있다.
- 즉, 사용자 누구나 Map의 내용을 지울 권한이 있다는 것이다.
- 또한, 설계 시 Map에 특정 객체 유형만 저장하기로 결정했다고 해도 Map은 객체 유형을 제한하지 않기 때문에 사용자가 마음만 먹으면 어떤 객체라도 추가할 수 있다.

```java
Map sensors = new HashMap();//Sensor라는 객체를 담는 Map

Sensor s = (Sensor)sensors.get(sensorId);//Sensor 객체가 필요한 코드는 다음과 같이 Sensor객체를 가져옴
```

- 위와 같은 코드가 한 번이 아니라 여러 차례 나온다.
- 또한 의도가 분명히 드러나지 않는다.
- 대신 다음과 같이 제네릭스를 사용하면 코드 가독성이 높아진다.

```java
Map<String, Sensor> sensors = new HashMAp<Sensor>();
...
Sensor s = sensors.get(sensorId);
```

- 위 코드는 가독성이 높지만 “Map<String, Sensor>가 사용자에게 필요하지 않은 기능까지 제공한다.”는 문제는 해결하지 못한다.
- 프로그램에서 Map<String, Sensor> 인스턴스를 여기저기로 넘긴다면, Map 인터페이스가 변할 경우, 수정할 코드가 많아진다.
- 다음은 Map을 좀 더 깔끔하게 사용한 코드이다. 아래처럼 제네릭스의 사용 여부는 Sensors 안에서 결정되기 때문에 사용자가 제네릭스가 사용되었는지에 대해서 신경쓸 필요가 없다.

```java
public class Sensors {
	private Map sensors = new HashMap();

	public Sensor getById(String id) {
		return (Sensor) sensors.get(id);
	}
	// 이하 생략
}
```

- 위 코드를 보면 경계 인터페이스인 Map을 Sensors 클래스 안으로 숨긴다. 따라서 Map 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않는다. 즉, 제네릭스를 사용하든 안하든 더 이상 문제가 안 된다는 것이다.
- Sensors 클래스는 프로그램에 필요한 인터페이스만 제공하기 때문에 코드는 이해하고 쉽고 오용하는 것은 어렵다.
- Map클래스를 사용할 때마다 캡슐화 하라는 것이 아닌 Map을 여기저기 넘기지 말라는 것이다.
- Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 말아야한다.

### 경계 살피고 익히기

---

- 외부 패키지 테스트가 우리 책임은 아니다. 하지만 우리 자신을 위해 우리가 사용할 코드를 테스트하는 편이 바람직하다.
- 외부 코드를 호출하는 것이 아닌 곧바로 우리쪽 코드를 작성하고 간단한 테스트 케이스를 통해 외부 코드를 익혀라. 이름 짐 뉴커크는 학습 테스트라 부른다.
- 학습 테스트는 외부 API를 호출한다. 통제된 환경에서 API를 제대로 이해하는지를 확인하는 셈이다.
- 학습테스트는 API를 사용하려는 목적에 초점을 맞춘다.

### 학습 테스트는 공짜 이상이다

---

- 학습 테스트는 패키지가 예상대로 도는지 검증한다.
- 패키지는 새 버전이 나올 때마다 새로운 위험이 생긴다.
- 새 버전이 우리 코드와 호환되지 않으면 학습 테스트가 이 사실을 곧바로 밝혀낸다.
- 실제 코드와 동일한 방식으로 인터페이스를 사용하는 테스트 케이스가 필요하다.

### 아직 존재하지 않는 코드를 사용하기(아는 코드와 모르는 코드를 분리하는 경계)

---

- 우리가 바라는 인터페이스를 구현하면 우리가 인터페이스를 전적으로 통제한다는 장점이 생긴다.
- 코드의 가독성도 높아지고 코드의 의도도 분명해진다.
- 우리가 통제하지 못하며 정의되지도 않은 송신기 API에서 CommunicationsController를 분리했다.
- 우리에게 필요한 인터페이스를 정의했으므로 깔끔하고 깨끗한 코드를 가진 CommunicationsController를 구현할 수 있다.
- 송신기 API가 정의된 후에는 Adapter 패턴으로 API 사용을 캡슐화해 API가 바뀔 때 수정할 코드를 한곳으로 모았다.
- 테스트도 간단하다.
- 요약

  잘 모르는 외부코드가 아닌 본인이 잘 아는 코드의 경우 전적으로 통제할 수 있고 테스트 하는 것 또한 어렵지 않기 때문에 본인이 알고 있는 것으로 본인의 코드를 작성하는 것이 좋다.


### 깨끗한 경계

---

경계에서는 흥미로운 일이 많이 벌어지는데 변경이 그 대표적인 예이다.

소프트웨어 설계가 우수하면 변경에 있어 많은 투자와 재작업이 필요하지 않다. 또한 엄청난 시간과 노력과 재작업을 요구하지도 않는다.
통제하지 못하는 코드는 그 반대이니 각별히 주의할 필요가 있다.

경계에 위치하는 코드는 깔끔하게 분리한다.

통제 불가능한 외부 패키지보다 우리 코드에 의존하는 편이 훨씬 좋다.

외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리해야한다.
