# C++ Complete Notes — For Bank IT Officer / Computer Knowledge Exams

---

## 1. Header Files & Built-in Functions

A **header file** contains function declarations, macros, and class definitions that you `#include` so you can use pre-written code instead of writing it yourself.

| Header | Purpose | Common Built-in Functions |
|---|---|---|
| `<iostream>` | Input/Output stream | `cin`, `cout`, `cerr`, `clog`, `endl` |
| `<cstdio>` / `<stdio.h>` | C-style I/O | `printf()`, `scanf()`, `gets()` |
| `<cmath>` / `<math.h>` | Math functions | `sqrt()`, `pow()`, `abs()`, `ceil()`, `floor()` |
| `<cstring>` / `<string.h>` | C-string handling | `strlen()`, `strcpy()`, `strcat()`, `strcmp()` |
| `<string>` | C++ string class | `.length()`, `.substr()`, `.find()`, `.append()` |
| `<vector>` | Dynamic array container | `.push_back()`, `.pop_back()`, `.size()` |
| `<algorithm>` | Common algorithms | `sort()`, `reverse()`, `max()`, `min()`, `swap()` |
| `<cstdlib>` / `<stdlib.h>` | General utilities | `malloc()`, `calloc()`, `free()`, `rand()`, `exit()` |
| `<ctime>` / `<time.h>` | Date/time | `time()`, `clock()` |
| `<fstream>` | File handling | `ifstream`, `ofstream`, `fstream` |
| `<map>`, `<set>`, `<stack>`, `<queue>` | STL containers | container-specific methods |
| `<climits>` | Limits of data types | `INT_MAX`, `INT_MIN` |

**Note for exams:** A header file itself has *no executable body* for most declarations — it just tells the compiler "this function/class exists, its definition is linked in elsewhere." The `.cpp` file (or the library) supplies the actual implementation.

---

## 2. Streams (I/O)

A **stream** is a sequence of bytes flowing between the program and a device (keyboard, screen, file).

- **Input stream** — data flows *into* the program → `cin`, `ifstream`
- **Output stream** — data flows *out of* the program → `cout`, `ofstream`
- **cin** — reads from standard input (keyboard), stops at whitespace
- **cout** — writes to standard output (screen), `<<` is the insertion operator
- **cin.getline()** — reads a full line including spaces
- **cerr** — unbuffered error stream (prints immediately, no buffering delay)
- **clog** — buffered error/log stream
- **File streams:**
  - `ifstream` → read from a file
  - `ofstream` → write to a file
  - `fstream` → both read and write

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    ofstream fout("data.txt");
    fout << "Hello Bank Exam";
    fout.close();

    ifstream fin("data.txt");
    string line;
    getline(fin, line);
    cout << line;
}
```

---

## 3. Memory Allocation — Very Important for Exams

C++ has **two types of memory**:

| Memory Type | Where | Managed By | Speed | Lifetime |
|---|---|---|---|---|
| **Stack** | Fixed, small | Compiler (automatic) | Fast | Ends when function/scope ends |
| **Heap** | Large, flexible | Programmer (manual) | Slower | Until explicitly freed / `delete` |

### Static vs Dynamic allocation
- **Static (compile-time) allocation** — memory size decided at compile time. E.g., `int a[10];` — fixed array on the stack.
- **Dynamic (run-time) allocation** — memory allocated while the program runs, on the **heap**, using `new` (C++) or `malloc()`/`calloc()` (C).

### 🔑 The exact point you asked about — garbage value vs zero-initialized

| Function/Operator | Initializes memory to 0? | Notes |
|---|---|---|
| `malloc()` | ❌ **No — gives garbage/uninitialized value** | Only allocates raw memory |
| `calloc()` | ✅ **Yes — initializes all bytes to 0** | "calloc" = **c**lear + **alloc**ate |
| `new` (single variable, e.g. `new int`) | ❌ **No — gives garbage value** | Same as malloc behavior |
| `new int()` / `new int{}` (with parentheses/braces) | ✅ **Yes — value-initializes to 0** | Special C++ syntax |
| `new int[5]` (array, no initializer) | ❌ **No — garbage values** | |
| `new int[5]()` (array with parentheses) | ✅ **Yes — all elements zero** | |
| Global/static variables (uninitialized) | ✅ **Yes — automatically 0** | Stored in BSS segment |
| Local variables (uninitialized) | ❌ **No — garbage value** | Stack doesn't clear old data |

```cpp
int *p1 = new int;        // garbage value
int *p2 = new int();      // 0
int *p3 = (int*) malloc(sizeof(int));   // garbage value
int *p4 = (int*) calloc(1, sizeof(int)); // 0
delete p1; delete p2; free(p3); free(p4);
```

**Freeing memory:**
- C: `free(ptr)`
- C++: `delete ptr;` (single) or `delete[] ptr;` (array)
- Forgetting this causes a **memory leak**.

---

## 4. Data Types

| Category | Types |
|---|---|
| **Primitive/Basic** | `int`, `char`, `float`, `double`, `bool`, `void` |
| **Derived** | array, pointer, function, reference |
| **User-defined** | `struct`, `class`, `union`, `enum`, `typedef` |
| **Modifiers** | `signed`, `unsigned`, `short`, `long` |

Typical sizes (on a 64-bit system, compiler-dependent):
- `char` → 1 byte
- `int` → 4 bytes
- `float` → 4 bytes
- `double` → 8 bytes
- `bool` → 1 byte
- `long` → 8 bytes

---

## 5. Static vs Dynamic Typing (Language-level concept)

- **C++ is a statically typed language** — the type of every variable is checked and fixed **at compile time**. You cannot assign a `string` to an `int` variable later.
- **Dynamically typed languages** (Python, JavaScript) check types **at run time**, and a variable's type can change.
- This is different from *static/dynamic memory allocation* (Section 3) — don't confuse the two in exams; they are separate concepts that happen to share the word "static/dynamic."

---

## 6. Type Casting

Converting one data type into another.

1. **Implicit (automatic) type conversion** — compiler does it silently.
   ```cpp
   int a = 5;
   double b = a;   // int auto-converted to double
   ```
2. **Explicit type conversion (casting)** — programmer forces it.
   - **C-style cast:** `(int) 3.14`
   - **Function-style cast:** `int(3.14)`
   - **C++ cast operators:**
     - `static_cast<type>(value)` — general-purpose, compile-time checked (most common/safe)
     - `dynamic_cast<type>(value)` — safe downcasting in class hierarchies (runtime-checked, needs polymorphism)
     - `const_cast<type>(value)` — adds/removes `const`
     - `reinterpret_cast<type>(value)` — reinterprets bit pattern (low-level, unsafe)

---

## 7. Pointers

A **pointer** is a variable that stores the **memory address** of another variable.

```cpp
int a = 10;
int *p = &a;      // p holds address of a
cout << *p;       // dereference: prints value at that address, i.e., 10
cout << p;        // prints the address itself
```

- `&` → address-of operator
- `*` → dereference operator (also used to declare a pointer)
- **Null pointer:** `int *p = nullptr;` (points to nothing)
- **Dangling pointer:** points to memory that has already been freed
- **Pointer to pointer:** `int **pp = &p;`
- **Pointer arithmetic:** `p+1` moves to the next memory block of that data type's size
- **Void pointer:** `void *p;` — generic pointer, can hold address of any type but can't be dereferenced directly

### Pointers vs References
| Pointer | Reference |
|---|---|
| Can be reassigned to point elsewhere | Cannot be re-bound after initialization |
| Can be `NULL`/`nullptr` | Cannot be null |
| Needs `*` to dereference | Used directly like the original variable |
| Has its own memory address | Just an alias — no separate address in practice |

---

## 8. Functions

A function is a named, reusable block of code.

```cpp
returnType functionName(parameterList) {
    // body
    return value;
}
```

### Types of functions
1. **Built-in / Library functions** — `sqrt()`, `strlen()`, etc.
2. **User-defined functions** — written by the programmer.
3. **Based on return/parameters:**
   - No arguments, no return
   - No arguments, with return
   - With arguments, no return
   - With arguments, with return
4. **Recursive function** — calls itself (e.g., factorial, Fibonacci).
5. **Inline function** — `inline` keyword requests the compiler to substitute the function's code directly at the call site (saves function-call overhead, used for small functions).
6. **Friend function** — see Section 12.
7. **Virtual function** — see Section 13 (used for runtime polymorphism).
8. **Function overloading** — same function name, different parameter list (compile-time/static polymorphism).
   ```cpp
   int add(int a, int b);
   double add(double a, double b);
   ```
9. **Default arguments** — `void greet(string name = "Guest");`
10. **Lambda function** (modern C++) — anonymous inline function: `auto sq = [](int x){ return x*x; };`

### Pass by Value vs Pass by Reference

| Pass by Value | Pass by Reference |
|---|---|
| A **copy** of the argument is sent | The **actual variable** (its address/alias) is sent |
| Changes inside function do NOT affect the original | Changes inside function DO affect the original |
| `void f(int x)` | `void f(int &x)` |
| Safer, but uses extra memory for copy | Efficient for large data (no copying), but riskier |

```cpp
void byValue(int x) { x = 100; }        // original unchanged
void byRef(int &x)  { x = 100; }        // original changed
void byPointer(int *x) { *x = 100; }    // "pass by address" - original changed via pointer
```

There's also a third style, **pass by address/pointer**, which some syllabi list separately from reference — functionally it achieves the same result as pass-by-reference but uses pointer syntax.

---

## 9. Decision-Making Statements

### if / else
```cpp
if (condition) { }
else if (condition2) { }
else { }
```

### switch statement
```cpp
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // code
}
```
- **Does switch take an expression?** Yes — `switch(expression)` evaluates an **integral or enum expression** (int, char, enum). In C++11 onward, it does **not** accept `float`, `double`, or `string` directly (unlike some other languages).
- Without `break`, execution **falls through** to the next case.
- **Ternary operator** (conditional expression, shorthand for if-else): `condition ? valueIfTrue : valueIfFalse;`

---

## 10. Loops

| Loop | Structure | Use case |
|---|---|---|
| **for** | `for(init; condition; update)` | Known number of iterations |
| **while** | `while(condition) { }` | Condition checked before each iteration |
| **do-while** | `do { } while(condition);` | Executes **at least once**, condition checked after |
| **range-based for** (C++11) | `for(int x : arr) { }` | Iterating over containers/arrays cleanly |
| **nested loops** | loop inside a loop | Matrices, patterns |

---

## 11. Jump Statements

| Statement | Effect |
|---|---|
| `break` | Exits the nearest enclosing loop or switch immediately |
| `continue` | Skips the rest of the current iteration, moves to next iteration |
| `goto label;` | Unconditional jump to a labeled statement (discouraged — makes code hard to follow) |
| `return` | Exits a function, optionally sending back a value |
| `exit()` | Terminates the entire program (from `<cstdlib>`) |

---

## 12. Classes, Objects & Friend Function

### Class basics
```cpp
class Account {
private:
    double balance;       // hidden from outside
public:
    Account(double b) { balance = b; }     // constructor
    ~Account() { }                          // destructor
    void deposit(double amt) { balance += amt; }
    double getBalance() { return balance; }
};

int main() {
    Account acc(1000);   // object creation
    acc.deposit(500);
}
```

### Access specifiers
- **private** — accessible only within the class
- **public** — accessible from anywhere the object is visible
- **protected** — accessible within the class and its derived (child) classes

### Constructors & Destructors
- **Constructor** — special function, same name as class, no return type, runs automatically when an object is created. Can be default, parameterized, or copy constructor.
- **Destructor** — `~ClassName()`, runs automatically when object goes out of scope/is deleted; used to free resources.

### Friend function
A function that is **not a member of the class** but is still granted access to its **private and protected members**, by declaring it with the `friend` keyword **inside** the class.

```cpp
class Box {
private:
    int width;
public:
    Box(int w) : width(w) {}
    friend void printWidth(Box b);   // friend declaration
};

void printWidth(Box b) {
    cout << b.width;   // allowed only because it's a friend
}
```
- A friend function is **not inherited** and does **not** need an object to be "called on" — it's called normally.
- A **friend class** is also possible — every member function of that class gets access.

### The Four Pillars of OOP (commonly asked, and part of "everything missing")
1. **Encapsulation** — bundling data + methods together, hiding internal details using access specifiers.
2. **Abstraction** — showing only essential features, hiding implementation complexity (e.g., abstract classes, interfaces).
3. **Inheritance** — a class (child) acquires properties of another class (parent) using `class Child : public Parent`.
4. **Polymorphism** — one interface, many forms.
   - **Compile-time (static) polymorphism** — function overloading, operator overloading.
   - **Run-time (dynamic) polymorphism** — function overriding using `virtual` functions.

### Virtual functions & Abstract classes
```cpp
class Shape {
public:
    virtual void draw() { cout << "Shape"; }   // virtual = can be overridden
};
class Circle : public Shape {
public:
    void draw() override { cout << "Circle"; }
};
```
- `virtual` enables **runtime polymorphism** via a mechanism called the **vtable (virtual table)**.
- A **pure virtual function** (`virtual void draw() = 0;`) makes the class **abstract** — it cannot be instantiated, only used as a base.

### Operator Overloading
Redefining how an operator behaves for a user-defined type.
```cpp
class Complex {
public:
    int real, imag;
    Complex operator+(Complex const &c) {
        Complex res;
        res.real = real + c.real;
        return res;
    }
};
```

---

## 13. Locks / Access Control / Concurrency Locks — Two Meanings

Since "locks" is ambiguous, here are both meanings relevant to C++:

### A) Access-level "locking" (data hiding)
The `private`, `public`, and `protected` keywords "lock" data from unintended access — this is the encapsulation mechanism covered in Section 12.
- `const` keyword **locks** a variable so it cannot be modified after initialization.
- `final` keyword (C++11) **locks** a class from being inherited, or a virtual function from being overridden further.

### B) Concurrency locks (multithreading — from `<mutex>`)
Used when multiple threads access shared data, to prevent race conditions.
- **`std::mutex`** — a lock object; `.lock()` and `.unlock()` manually guard a critical section.
- **`std::lock_guard`** — automatically locks on creation and unlocks when it goes out of scope (RAII-safe).
- **`std::unique_lock`** — more flexible, lockable/unlockable manually, works with condition variables.
- **`std::deadlock`** — a situation (not a keyword) where two or more threads wait on each other's locks forever — something to *avoid*, common exam-trap term.

```cpp
#include <mutex>
std::mutex mtx;
void safePrint() {
    mtx.lock();
    cout << "Critical section";
    mtx.unlock();
}
```

---

## 14. Execution of a C++ Program — Layers / Translation Process

This answers your "layers/converter" question — the full pipeline from source code to running program:

```
Source Code (.cpp)
        ↓
 [1] PREPROCESSOR  → expands #include, #define, macros → produces expanded source
        ↓
 [2] COMPILER      → translates C++ code into Assembly code
        ↓
 [3] ASSEMBLER     → converts Assembly code into Object code (machine code, .o/.obj — not yet runnable alone)
        ↓
 [4] LINKER        → combines your object code + library object code (like <iostream>'s compiled code) 
                      into a single executable file (.exe), resolves function references
        ↓
 [5] LOADER        → loads the executable into memory (RAM) when you run it
        ↓
 [6] CPU EXECUTION → the OS hands control to the program; CPU executes machine instructions
```

- **C++ is a compiled language** (unlike Java, which is compiled to bytecode and run on the JVM, or Python, which is interpreted).
- **Interpreter vs Compiler:** an interpreter executes code line-by-line at runtime (slower, easier to debug); a compiler translates the whole program to machine code before execution (faster to run).
- Common C++ compilers: **GCC/G++, Clang, MSVC (Microsoft Visual C++), Turbo C++** (legacy, still asked about in some exams).

---

## 15. STL (Standard Template Library) — Frequently Asked, Often Missing from Notes

STL has 4 main components:
1. **Containers** — `vector`, `list`, `deque`, `map`, `set`, `stack`, `queue`
2. **Algorithms** — `sort()`, `find()`, `binary_search()`, `reverse()`
3. **Iterators** — act like pointers to move through containers (`begin()`, `end()`)
4. **Functors** — objects that can be called like functions

```cpp
#include <vector>
#include <algorithm>
vector<int> v = {5, 2, 8, 1};
sort(v.begin(), v.end());   // 1 2 5 8
```

---

## 16. Exception Handling — Also Frequently Missing

```cpp
try {
    int a = 5, b = 0;
    if (b == 0) throw runtime_error("Divide by zero!");
    cout << a/b;
}
catch (runtime_error &e) {
    cout << "Caught: " << e.what();
}
catch (...) {
    cout << "Unknown exception";
}
```
- `try` — block where exception might occur
- `throw` — signals an exception
- `catch` — handles it
- Prevents abrupt program crashes.

---

## 17. Templates — Also Frequently Missing

Allows writing **generic, type-independent** functions/classes.
```cpp
template <typename T>
T maxVal(T a, T b) {
    return (a > b) ? a : b;
}
// works for int, float, char, etc. — one function, many types
```

---

## 18. Quick-Fire Table for Last-Minute Revision

| Term | One-line answer |
|---|---|
| `malloc` vs `calloc` | malloc → garbage, calloc → zero-initialized |
| `new` vs `malloc` | `new` calls constructor & is type-safe; `malloc` doesn't |
| Stack vs Heap | Stack = automatic/fast/small; Heap = manual/slower/large |
| Pass by value vs reference | Copy vs actual variable access |
| Function overloading vs overriding | Overloading = same class, diff signature (compile-time); Overriding = base/derived, same signature (`virtual`, run-time) |
| Static vs dynamic typing | Compile-time type check vs run-time type check |
| `break` vs `continue` | Exit loop entirely vs skip current iteration |
| Structure vs Class | struct members default `public`; class members default `private` |
| Compiler vs Interpreter | Whole-program translation vs line-by-line execution |
| Constructor vs Destructor | Object creation setup vs object destruction cleanup |

---

*This document is built to map directly onto typical bank IT-officer / computer-knowledge syllabus points (SBI SO, IBPS SO IT Officer, RBI Grade B – ESI stream). Want me to convert this into a timed MCQ practice tool (same style as your other Reasoning/English/Quant tools) covering these exact topics?*
