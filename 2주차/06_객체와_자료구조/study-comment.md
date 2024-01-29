# 6장 객체와 자료구조

객체와 자료구조란 무엇이고, 그 차이는 무엇일까? 결론부터 설명하자면,

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다.
- 자료구조는 자료를 그대로 공개하며 별다른 함수를 제공하지 않는다. 다시 말해, 메모리에 저장된 변수 값들의 모음이다. 어떤 변수들이 하나의 의미를 구성한다면 이들을 묶어 하나의 자료구조로 구성한다.

### 자료 추상화

```java
public interface Point {
	double getX();
	double getY();
	void setCartesian(double x, double y);
	double getR();
	double getTheta();
	void setPolar(double r, double theta);
}
```

위 코드는 자료 구조 이상을 표현한다. 접근 정책을 강제하는 것이다. 읽을 때는 각 값을 개별적으로 읽어야 하지만, 설정할 때는 두 값을 한꺼번에 설정해야한다.

또, 위 인터페이스를 구현한 클래스들의 내부에 어떤 좌표계로 저장되어 있을지 알 수 없다.

```java
public class Point {
	public double x;
	public double y;
}
```

위 코드는 구현을 노출한다. 변수를 private으로 선언하더라도 각 값마다 get(조회) 함수와 set(설정) 함수를 제공한다면 구현을 외부로 노출하는 셈이다.

그저 변수 사이에 함수를 넣는다고 구현이 저절로 감춰지는 것이 아니다. 즉, 형식적으로 getter, setter로 변수를 다룬다고 객체가 되지 않는다.

클라이언트가 내부 구현을 모른채로 사용할 수 있어야 진정한 의미의 객체이다. 즉, 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 편이 좋다.

그렇다면 인터페이스를 사용하면 무조건 추상화할 수 있을까? 아니다. 개발자는 객체가 포함하는 자료를 표현할 가장 좋은 방법을 심각하게 고민해야 한다.

- 추가

    ```java
    public class Employee {
    	private int id;
    	private String name;
    
    	public Employee(int id, String name) {
    		this.id = id;
    		this.name = name;
    	}
    
    	public int getId() {
    		return id;
    	}
    
    	public void setId(int id) {
    		this.id = id;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public void setName(String name) {
    		this.name = name;
    	}
    }
    ```

    - 자바 수업에서 VO를 이런 형식으로 만들었던 기억이 있다. 위와 같이 해야 추상화가 되어 객체가 된다? 위와 같은 코드는 bean 규약에 따른 코딩일 뿐 자료구조를 객체로 만들어주지는 못한다고 볼 수 있다. 그냥 변수들을 모아 놓은 자료구조일 뿐이다.

### 자료/객체 비대칭

객체와 자료구조, 두 개념은 사실상 정반대이다.

```java
public class Square {
	public Point topLeft;
	public double side;
}

public class Rectangle {
	public Point topLeft;
	public double height;
	public double width;
}

public class Circle {
	public Point center;
	public double radius;
}

public class Geometry {
	public final double PI = 3.14

	public double area(Object shape) throws NoSuchshapeException {
		if (shape instanceof Square) {
				Square s = (Square)shape;
				return s.side * s.side;
		} else if (shape instanceof Rectangle) {
				Rectangle r = (Rectangle)shape;
				return r.height * r.wide;
		} else if (shape instanceof Circle) {
				Circle c = (Circle)shape;
				return PI * c.radius * r.radius;
		}
		throw new NoSuchShapeException();
	}
}
```

위 코드에서 3개의 도형 클래스는 자료구조이다. 여기서 새로운 도형을 추가한다면? 각 클래스들은 아무 영향도 받지 않지만 area() 함수가 변경이 일어난다. area() 와 같은 함수가 여러개라면?

(자료구조를 사용하는) 절차적인 코드는 새로운 자료구조를 추가하는 것이 어렵다. 하지만, 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다.

```java
public class Square implements Shape {
	private Point topLeft;
	private double side;

	public double area() {
		return side * side;
	}
}

public class Rectangle implements Shape {
	private Point topLeft;
	private double height;
	private double width;

	public double area() {
		return height * width;
	}
}

public class Circle implements Shape {
	private Point center;
	private double radius;
	public final double PI = 3.14;

	public double area() {
		return PI * c.radius * r.radius;
	}
```

위 코드는 이전 코드의 객체지향 버전이다. 여기서는 새로운 도형을 추가하는 것은 쉽고 기존 함수에 아무런 영향을 미치지 않는다. 하지만, Shape에 새로운 함수를 추가하려면 모든 도형 클래스(하위 클래스)를 수정해야한다.

객체 지향 코드에서는 함수를 추가하는 것이 어렵다. 하지만, 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.

### 디미터 법칙

디미터 법칙은 사용하는 객체의 내부 구현을 몰라야 한다는 법칙이다. 앞에서 봤듯이, 객체는 자료를 숨기고 함수를 공개한다. 즉, 객체는 getter로 내부 구조를 공개하면 안된다는 의미이다. 그러면 내부 구조를 노출하는 셈이기 때문이다.

좀 더 정확히 말하자면, 다른 객체의 함수가 반환한 객체의 함수를 호출해서는 안된다.

```java
String a = b.c().d().e().f();
```

이렇게 다른 객체의 구현을 줄줄이 노출하는 코드를 ****train wreck(기차 충돌)이라고 부른다. 일반적으로 조잡하다 여겨지는 방식이므로 피하는 편이 좋다.

디미터 법칙을 위반하는지 여부는 객체인지 아니면 자료 구조인지에 따라 달라진다. 객체라면 내부 구조를 숨겨야 하므로 디미터 법칙을 위반한다. 반면, 자료구조라면 당연히 내부 구조를 노출하므로 디미터 법칙이 적용되지 않는다.

때때로 절반은 객체, 절반은 자료 구조인 **잡종 구조**가 나온다. 이런 잡종 구조는 새로운 함수를 추가하는 것도 어렵고 새로운 자료 구조도 추가하기 어렵다. 즉, 양쪽의 단점만 모아놓은 구조다. 프로그래머가 함수나 타입을 보호할지 공개할지 확신하지 못해 어중간하게 내놓은 설계에 불과하다.

### 자료 전달 객체

```java
public class Address {
	private String street;
	private String zip;

	public Address(String street, String zip) {
		this.street = street;
		this.zip = zip;
	}

	public String getStreet() {
		return street;
	}

	public String getZip() {
		return zip;
	}
}
```

자료 구조체(Data Transter Object, DTO)의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스이다. DTO는 데이터베이스와 통신하거나 소켓에서 받은 메시지의 구문을 분석할 때 유용하다.

흔히 DTO는 데이터베이스에 저장된 가공되지 않은 정보를 어플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음으로 사용하는 구조체다.

좀 더 일반적인 형태는 bean 구조이다. (**아마도 VO?**)

DTO의 특수한 형태로 활성 레코드가 있다. 공개 변수가 있거나 비공개 변수에 getter, setter가 있는 자료구조지만, save나 find와 같은 탐색 함수도 제공한다. 활성 레코드는 자료 구조로 취급한다. 규칙을 추가해 객체로 취급하면 안된다.
