# 7장 오류 처리

오류 처리(예외 처리)는 프로그램에 반드시 필요한 요소 중 하나이다. 오류 처리 코드로 인해 프로그램을 이해하기 어려워진다면 깨끗한 코드라 부르기 어렵다.

이 장에서는 깨끗하고 튼튼한 코드에 한걸음 더 다가가는 단계로 `오류를 처리하는 기법`과 `고려 사항`을 소개한다.

---

### 오류 코드보다 예외를 사용하라

- 로직과 오류를 처리하는 알고리즘을 분리하자
  - 오류 플래그를 설정하거나 호출자에게 오류 코드를 반환하는 코드
    - 함수를 호출한 즉시 오류를 확인해야 하기 때문에 호출자 코드가 복잡해짐

      ```java
      public class DeviceController {
        ...
        public void sendShutDown() {
          DeviceHandle handle = getHandle(DEV1);
          // 디바이스 상태를 점검
          if (handle != DeviceHandle.INVALID) {
            // 레코드 필드에 디바이스 상태 저장
            retrieveDeviceRecord(handle);
            //디바이스가 일지정지 상태가 아니라면 종료
            if (record.getStatus() != DEVICE_SUSPENDED) {
              pauseDevice(handle);
              clearDeviceWorkQueue(handle);
              closeDevice(handle);
            } else {
              logger.log("Device suspended. Unable to shut down");
            }
          } else {
            logger.log("Invalid handle for: " + DEV1.toString());
          }
        }
        ...
      }
      ```

  - 오류를 발견하면 예외를 던지는 코드
    - 논리와 오류 코드가 뒤섞이지 않기 때문에 호출자 코드가 더 깔끔해짐

      ```java
      public class DeviceController {
      
          public void sendShutDown() {
              try {
                  tryToShutDown();
              } catch (DeviceShutDownError e) {
                  logger.log(e);
              }
          }
      
          private void tryToShutDown() throws DeviceShutDownError {
              DeviceHandle handle = getHandle(DEV1);
              DeviceRecord record = retrieveDeviceRecord(handle);
      
              pauseDevice(handle);
              clearDeviceWorkQueue(handle);
              closeDevice(handle);
          }
      
          private DeviceHandle getHandle(DeviceId id) {
              ...
              throw new DeviceShutDownError("Invalid handle for: " + id.toString());
          }
      
          ...
      }
      ```


### Try-Catch-Finally 문부터 작성하라

- 예외가 발생할 코드를 짤 때는 try-catch-finally 문으로 시작하는 편이 낫다.
  - try 블록에서 무슨 일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 하기 때문에 try 블록은 트랜잭션과 비슷하다.
    - 트랜잭션: 데이터의 일관성을 보장하기 위해서 사용
  - try 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워진다.
- 예제
  - 파일이 없으면 예외를 던지는지 알아보는 단위테스트이다.

    ```java
    @Test(expected = StorageException.class)
    public void retrieveSectionShouldThrowOnInvalidFileName() {
        sectionStore.retrieveSection("invalid - file");
    }
    
    public List<RecordedGrip> retrieveSection(String sectionName) {
        return new ArrayList<RecordedGrip>();
    }
    ```

  - 예외를 던지지 않기 때문에 테스트가 실패한다. 아래 코드는 예외를 던진다.

    ```java
    public List<RecordedGrip> retrieveSection(String sectionName) {
        try {
            FileInputStream stream = new FileInputStream(sectionName)
        } catch (Exception e) {
            throw new StorageException("retrieval error", e);
        }
        return new ArrayList<RecordedGrip>();
    }
    ```

  - 코드가 예외를 던지므로 테스트가 성공한다.
- 먼저 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 권장한다. 그러면 자연스럽게 try블록의 트랜잭션 범위부터 구현하게 되므로 범위 내 트랜잭션 본질을 유지하기 쉬워진다.

### 미확인(Unchecked) 예외를 사용하라

- **확인된 예외는 OCP를 위반한다**. 즉, 하위 단계에서 확인된 예외를 던진다면 상위 단계 메서드 선언부에도 전부 해당 예외를 정의해야 한다.
- 예시
  - 최상위 함수가 아래 함수를 호출하고 아래 함수는 그 아래 함수를 호출한다. 단계를 내려갈수록 호출하는 함수의 수는 늘어난다.
  - 최상위 함수를 변경해 새로운 오류를 던진다고 가정하자. 확인된 오류를 던진다면 함수는 선언부에 throws 절을 추가해야하고, 아래의 함수 모두가 1) catch 블록에서 새로운 예외를 처리하거나 2) ㄱ선언부에 throw 절을 추가해야한다.
  - throw 경로에 위치하는 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 캡슐화가 깨진다.
- `**C**heckedException`과  `UncheckedException`  **의 차이**
  - 예외 처리 여부와 확인 시점이 다르다. CheckedException은 컴파일러가 체크하여 명시적인 예외 처리를 필요로 하며, 컴파일 단계에서 확인됩니다. 반면에 UncheckedException은 명시적인 예외 처리가 필요하지 않으며, 실행 단계에서 발생한다


### 예외에 의미를 제공하라

오류 메시지에 정보를 담아 예외와 함께 던진다. 실패한 연산 이름과 실패 유형도 언급한다. 이렇게 하면 오류가 발생한 원인과 위치를 찾기 쉬워진다.

### 호출자를 고려해 예외 클래스를 정의하라

- 오류를 정의할 때 오류를 잡아내는 방법이 기준이 되어야 한다.
- 오류를 형편없이 분류한 사례
  - 중복이 심함, 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일
  - 외부 라이브러리를 호출하는 try-catch-finally 문을 포함한 코드로, 외부 라이브러리가 던질 예외를 모두 잡아낸다.

    ```java
    ACMEPort port = new ACMEPort(12);
    
    try {
      port.open();
    } catch (DeviceResponseException e) {
      reportPortError(e);
      logger.log("Device response exception", e);
    } catch (ATM1212UnlockedException e) {
      reportPortError(e);
      logger.log("Unlock exception", e);
    } catch (GMXError e) {
      reportPortError(e);
      logger.log("Device response exception");
    } finally {
      ...
    }
    ```

- 호출하는 라이브러리 API를 감싸면서 예외 유형 하나를 반환하는 방법
  - LocalPort 클래스는 단순히 ACMEPort 클래스가 던지는 예외를 잡아 변환하는 wrapper 클래스일 뿐이다.
  - `외부 API를 사용할 때는 감싸기 기법`이 최선이다.
    - 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어들기 때문이다.
    - 나중에 다른 라이브러리로 갈아타도 비용이 적다.
    - 감싸기 클래스에서 외부 API를 호출하는 대신 테스트 코드를 넣어주는 방법으로 테스트하기도 쉬워진다.
    - 감싸기 기법을 사용하면 특정 업체가 API를 설계한 방식에 발목 잡히지 않는다.

    ```java
    LocalPort port = new LocalPort(12);
    try {
      port.open();
    } catch (PortDeviceFailure e) {
      reportError(e);
      logger.log(e.getMessage(), e);
    } finally {
      ...
    }
    
    public class LocalPort {
      private ACMEPort innerPort;
      public LocalPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
      }
    
      public void open() {
        try {
          innerPort.open();
        } catch (DeviceResponseException e) {
          throw new PortDeviceFailure(e);
        } catch (ATM1212UnlockedException e) {
          throw new PortDeviceFailure(e);
        } catch (GMXError e) {
          throw new PortDeviceFailure(e);
        }
      }
      ...
    }
    ```


### 정상 흐름을 정의하라

- catch문에서 예외를 처리하는 경우 코드가 지저분해지는 일이 발생할 수 있다.

    ```java
    try {
        MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
        m_total += expenses.getTotal();
    } catch(MealExpensesNotFound e) {
        m_total += getMealPerDiem();
    }
    ```

- 식비를 비용으로 청구했다면 그걸 더하고, 아니면 일일 기본 식비를 더하는 코드이다. 만약 청구식비가 없으면 일일 기본 식비를 반환하도록 DAO를 수정하면 아래와 같이 간결하게 된다.

    ```java
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
    ```

- 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식인 특수 사례 패턴이다.
- 이렇게 하면 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하므로 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어지다.

### null을 반환하지 마라

- null 확인이 너무 많아도 문제다.
- 메서드에서 null을 반환하고픈 유혹이 든다면 그 대신 예외를 던지거나 특수 사례 객체를 반환한다.
- 사용하려는 외부 API가 null을 반환한다면 감싸기 메서드를 구현해 예외를 던지거나 특수 사례 객체를 반환하는 방식을 고려한다.
- 예시

    ```java
    List<Employee> employees = getEmployees();
    if (employees != null) {
        for(Employee e : employees) {
            totalPay += e.getPay();
        }
    }
    ```

  - 위에서 직원 조회할 때 null이 아닌 빈 리스트를 반환한다면 코드가 훨씬 깔끔해지고, NullPointerException이 발생할 가능성도 줄어든다.

    ```java
    public List<Employee> getEmployees() {
    	if( .. 직원이 없다면 .. )
    		return **Collections.emptyList();**
    	}
    }
    
    List<Employee> employees = getEmployees();
    for(Employee e : employees) {
        totalPay += e.getPay();
    }
    ```


### null을 전달하지 마라

- 메서드에서 null을 반환하는 방식도 나쁘지만 메서드로 null을 전달하는 방식은 더 나쁘다.
- 예제
  - 다음은 두 지점 사이의 거리를 계산하는 메서드이다.

      ```java
      public class MetricsCalculator { 
        public double xProjection(Point p1, Point p2) { 
          return (p2.x – p1.x) * 1.5; 
        } 
      }
      ```

  - 누군가 인수로 null을 전달하면 당연히 NullPointerException이 발생한다.
  - 어떻게 고치면 좋을까? 함수 내에서 null 검사를 해야 한다.

      ```java
      double xProjection(Point p1, Point p2) {
          if(p1 == null || p2 == null){
              throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
          }
          return (p2.x - p1.x) * 1.5;
      }
      ```

  - 다른 대안으로 `assert`문을 사용하는 방법도 있지만, 여전히 `NullPointException` 문제를 해결해 줄 수는 없다.

      ```java
      double xProjection(Point p1, Point p2) {
          assert p1 != null : "p1 should not be null";
          assert p2 != null : "p2 should not be null";
      
          return (p2.x - p1.x) * 1.5;
      }
      ```

  - 애초에 null을 넘기지 못하도록 금지하는 정책이 합리적이다.
