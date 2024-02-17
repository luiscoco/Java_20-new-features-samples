# Java 20 new features samples

Java 20 language specification: https://docs.oracle.com/javase/specs/jls/se20/html/index.html

## ChatGPT4 prompt (https://chat.openai.com/)

For example: 

please provide me a simple java code sample about Java Record Patterns (JEP 432)

please explain me Java Record Patterns (JEP 432)

please provide me more advance sample about Java Record Patterns (JEP 432)

## Google Gemini prompt (https://gemini.google.com/app)

For example: 

please provide me a simple java code sample about Java Record Patterns (JEP 432)

please explain me Java Record Patterns (JEP 432)

please provide me more advance sample about Java Record Patterns (JEP 432)

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

Let's delve into a slightly **more advanced application of Java Record Patterns**

Imagine a scenario where you receive geometric data in a text format.  We'll leverage record patterns to elegantly parse and handle different shapes.

```java
record Shape(String type) {}
record Circle(String type, double radius) extends Shape {}
record Rectangle(String type, double width, double height) extends Shape {}
record Triangle(String type, double base, double height) extends Shape {}

public class ShapeParser {

    public static Shape parseShape(String input) {
        switch (input.split(":")[ [0]) {  // Simple "Type:Data" parsing
            case "Circle" -> {
                double radius = Double.parseDouble(input.split(":")[1]);
                return new Circle("Circle", radius);
            }
            case "Rectangle" -> {
                String[] dimensions = input.split(":")[1].split(",");
                double width = Double.parseDouble(dimensions[0]);
                double height = Double.parseDouble(dimensions[1]);
                return new Rectangle("Rectangle", width, height);
            }
            case "Triangle" -> {
                String[] dimensions = input.split(":")[1].split(",");
                double base = Double.parseDouble(dimensions[0]);
                double height = Double.parseDouble(dimensions[1]);
                return new Triangle("Triangle", base, height);
            }
            default -> throw new IllegalArgumentException("Unknown shape type: " + input);
        }
    }

    public static void main(String[] args) {
        String inputData = "Circle:5.2\nRectangle:3,6\nTriangle:4,2";

        for (String line : inputData.split("\n")) {
            Shape shape = parseShape(line);
            processShape(shape);
        }
    }

    static void processShape(Shape shape) {
        if (shape instanceof Circle(double radius)) {
            System.out.println("Circle with radius: " + radius);
        } else if (shape instanceof Rectangle(double width, double height)) {
            System.out.println("Rectangle area: " + (width * height));
        } else if (shape instanceof Triangle(double base, double height)) {
            System.out.println("Triangle area: " + (0.5 * base * height));
        }
    }
}
```

## 2. Pattern Matching for switch (JEP 433)

https://openjdk.org/jeps/453

Let's break down Java **Pattern Matching for switch** introduced in JEP 433 (preview), with further refinements in subsequent JEPs.

**The Traditional switch**

Before, Java's switch focused mainly on primitive types and enums:

```java
int dayOfWeek = 3;
switch (dayOfWeek) {
    case 1: 
        System.out.println("Monday"); 
        break;
    // ... other cases 
    default: 
        System.out.println("Unknown day");
}
```

**This had limitations**:

Type Checks and Casting: Often you'd have to use instanceof and cast within each case

Repetitive Structure: Boilerplate code to access values after type casting

Pattern Matching for switch Supercharges It

Let's see how it evolves with pattern matching:

```java
record Point(int x, int y) {}

String describe(Object obj) {
    return switch (obj) {
        case Point(int x, int y) -> "Point at coordinates: (" + x + ", " + y + ")";  
        case null -> "It's null";
        default -> "Unknown object type";
    };
}
```

**Key Enhancements**

Type Patterns: We can directly check if  obj is a Point using case Point(... ).

Deconstruction: If  obj is a Point, it simultaneously extracts x and y. No separate casting needed!

Guarded Patterns:

```java
 case Integer i && i > 0 -> "Positive integer"; // Additional conditions
```

Conciseness: Code within cases becomes less verbose, focusing on what you want to do with the  data, not just extracting it.

**Types of Patterns**

Type Patterns (As seen above)

Null Pattern: A dedicated case null

Guarded Patterns: Add constraints with && or || after a type pattern

Parenthesized Patterns: Mostly for clarity when combining other patterns

**Beyond Basics**

Exhaustiveness: Compiler might help you ensure your switch covers all possible cases of a variable's type

Scope of Variables: Variables introduced in patterns are conveniently scoped to that case block

**When It Shines**

Working with data-centric classes: Especially records, when the structure of your data matters

Hierarchical Data: Patterns can 'reach into' nested structures

Replacing chains of if..else if: Especially if these involve type checks

**Note**: JEP 433 started by enabling this only for instanceof type checks 

Newer Java versions broaden this for String objects and potentially expand the possibilities even further

Look for another code examples showing:

- Type checks with traditional switch vs. pattern matching switch

- Handling nested data structures

- Advanced guarded patterns

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
