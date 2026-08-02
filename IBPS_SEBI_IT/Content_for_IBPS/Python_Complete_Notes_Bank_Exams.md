# Python Complete Notes — For Bank IT Officer / Computer Knowledge Exams

*(Structured to mirror the C++ notes so you can directly compare both languages topic-by-topic)*

---

## 1. Python — Basic Nature of the Language

| Property | Python |
|---|---|
| Type of language | **Interpreted** (technically compiled to bytecode, then interpreted — see Section 15) |
| Typing | **Dynamically typed** (type decided at runtime) + **Strongly typed** (no silent unsafe conversions like `"5" + 5`) |
| Case sensitivity | Yes |
| Indentation | **Mandatory** — defines blocks (no `{}` like C++) |
| Semicolons | Not required (statement ends at newline) |
| Extension | `.py` |

---

## 2. Built-in Functions & Modules (equivalent to C++ header files)

Python doesn't need header files — it has **built-in functions** (always available) and **modules** you `import`.

### Commonly used built-in functions
| Function | Purpose |
|---|---|
| `print()`, `input()` | Output / Input |
| `len()` | Length of string/list/tuple/dict |
| `type()` | Returns data type of a variable |
| `int()`, `float()`, `str()`, `bool()`, `list()`, `tuple()`, `set()`, `dict()` | Type conversion functions |
| `range()` | Generates a sequence of numbers |
| `sorted()` | Returns a new sorted list |
| `sum()`, `min()`, `max()` | Aggregate functions |
| `id()` | Memory address (identity) of an object |
| `isinstance()` | Checks object's type |
| `enumerate()` | Adds index while looping |
| `zip()` | Combines multiple iterables element-wise |
| `map()`, `filter()`, `reduce()` | Functional programming tools |

### Commonly used modules (equivalent of C++'s `<iostream>`, `<cmath>`, etc.)
| Module | Purpose | Example |
|---|---|---|
| `math` | Mathematical functions | `math.sqrt()`, `math.pow()`, `math.floor()` |
| `random` | Random number generation | `random.randint()`, `random.choice()` |
| `datetime` | Date and time | `datetime.now()` |
| `os` | Operating system interaction | `os.getcwd()`, `os.listdir()` |
| `sys` | System-specific parameters | `sys.exit()`, `sys.argv` |
| `re` | Regular expressions | `re.match()`, `re.findall()` |
| `json` | JSON parsing | `json.dumps()`, `json.loads()` |
| `collections` | Extra data structures | `Counter`, `deque`, `OrderedDict` |
| `statistics` | Mean, median, mode | `statistics.mean()` |

```python
import math
print(math.sqrt(16))   # 4.0
```

---

## 3. Data Types

| Category | Types |
|---|---|
| **Numeric** | `int`, `float`, `complex` |
| **Text** | `str` |
| **Sequence** | `list`, `tuple`, `range` |
| **Mapping** | `dict` |
| **Set types** | `set`, `frozenset` |
| **Boolean** | `bool` (`True`/`False`) |
| **Binary** | `bytes`, `bytearray`, `memoryview` |
| **None type** | `NoneType` (Python's equivalent of `null`/`void`) |

Python has **no separate `char` type** — a single character is just a `str` of length 1. Also no explicit `short`/`long`/`unsigned` modifiers like C++; Python's `int` has **unlimited precision** (grows automatically, no overflow).

Check type: `type(x)` → e.g. `<class 'int'>`

---

## 4. Static vs Dynamic Typing (Language-level)

- **Python is dynamically typed** — you don't declare a variable's type; it's decided at runtime and **can change**:
  ```python
  x = 10          # x is int
  x = "hello"     # now x is str — totally legal, no error
  ```
- Contrast with **C++ (statically typed)** where a variable's type is fixed at compile time and cannot change.
- Python is still **strongly typed**: it won't silently mix incompatible types.
  ```python
  "5" + 5   # ❌ TypeError: can only concatenate str (not "int") to str
  ```

---

## 5. Type Casting

| Type | Example |
|---|---|
| **Implicit** | `x = 5 + 2.0` → Python auto-converts int to float → `7.0` |
| **Explicit** | `int("10")`, `float(5)`, `str(25)`, `list("abc")` → `['a','b','c']` |

```python
a = "25"
b = int(a) + 5   # explicit casting needed, else TypeError
print(b)          # 30
```

---

## 6. Mutable vs Immutable — Core Topic

This is one of Python's most-asked concepts (no direct C++ equivalent because C++ variables are memory slots, not "objects with identity").

**Mutable** = value can be changed **in place** after creation (same memory address/id retained).
**Immutable** = value **cannot** be changed after creation; any "change" actually creates a **new object**.

| Immutable | Mutable |
|---|---|
| `int`, `float`, `complex`, `bool` | `list` |
| `str` | `dict` |
| `tuple` | `set` |
| `frozenset` | `bytearray` |
| `bytes` | — |

```python
# Immutable example — string
s = "Bank"
print(id(s))        # some address, e.g. 140233
s = s + "PO"
print(id(s))        # DIFFERENT address — a new object "BankPO" was created

# Mutable example — list
l = [1, 2, 3]
print(id(l))         # some address
l.append(4)
print(id(l))         # SAME address — modified in place
```

**Exam trap:** Tuples are immutable, **but** if a tuple contains a mutable object (like a list), that inner list can still be changed:
```python
t = (1, 2, [3, 4])
t[2].append(5)     # ✅ allowed — the list inside is mutable
t[0] = 99           # ❌ TypeError — tuple itself is immutable
```

**Why it matters:** Mutable objects passed to functions can be changed by the function (similar effect to pass-by-reference in C++); immutable objects cannot be changed in place (similar effect to pass-by-value). See Section 11.

---

## 7. Strings — Slicing & Substrings

### Slicing syntax
```python
sequence[start : stop : step]
```
- `start` — index to begin (inclusive), default 0
- `stop` — index to end (**exclusive**), default = length
- `step` — jump size, default 1
- Negative indices count from the end (`-1` = last character)

```python
s = "COMPETITIVE"

s[0:5]      # 'COMPE'      → characters at index 0,1,2,3,4
s[:4]       # 'COMP'       → start omitted = from beginning
s[4:]       # 'ETITIVE'    → stop omitted = till end
s[:]        # 'COMPETITIVE' → full copy
s[-4:]      # 'TIVE'        → last 4 characters
s[::-1]     # 'EVITITEPMOC' → reverses the string
s[::2]      # 'CMEIIE'      → every 2nd character
s[2:9:2]    # 'MEIIE'[subset] → start=2, stop=9, step=2
```

Slicing works the same way on **lists** and **tuples**:
```python
lst = [10, 20, 30, 40, 50]
lst[1:4]     # [20, 30, 40]
lst[::-1]    # [50, 40, 30, 20, 10]
```

### Substring — checking, finding, extracting
```python
s = "State Bank of India"

"Bank" in s              # True  → substring membership check
s.find("Bank")            # 6     → index of first occurrence, -1 if not found
s.index("Bank")           # 6     → same as find(), but raises ValueError if not found
s.count("a")               # 2    → number of occurrences
s[6:10]                    # 'Bank' → extracting substring via slicing
s.replace("India", "Bharat")  # 'State Bank of Bharat'
s.split(" ")                # ['State', 'Bank', 'of', 'India']
" ".join(["State","Bank"])  # 'State Bank'
s.startswith("State")       # True
s.endswith("India")         # True
s.upper(), s.lower()        # case conversion
s.strip()                    # removes leading/trailing whitespace
```

---

## 8. Operators & Decision-Making

### if / elif / else
```python
marks = 85
if marks >= 90:
    grade = "A+"
elif marks >= 75:
    grade = "A"
else:
    grade = "B"
```

### match-case (Python 3.10+, equivalent to C++ `switch`)
```python
match day:
    case 1:
        print("Monday")
    case 2:
        print("Tuesday")
    case _:
        print("Invalid")   # '_' = default case, like `default` in C++
```
- **Older Python (before 3.10) has NO switch equivalent** — this is a key difference from C++; you use `if-elif` chains or a dictionary-mapping trick instead.
- No fall-through by default (unlike C++'s switch, which falls through without `break`).

### Ternary / conditional expression
```python
result = "Pass" if marks >= 40 else "Fail"
```

---

## 9. Loops

| Loop | Structure | Notes |
|---|---|---|
| **for** | `for i in range(5):` | Iterates over a sequence/iterable directly (not counter-based like C++) |
| **while** | `while condition:` | Same concept as C++ |
| **do-while** | ❌ **Does not exist in Python** | Simulate with `while True:` + `break` |
| **Nested loops** | loop inside loop | Same as C++ |
| **List/Dict/Set comprehension** | `[x*x for x in range(5)]` | Python-only compact loop syntax, no C++ equivalent |

```python
for i in range(1, 6):        # 1 2 3 4 5
    print(i)

for ch in "Bank":             # iterating directly over a string
    print(ch)

squares = [x**2 for x in range(5)]   # list comprehension: [0,1,4,9,16]
```

---

## 10. Jump Statements

| Statement | Effect | Same as C++? |
|---|---|---|
| `break` | Exits the loop | ✅ Same |
| `continue` | Skips to next iteration | ✅ Same |
| `pass` | Does nothing — a placeholder (Python needs *something* in an empty block since indentation-based) | ❌ No direct C++ equivalent |
| `return` | Exits function, optionally with value | ✅ Same |
| `goto` | ❌ **Does not exist in Python** — considered bad practice, removed entirely | Different from C++ |

---

## 11. Functions

```python
def function_name(parameters):
    # body
    return value
```

### Types of functions
1. **Built-in functions** — `len()`, `print()`, etc.
2. **User-defined functions** — `def` keyword.
3. **Lambda (anonymous) functions** — single-expression inline functions:
   ```python
   square = lambda x: x * x
   print(square(5))   # 25
   ```
4. **Recursive functions** — function calling itself.
5. **Default argument functions:**
   ```python
   def greet(name="Guest"):
       print(f"Hello {name}")
   ```
6. **Variable-length arguments:**
   ```python
   def add(*args):        # *args → tuple of positional args
       return sum(args)
   def show(**kwargs):     # **kwargs → dict of keyword args
       print(kwargs)
   ```
7. **Generator functions** — use `yield` instead of `return`; produce values lazily, one at a time (memory-efficient):
   ```python
   def counter(n):
       for i in range(n):
           yield i
   ```
8. **Higher-order functions** — take/return other functions: `map()`, `filter()`.
9. **Decorators** — functions that modify the behaviour of another function (Python-specific, no C++ direct equivalent):
   ```python
   def my_decorator(func):
       def wrapper():
           print("Before function runs")
           func()
       return wrapper

   @my_decorator
   def say_hello():
       print("Hello")

   say_hello()
   ```

### Argument passing — "Pass by Object Reference"
Python uses neither pure pass-by-value nor pure pass-by-reference (as C++ defines them) — it uses **pass by object reference**:
- If you pass a **mutable** object (list, dict), changes made **inside the function persist outside** (like pass-by-reference).
- If you pass an **immutable** object (int, str, tuple), the function gets a new local binding — reassigning it inside does **not** affect the original (like pass-by-value).

```python
def modify_list(lst):
    lst.append(100)     # mutates the SAME object → affects caller
def modify_num(n):
    n = n + 100          # creates a NEW object → does NOT affect caller

l = [1,2,3]; modify_list(l);   print(l)   # [1, 2, 3, 100]
x = 5;       modify_num(x);    print(x)   # 5 (unchanged)
```

---

## 12. Memory Allocation & Management (compare with C++ Section 3)

| Concept | Python |
|---|---|
| Memory management | **Automatic** — Python has a built-in **Garbage Collector**; no `malloc`/`free`/`new`/`delete` needed |
| Reference counting | Every object tracks how many references point to it; when count hits 0, memory is auto-freed |
| Garbage value? | **No such thing** — variables don't exist until assigned; there's no "uninitialized garbage" concept like C++ stack variables |
| `del` keyword | Removes a reference to an object (may trigger garbage collection if it was the last reference) |
| Manual GC control | `import gc; gc.collect()` — force garbage collection |
| Everything is an object | Even `int`, `str`, functions — all have an `id()` (memory address) and a `type()` |

```python
a = [1, 2, 3]
b = a          # b points to the SAME object (reference copy)
b.append(4)
print(a)        # [1, 2, 3, 4] — a is affected too!

import copy
c = copy.deepcopy(a)   # creates a completely independent copy
```
This is a very common exam trap — `b = a` does **not** create a new list in Python (unlike C++ where `int b = a;` copies the value).

---

## 13. Error Handling — Core Topic

### Types of Errors in Python

| Error Type | When it occurs | Example |
|---|---|---|
| **Syntax Error** | Code violates Python's grammar rules — caught **before** the program runs (this is Python's closest equivalent to a "compilation error") | `if x = 5:` (missing `==`) |
| **Runtime Error / Exception** | Code is syntactically correct but fails **while executing** | `10 / 0` → `ZeroDivisionError` |
| **Logical Error** | Code runs fine, no error message, but gives the **wrong result** | Using `+` instead of `*` by mistake |

### 🔑 "Compilation Error" — Python vs C++
- **C++** is compiled ahead-of-time; a compilation error stops the `.exe` from ever being built (e.g., missing semicolon, type mismatch).
- **Python is interpreted**, so there's technically no separate "compile step" the programmer controls — **but** Python does first check syntax and compiles to bytecode (see Section 15) before running, so a `SyntaxError` is the practical equivalent: it's caught **before any code executes**, even code below the error.
  ```python
  print("This runs")
  if True     # missing colon
      print("Never reached")
  # SyntaxError is raised immediately — NEITHER line executes, similar to a C++ compile error blocking the whole build
  ```

### Common built-in exceptions
| Exception | Cause |
|---|---|
| `ZeroDivisionError` | Division by zero |
| `TypeError` | Operation on incompatible types |
| `ValueError` | Right type, invalid value (e.g., `int("abc")`) |
| `IndexError` | List/string index out of range |
| `KeyError` | Dictionary key not found |
| `NameError` | Variable not defined |
| `AttributeError` | Invalid attribute/method access |
| `FileNotFoundError` | File doesn't exist |
| `ImportError` / `ModuleNotFoundError` | Module can't be imported |
| `RecursionError` | Maximum recursion depth exceeded |

### try / except / else / finally
```python
try:
    a = int(input("Enter number: "))
    result = 100 / a
except ZeroDivisionError:
    print("Cannot divide by zero")
except ValueError:
    print("Please enter a valid number")
except Exception as e:              # catches any other exception
    print(f"Something went wrong: {e}")
else:
    print(f"Result is {result}")    # runs ONLY IF no exception occurred
finally:
    print("This always runs")        # cleanup code — runs no matter what
```

### Raising exceptions manually
```python
def withdraw(balance, amount):
    if amount > balance:
        raise ValueError("Insufficient balance")
    return balance - amount
```

### Custom exceptions (user-defined, similar to a custom class in C++)
```python
class InsufficientBalanceError(Exception):
    def __init__(self, message="Balance too low for this transaction"):
        super().__init__(message)

try:
    raise InsufficientBalanceError()
except InsufficientBalanceError as e:
    print(e)
```

### `assert` statement
```python
assert age >= 18, "Must be an adult"   # raises AssertionError if False
```

---

## 14. Classes & OOP (compare with C++ Section 12)

```python
class Account:
    bank_name = "SBI"                 # class variable (shared by all objects)

    def __init__(self, balance):      # constructor (equivalent to C++ constructor)
        self.balance = balance         # instance variable

    def deposit(self, amt):
        self.balance += amt

    def __del__(self):                 # destructor (rarely used explicitly in Python)
        print("Object destroyed")

acc = Account(1000)
acc.deposit(500)
```

| Concept | C++ | Python |
|---|---|---|
| Constructor | `ClassName()` | `__init__(self)` |
| Destructor | `~ClassName()` | `__del__(self)` |
| Access control | `private/public/protected` keywords (enforced by compiler) | **Convention only** — `_var` (protected, by convention), `__var` (name-mangled "private"); NOT strictly enforced |
| `self`/`this` | `this` (implicit pointer) | `self` (explicit first parameter) |
| Inheritance | `class B : public A` | `class B(A):` |
| Multiple inheritance | Supported, complex | Supported, simpler syntax: `class C(A, B):` |
| Method overloading | Supported natively | **Not supported natively** — last definition overrides earlier ones; simulate with default args or `*args` |
| Method overriding | `virtual` keyword needed | Automatic — any subclass method with the same name overrides the parent's |
| Operator overloading | `operator+` | **Dunder (magic) methods**: `__add__`, `__eq__`, `__str__`, `__len__`, etc. |
| Abstract class | Pure virtual function `=0` | `from abc import ABC, abstractmethod` |
| Friend function | `friend` keyword | **Does not exist** — Python has no strict private access to bypass |

### The Four OOP Pillars — same concepts as C++, Python syntax
```python
# Inheritance
class Employee:
    def work(self): print("Working")

class Manager(Employee):              # inherits from Employee
    def work(self):                    # Overriding (runtime polymorphism)
        print("Managing team")

# Polymorphism in action
for person in [Employee(), Manager()]:
    person.work()     # different output depending on actual object type

# Encapsulation
class Wallet:
    def __init__(self):
        self.__cash = 0             # "private" via double underscore (name mangling)
    def add(self, amt):
        self.__cash += amt

# Abstraction
from abc import ABC, abstractmethod
class Shape(ABC):
    @abstractmethod
    def area(self): pass
```

---

## 15. Execution of a Python Program — Layers (compare with C++ Section 14)

```
Source Code (.py)
        ↓
 [1] PYTHON INTERPRETER checks SYNTAX first
        ↓  (if syntax is invalid → SyntaxError, nothing runs — this is Python's "compile error")
 [2] COMPILATION TO BYTECODE  → source is compiled into intermediate .pyc bytecode
        ↓                        (stored in __pycache__ folder for reuse)
 [3] PYTHON VIRTUAL MACHINE (PVM) → interprets/executes the bytecode line by line
        ↓
 [4] OUTPUT
```

- Python is technically **"compiled + interpreted"** — it compiles to bytecode first, then the **PVM (Python Virtual Machine)** interprets that bytecode. This is conceptually similar to Java compiling to bytecode for the JVM.
- **CPython** is the standard/default interpreter (written in C). Other implementations: **PyPy** (faster, JIT-compiled), **Jython** (runs on JVM), **IronPython** (runs on .NET).
- Unlike C++ (Section 14: preprocessor → compiler → assembler → linker → loader → CPU produces a standalone `.exe`), Python **needs the interpreter installed** on any machine that runs it — there's no separate linking step producing a native executable by default.

---

## 16. Important Python-only Concepts (things C++ doesn't really have — filling the gaps)

### List, Dict, Set, Tuple — quick comparison
| Structure | Ordered? | Mutable? | Duplicates allowed? | Syntax |
|---|---|---|---|---|
| `list` | Yes | ✅ Yes | Yes | `[1,2,3]` |
| `tuple` | Yes | ❌ No | Yes | `(1,2,3)` |
| `set` | No (unordered) | ✅ Yes | ❌ No | `{1,2,3}` |
| `dict` | Yes (insertion order, 3.7+) | ✅ Yes (keys immutable, values mutable) | Keys: No, Values: Yes | `{"a":1,"b":2}` |

### Comprehensions
```python
squares = [x*x for x in range(10) if x % 2 == 0]     # list comprehension
sq_dict = {x: x*x for x in range(5)}                   # dict comprehension
sq_set  = {x*x for x in range(5)}                       # set comprehension
```

### Iterators & Generators
- **Iterable** — anything you can loop over (`list`, `str`, `dict`...)
- **Iterator** — object with `__next__()` that produces values one at a time
- **Generator** — function using `yield`; automatically becomes an iterator, doesn't store all values in memory at once (more memory-efficient than a list for large sequences)

### Modules & Packages
- **Module** — a single `.py` file
- **Package** — a folder of modules with an `__init__.py`
- `import module_name`, `from module_name import function_name`

### File Handling
```python
with open("data.txt", "r") as f:   # 'with' auto-closes the file (context manager)
    content = f.read()
```
Modes: `"r"` read, `"w"` write (overwrite), `"a"` append, `"rb"/"wb"` binary.

### Multithreading / Locks (compare with C++ Section 13B)
```python
import threading
lock = threading.Lock()
lock.acquire()
# critical section
lock.release()
```
- Python has the **GIL (Global Interpreter Lock)** — only one thread executes Python bytecode at a time, even on multi-core CPUs. This is a very common interview/exam trivia point unique to Python (C++ has no such restriction).

---

## 17. Quick-Fire Comparison Table: Python vs C++

| Concept | C++ | Python |
|---|---|---|
| Typing | Static | Dynamic |
| Compilation | Compiled to native machine code | Compiled to bytecode, then interpreted (PVM) |
| Memory management | Manual (`new`/`delete`) | Automatic (Garbage Collector) |
| Variable declaration | Needs data type | No type declaration needed |
| Semicolons/braces | Required | Not required (indentation-based) |
| switch statement | Yes | `match-case` (3.10+) only, otherwise use if-elif |
| do-while loop | Yes | ❌ Does not exist |
| Pointers | Yes, explicit | ❌ No explicit pointers (references handled internally) |
| Function/operator overloading | Native support | Overloading via `*args`/dunder methods only |
| Access modifiers | Enforced (`private` etc.) | Convention only (`_`, `__`) |
| Multiple inheritance | Supported, complex (diamond problem) | Supported, simpler (MRO - Method Resolution Order) |
| Speed | Faster (compiled, low-level) | Slower (interpreted, high-level) |
| Uninitialized variable | Holds garbage value | Doesn't exist until assigned (`NameError` if used) |

---

## 18. Quick-Fire Table for Last-Minute Revision

| Term | One-line answer |
|---|---|
| Mutable vs Immutable | Mutable changes in place (list/dict/set); immutable creates a new object on "change" (int/str/tuple) |
| Slicing | `seq[start:stop:step]` — stop is exclusive |
| `find()` vs `index()` | `find()` returns -1 if not found; `index()` raises `ValueError` |
| SyntaxError vs Exception | SyntaxError = caught before running (like a compile error); Exception = occurs while running |
| `is` vs `==` | `is` compares identity/memory address (`id()`); `==` compares value |
| Shallow copy vs Deep copy | Shallow copies references (nested objects shared); deep copy is fully independent |
| `*args` vs `**kwargs` | `*args` → tuple of extra positional args; `**kwargs` → dict of extra keyword args |
| GIL | Global Interpreter Lock — only one thread runs Python bytecode at a time |
| List comprehension | Compact way to build a list in one line: `[expr for item in iterable if condition]` |
| `pass` vs `continue` vs `break` | `pass` = do nothing (placeholder); `continue` = skip to next iteration; `break` = exit loop |

---

*Want this converted into a timed MCQ practice tool (same interactive style as your Reasoning/English/Quant tools), covering Python + C++ side by side for the IT Officer paper?*
