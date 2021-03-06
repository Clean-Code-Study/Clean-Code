# π 15μ₯. Junit λ€μ¬λ€λ³΄κΈ°

## π λͺ©μ°¨

1. [Junit νλ μμν¬](#01-junit-νλ μμν¬)
2. [κ²°λ‘ ](#02-κ²°λ‘ )

## λ€μ΄κ°κΈ° μ μ

- μ΄ μ₯μμλ JUnit νλ μμν¬μμ κ°μ Έμ¨ μ½λλ₯Ό νκ°νκ³ , κ°μ ν΄λ³Έλ€.

## 01. Junit νλ μμν¬

- λ¬Έμμ΄ λΉκ΅ μ€λ₯λ₯Ό νμν  λ μ μ©ν λͺ¨λ(μ½λ)μΈ `ComparisonCompactor`λ₯Ό μ΄ν΄λ³Έλ€.
- `ComparisonComparctor`λ λ λ¬Έμμ΄μ λ°μ μ°¨μ΄λ₯Ό λ°ννλ€.

### νμ€νΈμ½λ μ΄ν΄λ³΄κΈ°

```java
// ComparisonCompactorTest (p324~327)
public void testMessage() { ... }
public void testStartSame() { ... }
public void testEndSame() { ... }
public void testSame() { ... }
public void testNoContextStartAndEndSame() { ... }
public void testStartAndEndContext() { ... }
public void testStartAndEndContextWithEllipses() { ... }
public void testComparisonErrorStartSameComplete() { ... }
public void testComparisonErrorEndSameComplete() { ... }
public void testComparisonErrorEndSameCompleteContext() { ... }
public void testComparisonErrorOverlapingMatches() { ... }
public void testComparisonErrorOverlapingMatchesContext() { ... }
public void testComparisonErrorOverlapingMatches2() { ... }
public void testComparisonErrorOverlapingMatches2Context() { ... }
public void testComparisonErrorWithActualNull() { ... }
public void testComparisonErrorWithActualNullContext() { ... }
public void testComparisonErrorWithExpectedNull() { ... }
public void testComparisonErrorWithExpectedNullContext() { ... }
public void testBug609972() { ... }
```

- μ νμ€νΈ μΌμ΄μ€λ‘ ComparisonCompactor λͺ¨λμ λν μ½λ μ»€λ²λ¦¬μ§ λΆμμ 100% κ° λμ¨λ€. <br> μ΄λ νμ€νΈ μΌμ΄μ€κ° λͺ¨λ  ν, λͺ¨λ  ifλ¬Έ, λͺ¨λ  forλ¬Έμ μ€ννλ€λ μλ―Έμ΄λ€.

> λ€μμ μ€λͺνλ `ComparisonCompactor` μλ³Έμ μ΄ν΄λ³΄λ©΄, public λ©μλλ `compact()` νλ μΈλ°, μ΄λ₯Ό νμ€νΈνκΈ° μν΄μ μμ κ°μ΄ λ§μ νμ€νΈ μΌμ΄μ€λ₯Ό μμ±νλ€. νμ€νΈλ₯Ό μμ±νλ κ²μ μ€μμ±μ μ±μ μ½μΌλ©΄μ κ³μ λλΌκ³  μμ§λ§, νμ€νΈλ₯Ό μμ±νλ κ²μ μ­μ μ΄λ €μ΄ μΌμΈ κ² κ°λ€.

### `ComparisonCompactor` (μλ³Έ)

```java
// p327~329
public class ComparisonCompactor {

    private static final String ELLIPSIS = "...";
    private String final String DELTA_END = "]";
    private String final String DELTA_START = "[";

    private int fContextLength;
    private String fExpected;
    private String fActual;
    private int fPrefix;
    private int fSuffix;

    public ComparisonCompactor(int contextLength, String expected, String actual) {
        fContextLength = contextLength;
        fExpected = expected;
        fActual = actual;
    }

    public String compact(String message) { ... }

    private String compactString(String source) { ... }
    private void findCommonPrefix() { ... }
    private void findCommonSuffix() { ... }
    private String computeCommonPrefix() { ... }
    private String computeCommonSuffix() { ... }
    private boolean areStringsEqual() { ... }
}
```

### μλ μ½λλ₯Ό μ΄λ»κ² κ°μ νλ©΄ μ’μκΉ?

1. μ λμ΄ `f` <br> β μ€λλ  μ¬μ©νλ κ°λ° νκ²½μμλ λ³μ μ΄λ¦μ λ²μλ₯Ό λͺμν  νμκ° μλ€. μ λμ΄ `f`λ μ€λ³΅λλ μ λ³΄λ€.

   ```java
   // before
   private int fContextLength;
   private String fExpected;
   private String fActual;
   private int fPrefix;
   private int fSuffix;

   // after
   private int contextLength;
   private String expected;
   private String actual;
   private int prefix;
   private int suffix;
   ```

2. compact ν¨μ μμλΆμ μΊ‘μνλμ§ μμ μ‘°κ±΄λ¬Έ <br> β μλλ₯Ό λͺνν νννκΈ° μν΄ μ‘°κ±΄λ¬Έμ μΊ‘μννλ€. μ¦, μ‘°κ±΄λ¬Έμ λ©μλλ‘ λ½μλ΄ μ μ ν μ΄λ¦μ λΆμΈλ€.

   ```java
   // before
   if (expected == null || actual == null || areStringsEqual()) { ... }

   // after
   if (shouldNotCompact()) { ... } // λ©μλλ‘ λΆλ¦¬νλ€.
   ```

3. ν¨μμμ λ©€λ² λ³μμ μ΄λ¦μ΄ λκ°μ λ³μλ₯Ό μ¬μ©νλ€. <br> β μλ‘ λ€λ₯Έ μλ―Έμ΄κΈ° λλ¬Έμ μ΄λ¦μ λͺννκ² λΆμΈλ€.

   ```java
   // before
   String expected = compactString(this.expected);
   String actual = compactString(this.actual);

   // after
   String compactExpected = compactString(expected);
   String compactActual = compactString(actual);
   ```

   > λ©€λ² λ³μμ μ΄λ¦μ΄ λκ°μ λ³μλ₯Ό μ¬μ©ν΄μ λ¬Έμ κ° μλ€κΈ°λ³΄λ€λ, `compactString()` λ©μλλ₯Ό ν΅ν΄ ν λ² κ°κ³΅λλ κ°μΈλ° λκ°μ μ΄λ¦μ μ¬μ©νκΈ° λλ¬Έμ μ μ νμ§ μμλ³΄μΈλ€.

4. λΆμ λ¬Έμ κΈμ λ¬Έλ³΄λ€ μ΄ν΄νκΈ° μ½κ° λ μ΄λ ΅λ€. <br> β κΈμ μΌλ‘ λ§λ€κ³  μ‘°κ±΄λ¬Έμ λ°μ νλ€.

   ```java
   // before
   if (shouldNotCompact()) {
       // μμΆμ μ§ννμ§ μλλ€.
   }
   // μμΆμ μ§ννλ€.

   // after
   if (canBeCompact()) { // `shouldNotCompact()`μ μ‘°κ±΄μ λ°μ νκ³ , λ©μλ μ΄λ¦μ `canBeCompact()`λ‘ μμ νλ€.
       // μμΆμ μ§ννλ€.
   } else {
       // μμΆμ μ§ννμ§ μλλ€.
   }
   ```

   > else λ¬Έμ μ­μ νλ κ²μ΄ λ μ’μ κ² κ°λ€.
   > λν, μμμ μ μν μ½λλ³΄λ€λ μλμ κ°μ΄ `shouldNotCompact()` λΆλΆλ§ `!canBeCompact()`λ‘ λ°κΎΈλ κ²μ΄ λ μ’μ§ μμκΉ?

   ```java
   if (!canBeCompact()) {
       // μμΆμ μ§ννμ§ μλλ€.
   }
   // μμΆμ μ§ννλ€.
   ```

   > κ·Έλ¦¬κ³  μμΆμ μ§ννμ§ μλλ€λ κ²μ Exceptionμ μ΄μ©ν΄μ μ²λ¦¬νλ κ²λ μ½λκ° μ’ λ κΉλν΄μ§ μ μλ λ°©λ²μ΄ μλκΉ μκ°λλ€.

5. `compact()` ν¨μ μ΄λ¦μ΄ μ μ νμ§ μλ€. λ¬Έμμ΄μ μμΆνλ ν¨μμ΄μ§λ§ μ€μ λ‘λ `canBeCompacted()`κ° `false`μ΄λ©΄ μμΆνμ§ μλλ€. ν¨μμ compactλΌλ μ΄λ¦μ λΆμ΄λ©΄ μ€λ₯ μ κ²μ΄λΌλ λΆκ° λ¨κ³κ° μ¨κ²¨μ§λ€. λν, ν¨μλ λ¨μν μμΆλ λ¬Έμμ΄μ΄ μλλΌ νμμ΄ κ°μΆ°μ§ λ¬Έμμ΄μ λ°ννλ€. <br> β `formatCompactedComparison` μ΄λΌλ μ΄λ¦μ΄ μ ν©νλ€.

   ```java
   // before
   public String compact(String message) { ... }

   // after
   public String formatCompactedComparison(String message) { ... }
   ```

   > νμ§λ§ μΆμν κ΄μ μμ public λ©μλμ μ΄λ¦μ΄ λλ¬΄ κ΅¬μ²΄μ μ΄μ§ μμκ°?

6. μμ λ¬Έμμ΄κ³Ό μ€μ  λ¬Έμμ΄μ μ§μ§λ‘ μμΆνλ λΆλΆμ `compactExpectedAndActual()` λ©μλλ‘ λΆλ¦¬νλ€. νμ§λ§ νμμ λ§μΆλ μμμ formatCompactedComparisonμκ² μ μ μΌλ‘ λ§‘κΈ΄λ€. `compactExpectedAndActual`μ μμΆλ§ μννλ€.

   ```java
   // before
   public formatCompactedComparison(String message) {
       if (canBeCompacted()) {
           findCommonPrefix();
           findCommonSuffix();
           String compactExpected = compactString(expected);
           String compactActual = compactString(actual);
           return Assert.format(message, compactExpected, compactActual);
       } else {
           // μμΆνμ§ μλλ€.
       }
   }

   // after
   private String compactExpected;
   private String compactActual;

   public formatCompactedComparison(String message) {
       if (canBeCompacted()) {
           compactExpectedAndActual();
           return Assert.format(message, compactExpected, compactActual);
       } else {
           // μμΆνμ§ μλλ€.
       }
   }

   private void compactExpectedAndActual() { ... }
   ```

7. `compactExpectedAndActual()` λ΄λΆμμ ν¨μμ μ¬μ©λ°©μμ΄ μΌκ΄μ μ΄μ§ λͺ»νλ€. λν, λ©€λ² λ³μ μ΄λ¦λ μ’ λ μ ννκ² λ°κΎΌλ€.

   ```java
   // before
   private int prefix;
   private int suffix;

   private void compactExpectedAndActual() {
       findCommonPrefix();
       findCommonSuffix();
       compactExpected = compactString(expected);
       compactActual = compactString(actual);
   }

   private void findCommonPrefix() { ... }
   private void findCommonSuffix() { ... }

   // after
   private int prefixLength;
   private int suffixLength;

   private void compactExpectedAndActual() {
       findCommonPrefixAndSuffix();
       compactExpected = compactString(expected);
       compactActual = compactString(actual);
   }

   private void findCommonPrefixAndSuffix() { ... }
   private int findCommonPrefix() { ... }
   private char charFromEnd(String s, int i) { ... }
   private boolean suffixOverlapsPrefix(int suffixLength) { ... }
   ```

   > νμ§λ§ κ²°κ΅­ ν¨μμ μ¬μ©λ°©μμ΄ μΌκ΄μ μ΄μ§ λͺ»νλ€λ μ μ κ°μ νμ§ λͺ»ν κ²μΌλ‘ λ³΄μ΄λ©°, ν¨μλ₯Ό μΌκ΄μ μΌλ‘ μ¬μ©ν΄μΌ νλ κ²μ΄ λ°λμ μ³μ κ²μΈμ§ λͺ¨λ₯΄κ² λ€. μν©μ λ°λΌ κ°μ λ°νν  μλ μκ³ , κ°μ λ°νν  νμκ° μμ μλ μλλ° μΈμμ μΌλ‘ ν¨μμ μ¬μ©λ°©μμ μΌμΉμν€λ κ²μ΄ μ€νλ € μΈμμ μΈ κ²μΌλ‘ λ³΄μ¬μ§λ€.

8. prefixLengthμ suffixLengthλ μΈμ λ 1 μ΄μμ΄κΈ° λλ¬Έμ `compactString()` λ΄λΆμμ ifλ¬Έμ΄ νμμλ€.

   ```java
   // before
   private String compactString(String source) {
       String result = DELTA_START
           + source.substring(prefixLength, source.length() - suffixLength)
           + DELTA_END;
       if (prefixLength > 0) {
           result = computedCommonPrefix() + result;
       }
       if (suffixLength > 0) {
           result = result + computedCommonSuffix();
       }
       return result;
   }

   // after
   private String compactString(String source) {
       return computeCommonPrefix()
           + DELTA_START
           + source.substring(prefixLength, source.length() - suffixLength)
           + DELTA_END
           + computeCommonSuffix();
   }
   ```

### `ComparisonCompactor` (μ΅μ’ λ²μ )

```java
// p338~341
public class ComparisonCompactor {
    private static final String ELLIPSIS = "...";
    private static final String DELTA_END = "]";
    private static final String ELETA_START = "[";

    private int contextLength;
    private String expected;
    private String actual;
    private int prefixLength;
    private int suffixLength;

    public ComparisonCompactor(int contextLength, String expected, String actual) { ... }

    public String formatCompactedComparison(String message) { ... }
    private boolean shouldBeCompacted() { ... }
    private boolean shouldNotBeCompacted() { ... }
    private void findCommonPrefixAndSuffix() { ... }
    private char charFromEnd(String s, int i) { ... }
    private boolean suffixOverlapsPrefix() { ... }
    private void findCommonPrefix() { ... }
    private String compact(String s) { ... }
    private String startingEllipsis() { ... }
    private String startingContext() { ... }
    private String delta(String s) { ... }
    private String endingContext() { ... }
    private String endingEllipsis() {... }
}
```

- λͺ¨λμ μΌλ ¨μ λΆμ ν¨μμ μΌλ ¨μ μ‘°ν© ν¨μλ‘ λλλ€. μ μ²΄ ν¨μλ μμμ μΌλ‘ μ λ ¬νμΌλ―λ‘ κ° ν¨μκ° μ¬μ©λ μ§νμ μ μλλ€. λΆμ ν¨μκ° λ¨Όμ  λμ€κ³  μ‘°ν© ν¨μκ° κ·Έ λ€λ₯Ό μ΄μ΄μ λμ¨λ€.
- μ½λλ₯Ό λ¦¬ν©ν°λ§νλ κ³Όμ μμ μλ νλ λ³κ²½μ λλλ¦¬λ κ²½μ°κ° ννλ€. λ¦¬ν©ν°λ§μ μ½λκ° μ΄λ μμ€μ μ΄λ₯Ό λκΉμ§ μλ§μ μνμ°©μ€λ₯Ό λ°λ³΅νλ μμμ΄κΈ° λλ¬Έμ΄λ€. :point_right: μ€μ λ‘ μμμ μ λ¦¬νλ [μλ μ½λλ₯Ό μ΄λ»κ² κ°μ νλ©΄ μ’μκΉ?](#μλ-μ½λλ₯Ό-μ΄λ»κ²-κ°μ νλ©΄-μ’μκΉ) μμ κ°μ νλ μ½λκ° μ΅μ’ λ²μ μ λ°μλμ§ μμ λ΄μ©λ€λ μ‘΄μ¬νλ€.

## 02. κ²°λ‘ 

- μΈμμ κ°μ μ΄ λΆνμν λͺ¨λμ μλ€. μ½λλ₯Ό μ²μλ³΄λ€ μ‘°κΈ λ κΉ¨λνκ² λ§λλ μ±μμ μ°λ¦¬ λͺ¨λμκ² μλ€. :point_right: μλ¬΄λ¦¬ μ μ§μ¬μ§ λͺ¨λ(μ½λ)μ΄λΌκ³  νλλΌλ κ°μ ν  λΆλΆμ΄ μλ€λ©΄ κ°μ νλΌ!
