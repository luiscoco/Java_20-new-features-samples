# Java 20 new features samples

Java 20 language specification: https://docs.oracle.com/javase/specs/jls/se20/html/index.html

## ChatGPT4 prompt (https://chat.openai.com/)

For example: 

please provide me a simple java code sample about Java Record Patterns (JEP 432)

please explain me Java Record Patterns (JEP 432)

## Google Gemini prompt (https://gemini.google.com/app)

For example: 

please provide me a simple java code sample about Java Record Patterns (JEP 432)

please explain me Java Record Patterns (JEP 432)

## 1. Record Patterns (JEP 432)

https://openjdk.org/jeps/453

**Records Recap**

First, a quick review. Records in Java provide a concise way to define data-centric classes using this kind of syntax:

```java
record Person(String name, int age) {}
```

This implicitly creates fields name and age, along with accessors, constructors, toString(), equals(), and more

**What are Record Patterns?**

Record patterns allow us to deconstruct records. They primarily enhance instanceof and switch expressions for cleaner, more streamlined code when working with records

**Pattern Matching with instanceof**

Traditionally, after an **instanceof** check, you'd need to  cast:

```java
if (obj instanceof Person) {
    Person person = (Person) obj;
    String name = person.name(); 
    // ...
}
```

With record patterns, you can check the type and extract attributes simultaneously:

```java
if (obj instanceof Person(String name, int age)) {
   // 'name' and 'age' variables in scope here
}
```

**Record Patterns in switch**

Record patterns make switch cases more expressive with records:

```java
switch (shape) {
    case Circle(double radius) -> System.out.println("Circle with radius: " + radius);
    case Rectangle(double width, double height) -> System.out.println("Rectangle area: " + width * height);
    default -> System.out.println("Unknown shape"); 
}
```

**Advantages**

**Conciseness**: Removes boilerplate casting and extractions

**Readability**: Code focuses on the record structure and what matters

**Type Safety**: The compiler helps you work with record components within cases

**Nested Patterns**: Powerful for records containing other records - deconstruct deeply!

**When to Use Them**

Record patterns shine particularly when:

- Working with nested records

- Performing logic based on varying record types

- You value explicitness and readability in your code

**Important note**:  For simple access to record components, the built-in accessors (e.g., person.name()) are often enough. Record patterns truly come into play with conditional logic and more complex object handling

## 2. Pattern Matching for switch (JEP 433)

https://openjdk.org/jeps/453



## 3. Scoped Values (JEP 429)

https://openjdk.org/jeps/453



## 4. Foreign Function & Memory API (JEP 434)

https://openjdk.org/jeps/453



## 5. Virtual Threads (JEP 436)

https://openjdk.org/jeps/453



## 6. Structured Concurrency (JEP 437)

https://openjdk.org/jeps/453



## 7. Vector API 5 (JEP 438)

https://openjdk.org/jeps/453
