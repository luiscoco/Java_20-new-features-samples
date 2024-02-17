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

Let's craft a  **more advanced example** about Java Pattern Matching for switch

**Scenario: Geometric Calculations**

Consider a scenario where you're working with shapes, and you need to calculate properties based on their type

```java
// Our Records
record Shape(String type) {}
record Circle(String type, double radius) extends Shape {}
record Rectangle(String type, double width, double height) extends Shape {}
record Triangle(String type, double base, double height) extends Shape {}

public class ShapeCalculations {

    public static String calculate(Shape shape) {
        return switch (shape) {
            case Circle(double radius) -> 
                 "Circle with area: " + (Math.PI * radius * radius);

            case Rectangle(double width, double height) -> 
                 "Rectangle with area: " + (width * height);

            case Triangle(double base, double height) -> 
                 "Triangle with area: " + (0.5 * base * height);

            case Shape() -> "Unknown or generic shape"; // Catch-all case
        };
    }

    public static static main(String[] args) {
        Shape shape1 = new Circle("Circle", 2.5);
        Shape shape2 = new Rectangle("Rectangle", 4, 3);

        System.out.println(calculate(shape1)); 
        System.out.println(calculate(shape2)); 
    }
}
```

**Breakdown**

Shapes as Records: Our shapes now leverage records, making them natural fits for pattern matching

Calculations within switch: Each case directly extracts attributes and performs the necessary computation

Clarity: Our code directly conveys the intent of what we're doing for each shape type

Handling 'Unknown' Shape: We gracefully deal with cases where the Shape instance isn't specifically a circle, rectangle, or triangle

**Enhancements & Notes**

Sealed Hierarchies: If Shape were a sealed class or interface, the compiler could guarantee we've covered every possible subtype, providing extra safety

Complex Conditions: Pattern matching could incorporate guarded patterns (case Shape s && s.area() > 10 - hypothetical area() method)

Alternatives: While pattern-matching switch shines here, a polymorphic solution ('calculateArea() method on each shape) could be valid in some object-oriented designs

**Even More Advanced?**

We could imagine:

Shapes containing other shapes: Nested pattern matching to deconstruct deeply

Patterns on Strings: Newer Java releases allow this. Imagine pattern matching on an input command in a program

"When Clauses" (Not in Java yet): Other languages extend 'switch' further with conditions directly attached to patterns

## 3. Scoped Values (JEP 429)

https://openjdk.org/jeps/429

**What are Scoped Values?**

Scoped values introduce a new mechanism in Java to share data safely and efficiently within the boundaries of a dynamic scope

Think of a dynamic scope as the active execution path of a task or thread, including any sub-tasks or sub-threads it might create

A scoped value acts like a container that holds a value. Unlike passing data through method parameters, scoped values let you make your data implicitly available down the call chain within this dynamic scope

**Why Scoped Values?**

Cleaner Data Sharing:

Traditional thread-local variables work, but they can make code cumbersome, especially when you have deeply nested method calls

Scoped values are designed to share data directly across this execution path, making your code easier to read and reason about

**Performance (Especially with Virtual Threads)**:

Java's Project Loom aims to introduce lightweight virtual threads

Thread-local variables get tricky in a world with massive numbers of threads

Scoped values are well-suited for efficient data sharing in these scenarios

**Key Concepts**

**ScopedValue Class**: You represent a scoped value using the ScopedValue class

**Binding**: You associate a value to a scoped value using the bind() method

**Scope**: A scope defines the span where a scoped value and its binding are visible – meaning where the value can be accessed. Scopes can be nested

**Immutability**: Values bound to a scoped value should be immutable to ensure thread safety

It's a way to protect against unintended data changes in other parts of the execution path

**Simple Example**

```java
public class ScopedExample {
    static final ScopedValue<String> message = new ScopedValue<>();

    public static void main(String[] args) {
        try (ScopedValue.Handle scope = message.bind("Hello from Scoped Values!")) {
            new Thread(() -> System.out.println(message.get())).start(); 
        }
    }
}
```

**Explanation**:

We create a static ScopedValue to hold a String

In main, we bind the message "Hello from Scoped Values!"

The try-with-resources block establishes a scope

We start a new thread, which can still access the value "Hello from Scoped Values!" because it's running within the scope created in the main method

**Remember**

Scoped Values were introduced as an Incubator feature in JDK 19 and re-previewed in JDK 20, 21, and 22

They are still under development and their API might change slightly in the future

Use scoped values when data needs to be shared across methods in a task or execution flow, without direct parameter passing

## 4. Foreign Function & Memory API (JEP 434)

https://openjdk.org/jeps/434

The **Java Native Interface (JNI)** has been the traditional way to work with code written in languages like C or C++ from within Java. However, JNI has several drawbacks:

**Complexity**: Writing JNI code is verbose and error-prone. You need to deal with intricate mappings between Java types and native types

**Safety**: JNI is prone to memory management errors that can lead to crashes and instability within your Java application

**Performance**: The overhead of switching back and forth between the Java and native worlds can degrade performance

**What is the Foreign Function & Memory API?**

The Foreign Function & Memory API (FFM API) introduced a new, safer, simpler, and more performant way for Java programs to interact with:

**Foreign Functions (code outside the JVM)**: This means calling functions in native libraries (typically written in C/C++ ) directly from your Java code

**Foreign Memory (memory not managed by the JVM)**: This enables Java code to access and manipulate memory located outside the standard Java heap

**Key Benefits**

**Ease of Use**: The FFM API provides a much more streamlined and type-safe interface compared to JNI. Mapping data between Java and native types gets much easier

**Safety**: The API has better memory management controls, significantly reducing the risk of errors that plagued JNI

**Performance**: Avoiding the overhead of JNI calls enhances performance for native code integration

**Flexibility**: You can work with various native libraries, not just ones explicitly designed for JNI

**How It Works (Simplified)**

**Memory Segments**: The API introduces the concept of MemorySegment to represent regions of memory (either on-heap, off-heap, or native memory)

**Memory Layouts**: These are descriptions of how data is structured in memory (e.g., the arrangement of fields in a struct in C)

**Method Handles**: These allow you to invoke native functions as if they were regular Java methods

**Example (Calling a C function)**

```java
import jdk.incubator.foreign.*;

public class FFMExample {
    public static void main(String[] args) {
        try (Linker linker = Linker.nativeLinker()) {
            SymbolLookup lookup = linker.defaultLookup();
            MethodHandle absFunc = lookup.find("abs").get();
            int result = (int) absFunc.invokeExact(-10); 
            System.out.println(result);  // Output: 10
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }
}
```

**In this example**:

We obtain a Linker to work with native code

We look up the native abs function (from the standard C library)

MethodHandle lets us invoke abs like a Java method

**Important Notes**

The FFM API was first released as an incubator feature in JDK 19, and continues to evolve in new Java versions

Since it's an Incubator feature, you'll likely need to enable it with command-line arguments when working with a Java version supported before it becomes standardized

## 5. Virtual Threads (JEP 436)

https://openjdk.org/jeps/436



## 6. Structured Concurrency (JEP 437)

https://openjdk.org/jeps/437



## 7. Vector API 5 (JEP 438)

https://openjdk.org/jeps/438


