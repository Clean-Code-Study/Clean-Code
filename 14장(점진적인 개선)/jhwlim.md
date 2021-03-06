# π 14μ₯. μ μ§μ μΈ κ°μ 

## π μμ½

- μ΄ μ₯μ μ μ§μ μΈ κ°μ μ λ³΄μ¬μ£Όλ μ¬λ‘λ₯Ό ν΅ν΄ μ½λκ° κ°μ λλ κ³Όμ μ λ³΄μ¬μ€λ€.
- **κΉ¨λν μ½λλ₯Ό μ§λ €λ©΄ λ¨Όμ  μ§μ λΆν μ½λλ₯Ό μ§  λ€μ μ λ¦¬ν΄μΌ νλ€.** μ΄ μ₯μμλ μ μ§μ μΌλ‘ μ½λλ₯Ό κ°μ νλ λ°©λ²μΌλ‘μ TDD κΈ°λ²μ μ μ©νμλ€.
- **μ½λλ μΈμ λ μ΅λν κΉλνκ³  λ¨μνκ² μ λ¦¬νμ. μ λλ‘ μ©μ΄κ°κ² λ°©μΉνλ©΄ μ λλ€.**
  - μννΈμ¨μ΄ μ€κ³λ **λΆν **λ§ μν΄λ νμ§μ΄ ν¬κ² λμμ§λ€.
  - **μ μ ν μ₯μλ₯Ό λ§λ€μ΄ μ½λλ§ λΆλ¦¬**ν΄λ μ€κ³κ° μ’μμ§λ€.
  - **κ΄μ¬μ¬λ₯Ό λΆλ¦¬**νλ©΄ μ½λλ₯Ό μ΄ν΄νκ³  λ³΄μνκΈ° ν¨μ¬ μ¬μμ§λ€.

## π λͺ©μ°¨

1. [Args κ΅¬ν](#01-args-κ΅¬ν)
2. [Args: 1μ°¨ μ΄μ](#02-args-1μ°¨-μ΄μ)
3. [String μΈμ](#03-string-μΈμ)
4. [κ²°λ‘ ](#04-κ²°λ‘ )

## 01. Args κ΅¬ν

- `Args` ν΄λμ€
  - κΈ°λ₯ : λͺλ Ήν μΈμμ κ΅¬λ¬Έ λΆμμ μν΄ main ν¨μλ‘ λμ΄μ€λ λ¬Έμμ΄ λ°°μ΄μ λΆμνλ€.

**Args.java**

```java
// p247~249
public class Args {

    private Map<Character, ArgumentMarshaler> marshalers;
    private Set<Character> argsFound;
    private ListIterator<String> currentArgument;

    public Args(String schema, String[] args) throws ArgsException { ... }

    private void parseSchema(String schema) throws ArgsException { ... }
    private void parseSchemaElement(String element) throws ArgsException { ... }
    private void validateSchemaElementId(char elementId) throws ArgsException { ... }
    private void parseArgumentStrings(List<String> argsList) throws ArgsException { ... }
    private void parseArgumentCharacters(String argChars) throws ArgsException { ... }
    private void parseArgumentCharacter(char argChar) throws ArgsException { ... }

    public boolean has(char arg) { ... }
    public int nextArgument() { ... }
    public boolean getBoolean(char arg) { ... }
    public String getString(char arg) { ... }
    public int getInt(char arg) { ... }
    public double getDouble(char arg) { ... }
    public String[] getStringArray(char arg) { ... }
}
```

**ArgumentMarshaler.java**

```java
// p250
public interface ArgumentMarshaler {

    void set(Iterator<String> currentArgument) throws ArgsException;
}
```

- `BooleanArguementMarshaler`, `StringArgumentMarshaler`, `IntegerArgumentMarshaler`, `DoubleArgumentMarshaler`, `StringArrayArgumentMarshaler` λ κ°κ° `ArgumentMarshaler` μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ€. (`getXXX()` static λ©μλ κ΅¬ν ν¬ν¨)

**ArgsException.java**

```java
// p251~253
public class ArgsException extends Exception {

    private char errorArgumentId = '\0';
    private String errorParameter = null;
    private ErrorCode errorCode = OK;

    // constructors

    // getter & setter

    public String errorMessage() { ... } // errorCodeλ₯Ό errorMessageλ‘ νμ±νλ€.

    public enum ErrorCode { ... } // errorCodeλ₯Ό λ¬Έμμ΄ μμλ‘ μ μ
}
```

- μμ μ½λμμ μλ‘μ΄ μΈμ μ ν(λ μ§, λ³΅μμ λ±)μ μΆκ°νλ λ°©λ²
  1. `ArgumentMarshaler` μμ μ ν΄λμ€λ₯Ό νμν΄ `getXXX()` ν¨μλ₯Ό μΆκ°νλ€.
  2. `parseSchemaElement()` ν¨μμ μ case λ¬Έλ§ μΆκ°νλ€.
  3. νμνλ€λ©΄ μ `ArgsException.ErrorCode`λ₯Ό λ§λ€κ³  μ μ€λ₯ λ©μμ§λ₯Ό μΆκ°νλ€.
- :star2: μμ κ°μ μ½λλ₯Ό μμ±νλ λ°©λ²
  - **κΉ¨λν μ½λλ₯Ό μ§λ €λ©΄ λ¨Όμ  μ§μ λΆν μ½λλ₯Ό μ§  λ€μ μ λ¦¬ν΄μΌ νλ€.**

## 02. Args: 1μ°¨ μ΄μ

### 1μ°¨ μ΄μ

**Args.java**

```java
// p255~261
public class Args {

    private String schema;
    private String[] args;
    private boolean valid = true;
    private Set<Character> unexpectedArguments = new TreeSet<>();
    private Map<Character, Boolean> booleanArgs = new HashMap<>();
    private Map<Character, String> stringArgs = new HashMap<>();
    private Map<Character, Integer> intArgs = new HashMap<>();
    private Set<Character> argsFound = new HashSet<>();
    private int currentArgument;
    private String errorArgumentId = '\0';
    private String errorParameter = "TILT";
    private ErrorCode errorCode = ErrorCode = ErrorCode.OK;

    private enum ErrorCode { ... }

    public Args(String schema, String[] args) throws ParseException { ... }

    private boolean parse() throws ParseException { ... }
    private boolean parseSchema() throws ParseException { ... }
    private void parseSchemaElement(String element) throws ParseException { ... }
    private void validateSchemaElementId(char elementId) throws ParseException { ... }
    private void parseBooleanSchemaElement(char elementId) { ... }
    private void parseIntegerSchemaElement(char elementId) { ... }
    private void parseStringSchemaElement(char element) { ... }
    private boolean isStringSchemaElement(String elementTail) { ... }
    private boolean isBooleanSchemaElement(String elementTail) { ... }
    private boolean isIntegerSchemaElement(String elementTail) { ... }
    private boolean parseArguments() throws ArgsException { ... }
    private void parseArgument(String arg) throws ArgsException { ... }
    private void parseElements(String arg) throws ArgsException { ... }
    private void parseElement(char argChar) throws ArgsException { ... }
    private boolean setArgument(char argChar) throws ArgsException { ... }
    private boolean isIntArg(char argChar) { ... }
    private void setIntArg(char argChar) throws ArgsException { ... }
    private void setStringArg(char argChar) throws ArgsException { ... }
    private boolean isStringArg(char argChar) { ... }
    private void setBooleanArg(char argChar, boolean value) { ... }
    private boolean isBooleanArg(char argChar) { ... }

    public int cardinality() { ... }
    public String usage() { ... }
    public String errorMessage() throws Exception { ... }
    private String unexpectedArgumentMessage() { ... }
    private boolean falseIfNull(Boolean b) { ... }
    private int zeroIfNull(Integer i) { ... }
    private String blankIfNull(String s) { ... }
    private String getString(char arg) { ... }
    private int getInt(char arg) { ... }
    private boolean getBoolean(char arg) { ... }
    private boolean has(char arg) { ... }
    private boolean isValid() { ... }

    private class ArgsException extends Exception { ... }
}
```

μ μ½λμ λ¬Έμ μ 

- μΈμ€ν΄μ€ λ³μ κ°μκ° λ§λ€.
- `TILT` μ κ°μ ν¬νν λ¬Έμμ΄, HashSetsμ TreeSets, try-catch-catch λΈλ‘ λ± λͺ¨λ μ§μ λΆν μ½λμ κΈ°μ¬νλ μμΈμ΄λ€.

### Booleanλ§ μ§μνλ λ²μ 

```java
// p261~264
public class Args {
    private String schema;
    private String[] args;
    private boolean valid;
    private Set<Character> unexceptedArguments = new TreeSet<>();
    private Map<Character, Boolean> booleanArgs = new HashMap<>();
    private int numberOfArguments = 0;

    public Args(String schema, String[] args) { ... }

    public boolean isValid() { ... }
    private boolean parse() { ... }
    private boolean parseSchema() { ... }
    private void parseSchemaElement(String element) { ... }
    private void parseBooleanSchemaElement(String element) { ... }
    private boolean parseArguments() { ... }
    private void parseArgument(String arg) { ... }
    private void parseElements(String arg) { ... }
    private void parseElement(char argChar) { ... }
    private void setBooleanArg(char argChar, boolean value) { ... }
    private boolean isBoolean(char argChar) { ... }

    public int cardinality() { ... }
    public String usage() { ... }
    public String errorMessage() { ... }
    private String unexceptedArgumentMessage() { ... }

    public boolean getBoolean(char arg) { ... }
}
```

- λλ¦λλ‘ κ΄μ°?μ μ½λλ€. κ°κ²°νκ³  λ¨μνλ©° μ΄ν΄νκΈ°λ μ½λ€.
- νμ§λ§ μ΄νμ Stringκ³Ό IntegerλΌλ μΈμ μ νμ λ κ°λ§ μΆκ°νμ λΏμΈλ°, μ½λκ° μμ²­λκ² μ§μ λΆν΄μ‘λ€.
- μ΄ μ±μμλ Booleanκ³Ό Stringμ μ§μνλ λ²μ λ μκ°νκ³  μμ§λ§ μΈμ μ νμ΄ μΆκ°λλ©΄μ μ½λκ° μλ§μ΄ λκ°λ κ³Όμ μ λ³΄μ¬μ€λ€. (p264~269)

### κ·Έλμ λ©μ·λ€

- Boolean, String, Integer μΈμ μ νμ μΆκ°ν ν([1μ°¨ μ΄μ](#1μ°¨-μ΄μ)) κΈ°λ₯μ λ μ΄μ μΆκ°νμ§ μκ³  λ¦¬ν©ν λ§μ μ§νν¨.
- μ μΈμ μ νμ μΆκ°νκΈ° μν΄μλ μ£Όμ μ§μ  μΈ κ³³μ μ½λλ₯Ό μΆκ°ν΄μΌ νλ€.
  1. μΈμ μ νμ ν΄λΉνλ HashMapμ μ ννκΈ° μν΄ μ€ν€λ§ μμμ κ΅¬λ¬Έμ λΆμνλ€.
  2. λͺλ Ήν μΈμμμ μΈμ μ νμ λΆμν΄ μ§μ§ μ νμΌλ‘ λ°ννλ€.
  3. getXXX λ©μλλ₯Ό κ΅¬νν΄ νΈμΆμμκ² μ§μ§ μ νμ λ°ννλ€.
- μΈμ μ νμ λ€μνμ§λ§ λͺ¨λκ° μ μ¬ν λ©μλλ₯Ό μ κ³΅νλ€. β `ArugmentMarshaler`(μΈν°νμ΄μ€)

### μ μ§μ μΌλ‘ κ°μ νλ€

- β οΈ νλ‘κ·Έλ¨μ λ§μΉλ κ°μ₯ μ’μ λ°©λ² μ€ νλλ κ°μ μ΄λΌλ μ΄λ¦ μλ κ΅¬μ‘°λ₯Ό ν¬κ² λ€μ§λ νμλ€.
- νμ€νΈ μ£Όλ κ°λ°(TDD) κΈ°λ²μ μ¬μ©
  - TDDλ μΈμ  μ΄λ λλΌλ μμ€νμ΄ λμκ°μΌ νλ€λ μμΉμ λ°λ₯Έλ€.
  - TDDλ μμ€νμ λ§κ°λ¨λ¦¬λ λ³κ²½μ νμ©νμ§ μλλ€. λ³κ²½μ κ°ν νμλ μμ€νμ΄ λ³κ²½ μ κ³Ό λκ°μ΄ λμκ°μΌ νλ€.
  - μ΄λ₯Ό μν΄μλ μΈμ λ  μ€νμ΄ κ°λ₯ν μλνλ νμ€νΈ μνΈ νμ
- μμ€νμ μμν λ³κ²½μ κ°νλ©΄μ νμ€νΈλ₯Ό ν΅κ³Όνλμ§ νμΈνλ€. νμ€νΈ μΌμ΄μ€κ° μ€ν¨νλ©΄ λ€μ λ¨κ³λ‘ λμ΄κ°μ§ μκ³  νμ€νΈλ₯Ό ν΅κ³Όν  μ μλλ‘ μ€λ₯λ₯Ό μμ νλ€.

## 03. String μΈμ

- String μΈμλ₯Ό μΆκ°νλ κ²μ μμ μ§νν κ²κ³Ό λ§μ°¬κ°μ§λ‘ **ν λ²μ νλμ© κ³ μΉλ©΄μ νμ€νΈλ₯Ό κ³μ λλ¦°λ€.**
- νμ€νΈ μΌμ΄μ€κ° νλλΌλ μ€ν¨νλ©΄ λ€μ λ³κ²½μΌλ‘ λμ΄κ°κΈ° μ μ μ€λ₯λ₯Ό μμ νλ€.
- Integer μΈμ μ νλ κ°μ κ³Όμ μ λ°λ³΅νλ€.

### μ²« λ²μ§Έ λ¦¬ν©ν λ§μ λλΈ λ²μ 

**Args.java**

```java
// p288~295
public class Args {

    private String schema;
    private String[] args;
    private boolean valid = true;
    private Set<Character> unexpectedArguments = new TreeSet<>();
    private Map<Character, ArgumentMarshaler> marshalers = new HashMap<>();
    private Set<Character> argsFound = new HashSet<>();
    private int currentArgument;
    private char errorArgumentId = '\0';
    private String errorParameter = "TILT";
    private ErrorCode errorCode = ErrorCode.OK;

    private enum ErrorCode { ... }

    public Args(String schema, String[] args) throws ParseException { ... }

    private boolean parse() throws ParseException { ... }
    private boolean parseSchema() throws ParseException { ... }
    private void parseSchemaElement(String element) throws ParseExcpetion { ... }
    private void validateSchemaElementId(char elementId) throws ParseException { ... }
    private boolean isStringSchemaElement(String elementTail) { ... }
    private boolean isBooleanSchemaElement(String elementTail) { ... }
    private boolean isIntegerSchemaElement(String elementTail) { ... }
    private boolean parseArguments() throws ArgsException { ... }
    private void parseArgument(String arg) throws ArgsException { ... }
    private void parseElements(String arg) throws ArgsException { ... }
    private void parseElement(char argChar) throws ArgsException { ... }
    private boolean setArgument(char argChar) throws ArgsException { ... }
    private void setIntArg(ArgumentMarshaler m) throws ArgsException { ... }
    private void setStringArg(ArgumentMarshaler m) throws ArgsException { ... }
    private void setBooleanArg(ArgumentMarshaler m) { ... }

    public int cardinality() { ... }
    public String usage() { ... }
    public String errorMessage() throws Exception { ... }
    private String unexpectedArgumentMessage() { ... }

    public boolean getBoolean(char arg) { ... }
    public String getString(char arg) { ... }
    public int getInt(char arg) { ... }
    public boolean has(char arg) { ... }
    public boolean isValid() { ... }

    private class ArgsException extends Exception {}

    private abstract class ArgumentMarshaler {
        public abstract void set(String s) throws ArgsException;
        public abstract Object get();
    }

    private class BooleanArgumentMarshaler extends ArgumentMarshaler { ... }
    private class StringArgumentMarshaler extends ArgumentMarshaler { ... }
    private class integerArgumentMarshaler extends ArgumentMarshaler { ... }
}
```

μ μ½λμ λ¬Έμ μ 

- μ²«λ¨Έλ¦¬μ λμ€λ λ³μ
- `setArgument()`λ μ νμ μΌμΌμ΄ νμΈνλ€.
- λͺ¨λ  set ν¨μ
- μ€λ₯ μ²λ¦¬ μ½λ

### λ€μ λ¦¬ν©ν λ§ μν

- λ¦¬ν©ν λ§ κ³Όμ  : p295~304
- μλ‘μ΄ μΈμ μ ν μΆκ° μμ : p304~307
- μμΈ/μ€λ₯ μ²λ¦¬ μ½λ λΆλ¦¬ : p307~310

### μΈμ¬μ΄νΈ

- μννΈμ¨μ΄ μ€κ³λ λΆν λ§ μν΄λ νμ§μ΄ ν¬κ² λμμ§λ€.
- μ μ ν μ₯μλ₯Ό λ§λ€μ΄ μ½λλ§ λΆλ¦¬ν΄λ μ€κ³κ° μ’μμ§λ€.
- κ΄μ¬μ¬λ₯Ό λΆλ¦¬νλ©΄ μ½λλ₯Ό μ΄ν΄νκ³  λ³΄μνκΈ° ν¨μ¬ μ¬μμ§λ€.
- `ArgsException`μ `errorMessage()` λ©μλ
  - λͺλ°±ν SRP μλ°μ΄λ€. Args ν΄λμ€κ° μ€λ₯ λ©μμ§ νμκΉμ§ μ±μμ‘κΈ° λλ¬Έμ΄λ€.
  - νμ§λ§ `ArgsException` ν΄λμ€κ° μ€λ₯ λ©μμ§ νμμ μ²λ¦¬ν΄μΌ μ³μκΉ? λ§μ½ `ArgsException`μκ² λ§‘κ²¨μλ μλλ€κ³  μκ°νλ€λ©΄ μλ‘μ΄ ν΄λμ€κ° νμνλ€.
  - νμ§λ§ λ―Έλ¦¬ κΉλνκ² λ§λ€μ΄μ§ μ€λ₯ λ©μμ§λ‘ μ»λ μ₯μ μ λ¬΄μνκΈ° μ΄λ ΅λ€.

## 04. κ²°λ‘ 

- κ·Έμ  λμκ°λ μ½λλ§μΌλ‘λ λΆμ‘±νλ€.
- λμ μ½λλ³΄λ€ λ μ€λ«λμ λ μ¬κ°νκ² κ°λ° νλ‘μ νΈμ μμν₯μ λ―ΈμΉλ μμΈλ μλ€.
- λμ μ½λλ μ©μ΄ λ¬Έλλ¬μ§λ€.
- λ¬Όλ‘  λμ μ½λλ κΉ¨λν μ½λλ‘ κ°μ ν  μ μλ€. νμ§λ§ λΉμ©μ΄ μμ²­λκ² λ§μ΄ λ λ€.
- λ°λ©΄ μ²μλΆν° μ½λλ₯Ό κΉ¨λνκ² μ μ§νκΈ°λ μλμ μΌλ‘ μ½λ€.
- **κ·Έλ¬λ―λ‘ μ½λλ μΈμ λ μ΅λν κΉλνκ³  λ¨μνκ² μ λ¦¬νμ. μ λλ‘ μ©μ΄κ°κ² λ°©μΉνλ©΄ μ λλ€.**
