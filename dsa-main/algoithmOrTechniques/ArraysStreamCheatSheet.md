# 🚀 Java Arrays & Strings Streams - Complete Cheat Sheet

> A comprehensive guide to working with Arrays and Strings using Java Streams API

[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.oracle.com/java/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

## 📋 Table of Contents

- [🧩 Arrays.stream() - Full Cheat Sheet](#-arraysstream---full-cheat-sheet)
- [🔤 String Streams - Complete Guide](#-string-streams---complete-guide)
- [🔄 Conversions & Transformations](#-conversions--transformations)
- [⚡ Performance Tips](#-performance-tips)
- [🛠️ Real-World Examples](#️-real-world-examples)
- [📚 Additional Resources](#-additional-resources)

---

## 🧩 Arrays.stream() - Full Cheat Sheet

### ✅ A. **Create a Stream from an Array**

```java
int[] nums = {1, 2, 3, 4, 5};
IntStream stream = Arrays.stream(nums);

// With range (start inclusive, end exclusive)
IntStream rangeStream = Arrays.stream(nums, 1, 4); // [2, 3, 4]

// From varargs
IntStream varStream = IntStream.of(1, 2, 3, 4, 5);
```

### ✅ B. **Common Operations on Arrays using Stream**

#### 1. **Sum**
```java
int sum = Arrays.stream(nums).sum(); // 15
```

#### 2. **Max / Min**
```java
int max = Arrays.stream(nums).max().getAsInt(); // 5
int min = Arrays.stream(nums).min().getAsInt(); // 1

// With custom comparator for objects
String[] words = {"apple", "banana", "cherry"};
Optional<String> longest = Arrays.stream(words)
    .max(Comparator.comparing(String::length));
```

#### 3. **Average**
```java
OptionalDouble avg = Arrays.stream(nums).average();
if (avg.isPresent()) {
    System.out.println(avg.getAsDouble()); // 3.0
}

// Or with orElse
double average = Arrays.stream(nums).average().orElse(0.0);
```

#### 4. **Filter values**
```java
int[] evens = Arrays.stream(nums)
    .filter(n -> n % 2 == 0)
    .toArray(); // [2, 4]

// Multiple conditions
int[] filtered = Arrays.stream(nums)
    .filter(n -> n > 2 && n < 5)
    .toArray(); // [3, 4]
```

#### 5. **Map (modify each element)**
```java
int[] squared = Arrays.stream(nums)
    .map(n -> n * n)
    .toArray(); // [1, 4, 9, 16, 25]

// Transform to different type
String[] strings = Arrays.stream(nums)
    .mapToObj(String::valueOf)
    .toArray(String[]::new); // ["1", "2", "3", "4", "5"]
```

#### 6. **Count elements**
```java
long count = Arrays.stream(nums).count(); // 5

long evenCount = Arrays.stream(nums)
    .filter(n -> n % 2 == 0)
    .count(); // 2
```

#### 7. **Sort**
```java
int[] sorted = Arrays.stream(nums)
    .sorted()
    .toArray();

// Reverse order
int[] reversed = Arrays.stream(nums)
    .boxed()
    .sorted(Collections.reverseOrder())
    .mapToInt(Integer::intValue)
    .toArray();
```

#### 8. **Distinct**
```java
int[] distinct = Arrays.stream(new int[]{1, 2, 2, 3, 3})
    .distinct()
    .toArray(); // [1, 2, 3]
```

#### 9. **Collect to List (boxed)**
```java
List<Integer> list = Arrays.stream(nums)
    .boxed()
    .collect(Collectors.toList());

// To Set
Set<Integer> set = Arrays.stream(nums)
    .boxed()
    .collect(Collectors.toSet());
```

#### 10. **Any match / All match / None match**
```java
boolean hasEven = Arrays.stream(nums).anyMatch(n -> n % 2 == 0); // true
boolean allPositive = Arrays.stream(nums).allMatch(n -> n > 0); // true
boolean noneNegative = Arrays.stream(nums).noneMatch(n -> n < 0); // true
```

#### 11. **Find First / Find Any**
```java
OptionalInt first = Arrays.stream(nums).findFirst(); // OptionalInt[1]
OptionalInt any = Arrays.stream(nums).findAny(); // OptionalInt[1]

// Find first even number
OptionalInt firstEven = Arrays.stream(nums)
    .filter(n -> n % 2 == 0)
    .findFirst(); // OptionalInt[2]
```

#### 12. **Reduce**
```java
// Sum using reduce
int sum = Arrays.stream(nums).reduce(0, (a, b) -> a + b); // 15

// Product
int product = Arrays.stream(nums).reduce(1, (a, b) -> a * b); // 120

// Max using reduce
OptionalInt max = Arrays.stream(nums).reduce(Integer::max);
```

---

## 🔤 String Streams - Complete Guide

### ✅ A. `String.chars()` → IntStream of ASCII values

```java
String s = "abc";
IntStream stream = s.chars(); // [97, 98, 99]

// Print each character
s.chars().forEach(c -> System.out.println((char) c));
```

#### 👉 Example: Convert characters to uppercase
```java
String upper = s.chars()
    .mapToObj(c -> String.valueOf((char) c).toUpperCase())
    .collect(Collectors.joining()); // "ABC"
```

#### 👉 Example: Count vowels
```java
String text = "Hello World";
long vowelCount = text.toLowerCase().chars()
    .filter(c -> "aeiou".indexOf(c) >= 0)
    .count(); // 3
```

#### 👉 Example: Remove non-alphabetic characters
```java
String cleaned = text.chars()
    .filter(Character::isLetter)
    .mapToObj(c -> String.valueOf((char) c))
    .collect(Collectors.joining()); // "HelloWorld"
```

### ✅ B. `String.codePoints()` → Unicode-aware

```java
String emoji = "Hello 😊 World 🌍";
emoji.codePoints().forEach(cp -> {
    System.out.println(Character.toString(cp));
});

// Count actual characters (including emojis)
long charCount = emoji.codePoints().count(); // 15
```

### ✅ C. Split and Stream words or characters

```java
String sentence = "hello world java";

// Stream of words
Stream<String> words = Arrays.stream(sentence.split(" "));

// Stream of characters
Stream<String> characters = sentence.chars()
    .mapToObj(c -> String.valueOf((char) c));

// Stream of characters (alternative)
Stream<Character> chars = sentence.chars()
    .mapToObj(c -> (char) c);
```

#### 👉 Word Processing Examples

```java
String text = "The quick brown fox jumps over the lazy dog";

// Longest word
Optional<String> longest = Arrays.stream(text.split(" "))
    .max(Comparator.comparing(String::length)); // "quick", "brown", "jumps"

// Word frequency
Map<String, Long> wordFreq = Arrays.stream(text.toLowerCase().split(" "))
    .collect(Collectors.groupingBy(
        Function.identity(),
        Collectors.counting()
    )); // {the=2, quick=1, brown=1, ...}

// Words starting with specific letter
List<String> wordsStartingWithT = Arrays.stream(text.toLowerCase().split(" "))
    .filter(word -> word.startsWith("t"))
    .collect(Collectors.toList()); // [the, the]

// Average word length
double avgLength = Arrays.stream(text.split(" "))
    .mapToInt(String::length)
    .average()
    .orElse(0.0); // 3.4
```

#### 👉 Character Analysis Examples

```java
String password = "MyP@ssw0rd123";

// Check password complexity
boolean hasUpper = password.chars().anyMatch(Character::isUpperCase);
boolean hasLower = password.chars().anyMatch(Character::isLowerCase);
boolean hasDigit = password.chars().anyMatch(Character::isDigit);
boolean hasSpecial = password.chars()
    .anyMatch(c -> "!@#$%^&*()".indexOf(c) >= 0);

// Character frequency
Map<Character, Long> charFreq = password.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(
        Function.identity(),
        Collectors.counting()
    ));
```

---

## 🔄 Conversions & Transformations

| From | To | Code |
|------|-----|------|
| `int[]` | `List<Integer>` | `Arrays.stream(nums).boxed().collect(Collectors.toList())` |
| `List<Integer>` | `int[]` | `list.stream().mapToInt(i -> i).toArray()` |
| `String` | `char[]` | `str.toCharArray()` |
| `String` | `List<Character>` | `str.chars().mapToObj(c -> (char)c).collect(Collectors.toList())` |
| `String` | `int[]` (ASCII) | `str.chars().toArray()` |
| `char[]` | `String` | `new String(charArray)` |
| `List<String>` | `String` | `list.stream().collect(Collectors.joining())` |
| `String[]` | `List<String>` | `Arrays.stream(stringArray).collect(Collectors.toList())` |
| `Stream<String>` | `String` | `stream.collect(Collectors.joining(", "))` |

### Advanced Conversions

```java
// String to Set of unique characters
Set<Character> uniqueChars = "programming".chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.toSet());

// Array of strings to single string with custom delimiter
String[] words = {"Java", "is", "awesome"};
String sentence = Arrays.stream(words)
    .collect(Collectors.joining(" ")); // "Java is awesome"

// Numbers to formatted strings
int[] numbers = {1, 2, 3, 4, 5};
String formatted = Arrays.stream(numbers)
    .mapToObj(n -> String.format("Number: %02d", n))
    .collect(Collectors.joining(", "));
```

---

## ⚡ Performance Tips

### 🔥 Use Primitive Streams
```java
// ❌ Slow - boxing/unboxing overhead
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
int sum = list.stream()
    .mapToInt(Integer::intValue)
    .sum();

// ✅ Fast - primitive stream
int[] array = {1, 2, 3, 4, 5};
int sum = Arrays.stream(array).sum();
```

### 🔥 Filter Before Map
```java
// ❌ Less efficient
String[] words = {"apple", "banana", "cherry", "date"};
List<String> result = Arrays.stream(words)
    .map(String::toUpperCase)           // Transform all
    .filter(s -> s.length() > 5)        // Then filter
    .collect(Collectors.toList());

// ✅ More efficient
List<String> result = Arrays.stream(words)
    .filter(s -> s.length() > 5)        // Filter first
    .map(String::toUpperCase)           // Transform only needed
    .collect(Collectors.toList());
```

### 🔥 Use Parallel Streams for Large Data
```java
int[] largeArray = IntStream.range(0, 1_000_000).toArray();

// Sequential
long sum1 = Arrays.stream(largeArray).sum();

// Parallel (for CPU-intensive operations)
long sum2 = Arrays.stream(largeArray).parallel().sum();
```

---

## 🛠️ Real-World Examples

### 📊 Data Analysis
```java
// Sales data analysis
int[] sales = {1200, 1500, 900, 1800, 2100, 1300, 1600};

// Statistics
IntSummaryStatistics stats = Arrays.stream(sales).summaryStatistics();
System.out.println("Total: " + stats.getSum());
System.out.println("Average: " + stats.getAverage());
System.out.println("Max: " + stats.getMax());
System.out.println("Min: " + stats.getMin());

// Above average sales
double average = stats.getAverage();
int[] aboveAverage = Arrays.stream(sales)
    .filter(sale -> sale > average)
    .toArray();
```

### 🔍 Text Processing
```java
// Log file analysis
String logEntry = "2023-07-28 14:30:25 ERROR Database connection failed";

// Extract timestamp
String timestamp = Arrays.stream(logEntry.split(" "))
    .limit(2)
    .collect(Collectors.joining(" ")); // "2023-07-28 14:30:25"

// Extract log level
Optional<String> logLevel = Arrays.stream(logEntry.split(" "))
    .filter(word -> word.matches("(DEBUG|INFO|WARN|ERROR)"))
    .findFirst(); // Optional["ERROR"]

// Word count in message
long wordCount = Arrays.stream(logEntry.split(" "))
    .skip(3) // Skip timestamp and level
    .count(); // 3
```

### 📝 Form Validation
```java
// Email validation and processing
String[] emails = {
    "john@gmail.com",
    "invalid-email",
    "alice@yahoo.com",
    "bob@company.co.uk"
};

// Valid emails only
List<String> validEmails = Arrays.stream(emails)
    .filter(email -> email.contains("@") && email.contains("."))
    .collect(Collectors.toList());

// Group by domain
Map<String, List<String>> emailsByDomain = Arrays.stream(emails)
    .filter(email -> email.contains("@"))
    .collect(Collectors.groupingBy(
        email -> email.substring(email.indexOf("@") + 1)
    ));
```

### 🎯 Game Score Processing
```java
// Player scores
int[] scores = {85, 92, 78, 95, 88, 76, 91, 83};

// Top 3 scores
int[] top3 = Arrays.stream(scores)
    .boxed()
    .sorted(Collections.reverseOrder())
    .limit(3)
    .mapToInt(Integer::intValue)
    .toArray(); // [95, 92, 91]

// Grade distribution
Map<String, Long> grades = Arrays.stream(scores)
    .boxed()
    .collect(Collectors.groupingBy(
        score -> {
            if (score >= 90) return "A";
            else if (score >= 80) return "B";
            else if (score >= 70) return "C";
            else return "F";
        },
        Collectors.counting()
    )); // {A=3, B=4, C=1}
```

---

## ✅ Summary

### 🔥 `Arrays.stream()` supports:
- ✅ `sum`, `max`, `min`, `average`, `count`
- ✅ `filter`, `map`, `sorted`, `distinct`
- ✅ `anyMatch`, `allMatch`, `noneMatch`
- ✅ `findFirst`, `findAny`, `reduce`
- ✅ `boxed`, `collect`, `toArray`
- ✅ `limit`, `skip`, `parallel`

### 🔥 For `String`, use:
- ✅ `String.chars()` (for ASCII values)
- ✅ `String.codePoints()` (for Unicode-aware processing)
- ✅ `split() + Arrays.stream()` (for word processing)
- ✅ `mapToObj(c -> (char)c)` to work with characters

### 💡 Key Takeaways:
- Use `.boxed()` to convert from primitive streams to object streams
- Primitive streams (`IntStream`, `LongStream`, `DoubleStream`) are more efficient
- Chain operations for readable and maintainable code
- Use parallel streams only for large datasets and CPU-intensive operations
- Always handle `Optional` results from operations like `findFirst()`, `max()`, `min()`

---

## 📚 Additional Resources

- [Java 8 Streams Tutorial](https://www.oracle.com/java/technologies/javase/8-whats-new.html)
- [Stream API Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
- [Best Practices for Java Streams](https://www.baeldung.com/java-8-streams)

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

⭐ **Star this repository if you found it helpful!**