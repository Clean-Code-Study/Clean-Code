# 14장 점진적인 개선

### Args 구현

---

- 코드 열기


    ```java
    import java.util.*;
    
    public class Args {
        private String schema;
        private Map<Character, ArgumentMarshaler> marshalers =
                                                 new HashMap<Character, ArgumentMarshaler>();
        private Set<Character> argsFound = new HashSet<Character>();
        private Iterator<String> currentArgument;
        private List<String> argsList;
    
        public Args(String schema, String[] args) throws ArgsException {
            this.schema = schema;
            argsList = Arrays.asList(args);
            parse();
        }
    
        private void parse() throws ArgsException {
            parseSchema();
            parseArguments();
        }
    
        private boolean parseSchema() throws ArgsException {
            for (String element : schema.split(",")) {
                if (element.length() > 0) {
                    parseSchemaElement(element.trim());
                }
            }
            return true;
        }
    
        private void parseSchemaElement(String element) throws ArgsException {
            char elementId = element.charAt(0);
            String elementTail = element.substring(1);
            validateSchemaElementId(elementId);
            if (elementTail.length() == 0)
                marshalers.put(elementId, new BooleanArgumentMarshaler());
            else if (elementTail.equals("*"))
                marshalers.put(elementId, new StringArgumentMarshaler());
            else if (elementTail.equals("#"))
                marshalers.put(elementId, new IntegerArgumentMarshaler());
            else if (elementTail.equals("##"))
                marshalers.put(elementId, new DoubleArgumentMarshaler());
            else
                throw new ArgsException(ArgsException.ErrorCode.INVALID_FORMAT,
                    elementId, elementTail);
        }
    
        private void validateSchemaElementId(char elementId) throws ArgsException {
            if (!Character.isLetter(elementId)) {
                throw new ArgsException(ArgsException.ErrorCode.INVALID_ARGUMENT_NAME,
                    elementId, null);
            }
        }
    
        private void parseArguments() throws ArgsException {
            for (currentArgument = argsList.iterator(); currentArgument.hasNext();) {
                String arg = currentArgument.next();
                parseArgument(arg);
            }
        }
    
        private void parseArgument(String arg) throws ArgsException {
            if (arg.startsWith("-"))
                parseElements(arg);
        }
    
        private void parseElements(String arg) throws ArgsException {
            for (int i = 1; i < arg.length(); i++)
                parseElement(arg.charAt(i));
        }
    
        private void parseElement(char argChar) throws ArgsException {
            if (setArgument(argChar))
                argsFound.add(argChar);
            else {
                throw new ArgsException(ArgsException.ErrorCode.UNEXPECTED_ARGUMENT,
                    argChar, null);
            }
        }
    
        private boolean setArgument(char argChar) throws ArgsException {
            ArgumentMarshaler m = marshalers.get(argChar);
            if (m == null)
                return false;
            try {
                m.set(currentArgument);
                return true;
            } catch (ArgsException e) {
                e.setErrorArgumentId(argChar);
                throw e;
            }
        }
        public int cardinality() {
            return argsFound.size();
        }
    
        public String usage() {
            if (schema.length() > 0)
                return "-[" + schema + "]";
            else
                return "";
        }
    
        public boolean getBoolean(char arg) {
            ArgumentMarshaler am = marshalers.get(arg);
            boolean b = false;
            try {
                b = am != null && (Boolean) am.get();
            } catch (ClassCastException e) {
                b = false;
            }
            return b;
        }
    
        public String getString(char arg) {
            ArgumentMarshaler am = marshalers.get(arg);
            try {
                return am == null ? "" : (String) am.get();
            } catch (ClassCastException e) {
                return "";
            }
        }
    
        public int getInt(char arg) {
            ArgumentMarshaler am = marshalers.get(arg);
            try {
                return am == null ? 0 : (Integer) am.get();
            } catch (Exception e) {
                return 0;
            }
        }
    
        public double getDouble(char arg) {
            ArgumentMarshaler am = marshalers.get(arg);
            try {
                return am == null ? 0 : (Double) am.get();
            } catch (Exception e) {
                return 0.0;
            }
        }
    
        public boolean has(char arg) {
            return argsFound.contains(arg);
        }
    }
    
    ```

- 위 코드가 위에서 아래로 읽힌다는 것에 주목해라

**ArgumentMarshaler 인터페이스와 파생클래스**

```java
import java.util.Iterator;

public interface ArgumentMarshaler {
    void set(Iterator<String> currentArgument) throws ArgsException;
    Object get();
}
```

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.INVALID_BOOLEAN;
import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_BOOLEAN;

public class BooleanArgumentMarshaler implements ArgumentMarshaler {
  private boolean booleanValue = false;

  public void set(Iterator<String> currentArgument) throws ArgsException {
    String parameter = null;
    try {
      parameter = currentArgument.next();
      booleanValue = Boolean.parseBoolean(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_BOOLEAN);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_BOOLEAN, parameter);
    }
  }

  public Object get() {
    return booleanValue;
  }
}
```

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_STRING;

public class StringArgumentMarshaler implements ArgumentMarshaler {
  private String stringValue = "";

  public void set(Iterator<String> currentArgument) throws ArgsException {
    try {
      stringValue = currentArgument.next();
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_STRING);
    }
  }

  public Object get() {
    return stringValue;
  }
}
```

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.INVALID_INTEGER;
import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_INTEGER;

public class IntegerArgumentMarshaler implements ArgumentMarshaler {
  private int intValue = 0;

  public void set(Iterator<String> currentArgument) throws ArgsException {
    String parameter = null;
    try {
      parameter = currentArgument.next();
      intValue = Integer.parseInt(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_INTEGER);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_INTEGER, parameter);
    }
  }

  public Object get() {
    return intValue;
  }
}
```

```java
public class ArgsException extends Exception {
    private char errorArgumentId = '\0';
    private String errorParameter = "TILT";
    private ErrorCode errorCode = ErrorCode.OK;

    public ArgsException() {}

    public ArgsException(String message) {super(message);}

    public ArgsException(ErrorCode errorCode) {
        this.errorCode = errorCode;
    }
    public ArgsException(ErrorCode errorCode, String errorParameter) {
        this.errorCode = errorCode;
        this.errorParameter = errorParameter;
    }

    public ArgsException(ErrorCode errorCode, char errorArgumentId,
        String errorParameter) {
        this.errorCode = errorCode;
        this.errorParameter = errorParameter;
        this.errorArgumentId = errorArgumentId;
    }

    public char getErrorArgumentId() {
        return errorArgumentId;
    }

    public void setErrorArgumentId(char errorArgumentId) {
        this.errorArgumentId = errorArgumentId;
    }

    public String getErrorParameter() {
        return errorParameter;
    }

    public void setErrorParameter(String errorParameter) {
        this.errorParameter = errorParameter;
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }

    public void setErrorCode(ErrorCode errorCode) {
        this.errorCode = errorCode;
    }

    public String errorMessage() throws Exception {
        switch (errorCode) {
            case OK:
            throw new Exception("TILT: Should not get here.");
            case UNEXPECTED_ARGUMENT:
            return String.format("Argument -%c unexpected.", errorArgumentId);
            case MISSING_STRING:
            return String.format("Could not find string parameter for -%c.",
                errorArgumentId);
            case INVALID_INTEGER:
            return String.format("Argument -%c expects an integer but was '%s'.",
                errorArgumentId, errorParameter);
            case MISSING_INTEGER:
            return String.format("Could not find integer parameter for -%c.",
                errorArgumentId);
            case INVALID_DOUBLE:
            return String.format("Argument -%c expects a double but was '%s'.",
                errorArgumentId, errorParameter);
            case MISSING_DOUBLE:
            return String.format("Could not find double parameter for -%c.",
                errorArgumentId);
        }
        return "";
    }

    public enum ErrorCode {
        OK, INVALID_FORMAT, UNEXPECTED_ARGUMENT, INVALID_ARGUMENT_NAME,
        MISSING_STRING,
        MISSING_INTEGER, INVALID_INTEGER,
        MISSING_DOUBLE, MISSING_BOOLEAN, INVALID_BOOLEAN, INVALID_DOUBLE}
    }

```

위 코드들 중 오류 코드 상수를 정의하는 ArgeException.java코드가 눈에 거슬릴 것이다.

단순한 개념을 구현하는데 비해 코드가 너무 많이 작성되어있기 때문이다.

코드가 길어진 데에 이유 중 하나는 java를 사용했기 때문이다.

java는 정적 타입 언어이기 때문에 타입 시스템을 만족하려면 많은 단어가 필요하다.

루비, 파이썬, 스몰토크와 같은 언어를 사용했다면 프로그램이 훨씬 작아졌을 것이다.

---

**위 코드는 크기는 크지만 전반적으로 깔끔한 구조에 속한다 위 코드를 어떻게 짰을까?**

- 깨끗한 코드를 짜기 위해서는 지저분한 코드를 짠 뒤에 정리해야한다.
- 작문과 같이 코드도 단계적으로 개선해야한다.
- 대부분의 신입 프로그래머는 돌아가는 프로그램을 목표로 잡는다.
  돌아가기만 하면 다음 업무로 넘어간다는 것이다.
- 경력이 쌓인 프로그래머들은 이런 행동이 코드를 망치는 행위라는 사실을 잘 안다.

1. Args: 1차 초안(p.255)
- 돌아가지만 엉망인 코드
- 변수의 개수도 많고 희한한 문자열과 HashSets, TreeSets, try-catch-finally 블록 등 코드를 지저분하게 하는 코드들이 들어가 있다.
- 처음 boolean 인수를 사용했을 때는 이렇게까지 엉망이지는 않았다. 하지만 엉망으로 가는 씨앗이 있었기 때문에 엉망이 된 것이다.
- 인수 유형을 String과 Integer만 추가 했는데에도 코드가 지저분해진 것이다.
- 저자는 인수 유형을 단계적으로 추가 했다. (String 추가한 코드 → p.264)
- 위 코드가 통제를 벗어나기 시작하는데 여기서 Integer 인수를 추가하면서 완전히 엉망이 되었다.

**그래서 멈췄다**

---

- 추가할 인수가 적어도 두 개가 더 있는데 그것을 추가하면 코드가 훨씬 나빠질 것이다.
- 코드 구조를 유지보수하기 좋은 상태로 만들기 위해서는 여기서 수정해야한다.
- 새 인수 유형을 추가 하기 위해서는 주요 지점에 추가해야한다.

**점진적으로 개선하다**

---

- 프로그램을 망치는 가장 좋은 방법은 개선이라는 이름 아래 구조를 크게 뒤집는 행위이다.
- 그저그런 개선은 프로그램을 전과 똑같이 돌리는 것이 아주 어렵기 때문이다.
- 그래서 TDD, 즉 테스트 주도 개발 기법을 사용해야하는데 TDD는 언제 어느 때라도 시스템이 돌아가야 한다는 원칙을 따른다. 다시 말해 시스템을 망가뜨리는 변경을 허용하지 않는다는 것이다.
- 변경 전후가 시스템이 똑같이 돌아간다는 것을 확인하기 위해서는 언제는 실행이 가능한 테스트 슈트가 필요하다.
- 이후 단위 테스트와 인수 테스트가 잘 돌아가는 한에서 자잘한 변경을 가하면 된다.

### String 인수

---

- 위의 경우들과 같이 한 번에 하나씩 고치면서 테스트를 계속 돌린다.
- 인수 유형을 처리하는 코드를 하나의 클래스에 넣고 파생클래스를 만들어 코드를 분리하면 프로그램의 구조가 변경되는 동안에도 시스템이 정상적으로 작동 되는 것을 유지하기 쉬워진다.

---

리팩터링을 하다보면 코드를 넣었다 뺐다 하는 사례가 아주 흔하다.

단계적으로 조금씩 변경하며 매번 테스트를 돌려야 하기때문에 코드를 여기저기 옮길 일이 많아진다.

---

- 소프트웨어 설계는 분할만 잘해도 품질이 크게 높아진다.
- 적절한 장소를 만들어 코드만 분리해도 설계가 좋아진다.
- 관심사를 분리하면 코드를 이해하고 보수하기 훨씬 더 쉬워진다.
