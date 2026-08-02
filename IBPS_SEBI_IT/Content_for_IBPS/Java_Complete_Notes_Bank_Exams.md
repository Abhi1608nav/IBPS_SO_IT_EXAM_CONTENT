# Java Complete Notes — For Bank IT Officer / Computer Knowledge Exams

*(Structured to mirror your C++ and Python notes so all three can be compared topic-by-topic)*

---

## 1. Java — Basic Nature of the Language

| Property | Java |
|---|---|
| Type of language | **Compiled + Interpreted** (compiles to bytecode, then run by JVM — "platform-independent") |
| Typing | **Statically typed** (like C++) — types fixed at compile time |
| Case sensitivity | Yes |
| Indentation | Not mandatory (uses `{}` like C++), but good practice |
| Semicolons | **Required**, like C++ |
| Extension | `.java` (source), `.class` (compiled bytecode) |
| Slogan | **"Write Once, Run Anywhere" (WORA)** — because it runs on the JVM, not directly on hardware |
| Pure OOP? | Almost — everything is inside a class, except primitive types |

---

## 2. JDK, JRE, JVM — Very Frequently Asked

| Term | Full form | Role |
|---|---|---|
| **JVM** | Java Virtual Machine | The engine that actually **runs** the bytecode (`.class` files); provides platform independence |
| **JRE** | Java Runtime Environment | JVM + core libraries — needed to **run** a Java program (no compiler) |
| **JDK** | Java Development Kit | JRE + compiler (`javac`) + development tools — needed to **write and compile** Java programs |

**Relationship:** JDK ⊃ JRE ⊃ JVM (JDK contains JRE, JRE contains JVM)

```
.java file → [javac compiler] → .class file (bytecode) → [JVM] → machine code → output
```

---

## 3. Built-in Functions & Packages (equivalent to C++ headers / Python modules)

Java organizes built-in code into **packages**, imported using `import`.

| Package | Purpose | Examples |
|---|---|---|
| `java.lang` | Core classes — **auto-imported**, no `import` needed | `String`, `Math`, `System`, `Integer`, `Object` |
| `java.util` | Utility classes, collections | `ArrayList`, `HashMap`, `Scanner`, `Random` |
| `java.io` | Input/Output | `BufferedReader`, `FileReader`, `PrintWriter` |
| `java.net` | Networking | `Socket`, `URL` |
| `java.sql` | Database connectivity (JDBC) | `Connection`, `ResultSet` |
| `java.time` | Date/time (modern) | `LocalDate`, `LocalDateTime` |

### Commonly used built-in methods
```java
Math.sqrt(16);        // 4.0
Math.pow(2,3);          // 8.0
Math.abs(-5);            // 5
String.valueOf(100);      // "100"
Integer.parseInt("25");    // 25
System.out.println("Hi");   // print with newline
System.out.print("Hi");      // print without newline
```

---

## 4. Streams / Input-Output (compare with C++ Section 2)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);   // input stream from keyboard
        System.out.print("Enter name: ");
        String name = sc.nextLine();
        System.out.println("Hello " + name);    // output stream to screen
    }
}
```

| Object | Purpose |
|---|---|
| `System.out` | Standard output stream |
| `System.in` | Standard input stream (raw — usually wrapped by `Scanner` or `BufferedReader`) |
| `System.err` | Standard error stream |
| `Scanner` | Easiest way to read user input (`nextInt()`, `nextLine()`, `nextDouble()`) |
| `BufferedReader` | Faster for reading large input, needs `try/catch` for `IOException` |

### File I/O
```java
FileWriter fw = new FileWriter("data.txt");
fw.write("Bank Exam");
fw.close();

FileReader fr = new FileReader("data.txt");
BufferedReader br = new BufferedReader(fr);
System.out.println(br.readLine());
```

---

## 5. Data Types

### Primitive types (8 total — stored directly, not objects)
| Type | Size | Default value |
|---|---|---|
| `byte` | 1 byte | 0 |
| `short` | 2 bytes | 0 |
| `int` | 4 bytes | 0 |
| `long` | 8 bytes | 0L |
| `float` | 4 bytes | 0.0f |
| `double` | 8 bytes | 0.0d |
| `char` | 2 bytes (Unicode) | `'\u0000'` |
| `boolean` | 1 bit (JVM-dependent) | false |

### Non-primitive / Reference types
`String`, arrays, classes, interfaces — these store a **reference (address)** to the object, not the value itself.

**Exam trap — default values differ from C++:**
- In C++, a local uninitialized variable holds a **garbage value**.
- In Java, **instance/class-level variables are auto-initialized** to the defaults above (0, false, null) — Java does **not** allow garbage values here.
- **BUT local variables inside a method are NOT auto-initialized** in Java either — the compiler forces you to assign a value before use, or it's a **compile-time error** ("variable might not have been initialized"). This is a key Java-specific safety feature not present in C++.

---

## 6. Static vs Dynamic Typing / Static Keyword

- **Java is statically typed**, same category as C++ (type fixed at compile time), unlike Python.
- Separately, Java has a **`static` keyword** (different concept, don't confuse in exams):
  - `static` variable — shared across **all objects** of a class (like a C++ class-level/static member); belongs to the class, not any one instance.
  - `static` method — can be called **without creating an object** (e.g., `Math.sqrt()`, or `main()` itself).
  - `static` block — runs once when the class is loaded, before `main()`.
  ```java
  class Counter {
      static int count = 0;     // shared by all objects
      Counter() { count++; }
  }
  ```

---

## 7. Type Casting

| Type | Direction | Example |
|---|---|---|
| **Widening (Implicit)** | Smaller type → Larger type, automatic, safe | `int a = 10; double b = a;` |
| **Narrowing (Explicit)** | Larger type → Smaller type, manual, may lose data | `double d = 9.7; int x = (int) d; // x = 9` |

```java
int i = 100;
double d = i;          // widening — automatic
double pi = 3.14;
int rounded = (int) pi; // narrowing — needs explicit cast, becomes 3 (truncates, doesn't round)
```

---

## 8. Strings in Java — Immutability, Slicing-equivalent, Substrings

### String immutability
**`String` in Java is immutable**, exactly like Python's `str` — once created, its content cannot change; any "modification" creates a new object.
```java
String s = "Bank";
s.concat("PO");
System.out.println(s);     // still "Bank" — concat() returned a NEW string that was never stored
s = s.concat("PO");         // now s correctly points to the new object "BankPO"
```

### Why immutable? (common exam question)
- **Security** (used in class loading, network connections)
- **String pool/caching** — identical string literals can share the same memory
- **Thread-safety** — immutable objects are automatically safe across threads
- **Hashcode caching** — makes `String` efficient as `HashMap` keys

### Mutable string alternatives
Java doesn't have Python-style easy slicing, but offers **`StringBuilder`** / **`StringBuffer`** for mutable string operations:
```java
StringBuilder sb = new StringBuilder("Bank");
sb.append("PO");           // modifies IN PLACE — same object
sb.insert(0, "SBI ");
sb.reverse();
System.out.println(sb);
```
| `StringBuilder` | `StringBuffer` |
|---|---|
| Mutable, **not thread-safe**, faster | Mutable, **thread-safe** (synchronized methods), slower |

### "Slicing" equivalent — `substring()`
Java has no `[start:stop:step]` slicing like Python; the closest tool is `substring()`:
```java
String s = "COMPETITIVE";

s.substring(0, 5);      // "COMPE"   → from index 0 to 4 (end index EXCLUSIVE, like Python's stop)
s.substring(4);          // "ETITIVE" → from index 4 to end
s.charAt(0);               // 'C'      → single character at index
s.length();                 // 11
s.indexOf("PET");            // 3       → like Python's find()
s.contains("Bank");           // substring membership check
s.replace("I", "1");           // "COMPET1T1VE"
s.split(" ");                    // splits into a String array
s.toUpperCase(); s.toLowerCase();
s.trim();                          // removes leading/trailing whitespace
new StringBuilder(s).reverse().toString();  // reversing (no direct s[::-1] equivalent)
```

**Comparing strings — exam favorite:**
```java
String a = "Bank";
String b = "Bank";
String c = new String("Bank");

a == b          // true  → both refer to same object in the String pool
a == c          // false → c is a NEW object in heap memory, different reference
a.equals(c)     // true  → compares VALUE, not reference — always use equals() for content comparison
```

---

## 9. Decision-Making Statements

### if / else if / else — same as C++
```java
if (marks >= 90) { }
else if (marks >= 75) { }
else { }
```

### switch statement
```java
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Invalid");
}
```
- Takes an expression of type `byte`, `short`, `char`, `int`, enum, or **`String`** (Java allows String in switch, unlike C++).
- Without `break`, execution **falls through**, same trap as C++.
- Java 14+ introduced modern **switch expressions**: `String result = switch(day) { case 1 -> "Mon"; default -> "Invalid"; };`

### Ternary operator
```java
String result = (marks >= 40) ? "Pass" : "Fail";
```

---

## 10. Loops

| Loop | Structure | Notes |
|---|---|---|
| **for** | `for(init; condition; update)` | Same as C++ |
| **while** | `while(condition) { }` | Same as C++ |
| **do-while** | `do { } while(condition);` | ✅ Exists (unlike Python) — runs at least once |
| **enhanced for-each** | `for(int x : arr) { }` | Java's version of Python's `for item in list` |

---

## 11. Jump Statements

| Statement | Effect |
|---|---|
| `break` | Exits loop/switch |
| `continue` | Skips to next iteration |
| `return` | Exits method, optionally with value |
| **`goto`** | ❌ **Reserved as a keyword but NOT implemented/usable** — Java deliberately excludes it (unlike C++ which allows it) |
| **Labeled break/continue** | Java-specific — `break outerLabel;` can break out of a specific nested loop, filling the gap left by no `goto` |

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) continue outer;    // skips to next i, not just next j
        System.out.println(i + "," + j);
    }
}
```

---

## 12. Methods (Java calls functions "methods" since everything lives inside a class)

```java
returnType methodName(parameters) {
    // body
    return value;
}
```

### Types of methods
1. **Predefined/library methods** — `Math.sqrt()`, `.length()`, etc.
2. **User-defined methods**
3. **Static methods** — belong to the class, called via `ClassName.method()`
4. **Instance methods** — belong to an object, called via `object.method()`
5. **Method overloading** — same name, different parameter list (compile-time polymorphism) — ✅ supported natively, unlike Python
   ```java
   int add(int a, int b) { return a+b; }
   double add(double a, double b) { return a+b; }
   ```
6. **Method overriding** — subclass redefines a parent's method (runtime polymorphism), needs `@Override` (recommended, not mandatory)
7. **Recursive methods** — method calling itself
8. **Constructors** — special "method" for object initialization (see Section 14)
9. **Abstract methods** — declared without a body, must be implemented by subclass (Section 14)
10. **Final methods** — `final` keyword prevents a method from being overridden

### Pass by Value vs Pass by Reference — Java-specific nuance
**Java is ALWAYS strictly pass-by-value** — this is a very common exam trap because it *looks* like pass-by-reference for objects:
- For **primitives** (`int`, `char`...) — a copy of the actual value is passed; changes inside the method don't affect the original.
- For **objects** (arrays, custom classes) — a copy of the **reference (address)** is passed, not the object itself. So the method **can modify the object's internal state** (because both copies point to the same object) but **cannot make the original reference point to a new object**.

```java
void changeValue(int x) { x = 100; }              // original int unaffected
void changeArray(int[] arr) { arr[0] = 100; }       // original array IS affected (same object)
void reassignArray(int[] arr) { arr = new int[]{9,9}; } // original reference is NOT affected
```
This is functionally identical to Python's "pass by object reference" behavior (Section 11 of the Python notes) — Java just labels the whole mechanism "pass by value" because *the reference itself* is copied.

---

## 13. Exception Handling — Core Topic

### Types of errors
| Type | When | Example |
|---|---|---|
| **Compile-time error** | Caught by `javac` before running — syntax errors, type mismatches, missing semicolons | `int x = "5";` |
| **Runtime error / Exception** | Occurs while the program is executing | `ArithmeticException` (divide by zero) |
| **Logical error** | Program runs, gives wrong output, no error shown | Wrong formula used |

### Checked vs Unchecked exceptions — Java-specific, no direct C++/Python equivalent
| Checked Exceptions | Unchecked Exceptions |
|---|---|
| Checked by the **compiler at compile time** — you MUST handle them (`try-catch` or `throws`) or the code won't compile | Occur at **runtime**, compiler doesn't force handling |
| Examples: `IOException`, `SQLException`, `ClassNotFoundException` | Examples: `ArithmeticException`, `NullPointerException`, `ArrayIndexOutOfBoundsException`, `NumberFormatException` |
| Extend `Exception` (but not `RuntimeException`) | Extend `RuntimeException` |

### Class hierarchy (exam favorite diagram)
```
Throwable
 ├── Error (serious, unrecoverable — e.g., OutOfMemoryError, StackOverflowError)
 └── Exception
      ├── Checked exceptions (IOException, SQLException...)
      └── RuntimeException (Unchecked: ArithmeticException, NullPointerException...)
```

### try / catch / finally
```java
try {
    int a = 10, b = 0;
    System.out.println(a / b);
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero: " + e.getMessage());
} catch (Exception e) {                  // generic catch-all, must come LAST
    System.out.println("Something went wrong");
} finally {
    System.out.println("This always runs");  // cleanup — like Python's finally
}
```
- Java has **no `else` clause** for try-catch (unlike Python's `try/except/else/finally`).
- Can also use **try-with-resources** to auto-close files/connections:
  ```java
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
      System.out.println(br.readLine());
  } catch (IOException e) { e.printStackTrace(); }
  ```

### throw vs throws
- **`throw`** — used **inside** a method to actually raise an exception: `throw new ArithmeticException("Invalid");`
- **`throws`** — used in a method **signature** to declare that it might throw a checked exception, passing responsibility to the caller:
  ```java
  void readFile() throws IOException { ... }
  ```

### Custom exceptions
```java
class InsufficientBalanceException extends Exception {
    InsufficientBalanceException(String msg) { super(msg); }
}

void withdraw(double balance, double amount) throws InsufficientBalanceException {
    if (amount > balance) throw new InsufficientBalanceException("Balance too low");
}
```

---

## 14. Classes, Objects & OOP (compare with C++ Section 12, Python Section 14)

```java
class Account {
    private double balance;             // encapsulation via access modifier

    Account(double balance) {            // constructor — same name as class, no return type
        this.balance = balance;           // 'this' = current object (like C++ 'this', Python 'self')
    }

    void deposit(double amt) {
        balance += amt;
    }

    double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        Account acc = new Account(1000);   // object creation — ALWAYS via 'new' in Java
        acc.deposit(500);
    }
}
```

### Access modifiers — stricter than both C++ and Python
| Modifier | Same class | Same package | Subclass (different package) | Everywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *default* (no modifier) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

Unlike Python (where access control is just naming convention), **Java strictly enforces these at compile time** — same philosophy as C++.

### Constructors
- **Default constructor** — auto-provided by Java if you write none.
- **Parameterized constructor** — takes arguments.
- **Copy constructor** — Java has **no built-in copy constructor like C++**; you write one manually or use `.clone()`.
- **No destructor in Java** — garbage collector handles cleanup automatically; the closest thing is `finalize()` (deprecated/unreliable, rarely used now).

### The Four OOP Pillars
1. **Encapsulation** — private fields + public getters/setters.
2. **Abstraction** — via `abstract class` or `interface`.
3. **Inheritance** — `class Manager extends Employee { }` (Java supports only **single inheritance** for classes — no multiple class inheritance like C++, to avoid the diamond problem).
4. **Polymorphism**
   - **Compile-time** — method overloading.
   - **Runtime** — method overriding (`@Override`), enabled via **dynamic method dispatch**.

### Interface vs Abstract class — very frequently asked
| Abstract Class | Interface |
|---|---|
| `abstract class Shape { abstract void draw(); }` | `interface Shape { void draw(); }` |
| Can have both abstract AND concrete (implemented) methods | Traditionally only abstract methods (Java 8+ allows `default`/`static` methods too) |
| Can have constructors, instance variables | Cannot have constructors; fields are implicitly `public static final` |
| A class can extend only **ONE** abstract class | A class can implement **MULTIPLE** interfaces — Java's way of achieving multiple-inheritance-like behavior |
| `extends` keyword | `implements` keyword |

```java
interface Payable { void pay(); }
interface Loggable { void log(); }
class Employee implements Payable, Loggable {   // multiple interfaces = Java's "multiple inheritance"
    public void pay() { }
    public void log() { }
}
```

### `final` keyword — Java's equivalent of C++ `const`/`final`
- `final` variable → value can't be reassigned (constant)
- `final` method → can't be overridden
- `final` class → can't be extended/inherited (e.g., `String`, `Integer` are `final` classes)

### No friend function equivalent
Java has **no `friend` keyword** (unlike C++) — there's no way to grant an outside function private access. Encapsulation is stricter by design; you must use public getters or place classes in the same package with default access.

---

## 15. Memory Allocation & Garbage Collection (compare with C++ Section 3, Python Section 12)

| Concept | Java |
|---|---|
| Memory management | **Automatic** — Java's **Garbage Collector (GC)** — no `delete`/`free` needed |
| `new` keyword | Always used to create objects on the **heap**; Java has no `malloc`/`calloc` — that distinction doesn't exist here |
| Stack | Stores local variables, method calls |
| Heap | Stores all objects (created via `new`) |
| Default values on heap | Instance variables (objects) default to `0`/`false`/`null` automatically — **no garbage value possible**, unlike C++'s `new int` |
| Garbage collection trigger | Automatic, when an object has no more references pointing to it; JVM decides *when* (`System.gc()` can only *request* it, not guarantee it) |
| Memory leak in Java? | Still possible (e.g., objects held in a never-cleared collection) but far rarer than C++ |
| `null` | Represents "no object" — similar to Python's `None`, C++'s `nullptr` |

**Key exam distinction from C++:** In C++, you must manually manage heap memory and can get garbage/uninitialized values. In Java, the JVM guarantees every object's fields start at a defined default value, and garbage collection prevents most memory leaks automatically.

---

## 16. Locks / Multithreading (compare with C++ Section 13B, Python Section 16)

Java has **native, built-in multithreading support** (via `Thread` class / `Runnable` interface) — stronger built-in support than both C++ (`<thread>`, added later) and Python (which is limited by the GIL).

```java
class MyThread extends Thread {
    public void run() { System.out.println("Thread running"); }
}
MyThread t = new MyThread();
t.start();     // starts a new thread (never call run() directly for this purpose)
```

### `synchronized` keyword — Java's built-in locking mechanism
```java
class Counter {
    private int count = 0;
    synchronized void increment() {    // only ONE thread can execute this method at a time
        count++;
    }
}
```
- `synchronized` method/block — automatically acquires a lock on the object before running, releases it after — Java's version of C++'s `mutex.lock()`/`unlock()`, but built into the language itself.
- **`volatile` keyword** — ensures a variable's value is always read from main memory, not a thread's cached copy — prevents visibility issues across threads.
- Java has **no GIL** (unlike Python) — genuinely runs multiple threads in parallel on multi-core CPUs.
- `wait()`, `notify()`, `notifyAll()` — inter-thread communication methods (from `Object` class).
- **Deadlock** — same concept as in C++/general OS theory — two threads waiting on each other's locks forever.

---

## 17. Arrays & Collections Framework

```java
int[] arr = new int[5];              // fixed-size array, default values = 0
int[] arr2 = {1, 2, 3, 4, 5};          // array literal

import java.util.ArrayList;
ArrayList<Integer> list = new ArrayList<>();   // dynamic-size, like C++'s vector / Python's list
list.add(10);
list.get(0);
list.remove(0);
```

| Collection | Similar to | Ordered? | Duplicates? |
|---|---|---|---|
| `ArrayList` | C++ `vector`, Python `list` | Yes | Yes |
| `LinkedList` | Doubly linked list | Yes | Yes |
| `HashMap` | Python `dict` | No | Keys: No |
| `HashSet` | Python `set` | No | No |
| `TreeMap`/`TreeSet` | Sorted map/set | Yes (sorted) | Depends |

### Generics — Java's type-safety tool (loosely similar to C++ templates)
```java
class Box<T> {
    T value;
    void set(T v) { value = v; }
    T get() { return value; }
}
Box<Integer> b = new Box<>();   // works for any type, type-checked at compile time
```

---

## 18. Execution of a Java Program — Layers (compare with C++ Section 14, Python Section 15)

```
Source Code (.java)
        ↓
 [1] JAVAC COMPILER  → checks syntax & types → translates to BYTECODE (.class file)
        ↓                (if errors here → compile-time error, nothing runs, same idea as C++)
 [2] CLASS LOADER     → JVM loads the .class file(s) into memory
        ↓
 [3] BYTECODE VERIFIER → checks bytecode for security/correctness before running
        ↓
 [4] JVM (Java Virtual Machine) → interprets bytecode OR uses JIT to compile hot code to native machine code
        ↓        (JIT = Just-In-Time compiler, makes repeated code run near-C++ speed)
 [5] EXECUTION on the actual OS/hardware
```

- This is why Java is "Write Once, Run Anywhere" — the `.class` bytecode is identical on any machine; only the JVM underneath differs per OS.
- Contrast: **C++** compiles directly to OS-specific native machine code (Section 14 of C++ notes) — a `.exe` built on Windows won't run on Linux. **Python** compiles to its own bytecode too, but Java additionally has the **JIT compiler**, giving it a real speed advantage over plain interpretation.

---

## 19. Quick-Fire Three-Way Comparison: C++ vs Java vs Python

| Concept | C++ | Java | Python |
|---|---|---|---|
| Typing | Static | Static | Dynamic |
| Execution | Compiled to native machine code | Compiled to bytecode → JVM (+JIT) | Compiled to bytecode → PVM (interpreted) |
| Memory management | Manual (`new`/`delete`) | Automatic (Garbage Collector) | Automatic (Garbage Collector) |
| Multiple inheritance (classes) | ✅ Yes | ❌ No (only via interfaces) | ✅ Yes |
| Pointers | Explicit, full control | No explicit pointers (references only) | No explicit pointers |
| Operator overloading | ✅ Yes | ❌ Not supported | ✅ Yes (via dunder methods) |
| `goto` | ✅ Exists | Reserved but unusable | ❌ Doesn't exist |
| do-while loop | ✅ Yes | ✅ Yes | ❌ Doesn't exist |
| switch on String | ❌ No | ✅ Yes | `match-case` (3.10+) only |
| String mutability | `std::string` is mutable | `String` immutable (`StringBuilder` for mutable) | `str` immutable |
| Access control | Enforced by compiler | Enforced by compiler (stricter, 4 levels) | Convention only, not enforced |
| Friend function | ✅ Exists | ❌ Doesn't exist | ❌ Doesn't exist |
| Platform independence | ❌ No (needs recompilation per OS) | ✅ Yes (JVM handles it) | ✅ Yes (needs interpreter installed) |

---

## 20. Quick-Fire Table for Last-Minute Revision

| Term | One-line answer |
|---|---|
| JDK vs JRE vs JVM | JDK = develop+compile; JRE = run; JVM = execute bytecode |
| `==` vs `.equals()` | `==` compares reference/address; `.equals()` compares value/content |
| Checked vs Unchecked exception | Checked = compiler forces handling; Unchecked = occurs at runtime only |
| `throw` vs `throws` | `throw` raises an exception; `throws` declares one in a method signature |
| Abstract class vs Interface | Abstract class = partial implementation, single inheritance; Interface = fully abstract (traditionally), multiple "inheritance" |
| `String` vs `StringBuilder` | Immutable vs mutable string |
| Method overloading vs overriding | Same class/diff signature (compile-time) vs parent-child/same signature (runtime, needs inheritance) |
| `this` keyword | Refers to the current object instance |
| `super` keyword | Refers to the parent class (calls parent constructor/method) |
| `static` vs `final` | `static` = belongs to class, not instance; `final` = cannot be changed/overridden/extended |
| Pass by value in Java | ALWAYS pass by value — for objects, the reference itself is copied |

---

*Want this converted into a timed MCQ tool covering Java + C++ + Python side by side — same interactive style as your Reasoning/English/Quant tools — for the IT Officer paper?*
