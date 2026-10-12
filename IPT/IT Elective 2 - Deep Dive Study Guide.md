# IT Elective 2 — Integrative Programming
## Deep-Dive Study Guide

*Covers all seven units: Introduction, Syntax/Variables/Data Types, Control Structures, Iterations, Arrays, Classes/Functions, File Management.*

---

# Unit 1 — Introduction to Programming Languages

## 1a. Overview of Programming Languages

A programming language is a formal notation with strict grammar rules (syntax) and defined meaning (semantics) that lets a person describe computation precisely enough for a machine to carry it out. "Formal" is the key word — unlike English, a programming language cannot tolerate ambiguity. `x = x + 1` means exactly one thing.

Languages are usually grouped by how close they sit to the hardware:

- **Machine code** — raw binary instructions a CPU executes directly. Not human-friendly.
- **Assembly language** — a thin, readable layer over machine code (`MOV`, `ADD`, `JMP`). One assembly instruction ≈ one machine instruction.
- **High-level languages** — Python, Java, C++, JavaScript. One line can represent dozens of machine instructions. These trade a little performance for a large gain in readability, portability, and development speed.

Within high-level languages there's a further split by **paradigm** — the mental model the language encourages:
- **Procedural** (C, early Python scripts) — programs as sequences of instructions and function calls.
- **Object-oriented** (Java, Python, C++) — programs as interacting objects that bundle data and behavior.
- **Functional** (Haskell, parts of Python/JavaScript) — programs as compositions of pure functions with minimal mutable state.
- **Declarative** (SQL, HTML) — you describe *what* you want, not *how* to get it.

Most mainstream languages, including Python, are **multi-paradigm** — they let you mix procedural, object-oriented, and functional styles in the same program.

## 1b. Aspects of Programming Language Features

When comparing or evaluating a language, four features matter most:

| Feature | Question it answers | Python's answer |
|---|---|---|
| **Syntax** | What does valid code look like? | Indentation-based blocks, minimal punctuation |
| **Semantics** | What does valid code *mean*? | Defined by the language reference / interpreter behavior |
| **Type system** | How are data types checked and enforced? | Dynamically typed, strongly typed |
| **Paradigm support** | What programming styles does it encourage? | Multi-paradigm |

**Typing** deserves special attention because it trips up beginners moving between languages:
- **Static vs. dynamic** — static (Java, C++) checks types before running the program (compile time); dynamic (Python, JavaScript) checks types while the program runs.
- **Strong vs. weak** — strong typing (Python) refuses to silently convert incompatible types (`"5" + 5` raises an error); weak typing (JavaScript) often converts automatically (`"5" + 5` → `"55"`).

Python is **dynamically, strongly typed**: you never declare a variable's type, but the interpreter never silently guesses one for you either.

## 1c. Importance of Studying Programming Languages

This isn't just academic. Three concrete payoffs:

1. **Choosing the right tool.** A scripting task, a mobile app, and an embedded sensor controller have different constraints — knowing *why* languages differ helps you pick well instead of by habit.
2. **Faster learning of new languages.** Concepts like variables, loops, and functions are universal; only the syntax changes. Once you understand the concept of a "for loop" deeply in Python, picking it up in Java or JavaScript is mostly a vocabulary problem.
3. **Better engineering judgment.** Understanding trade-offs (e.g., interpreted languages are more flexible but generally slower; compiled languages are faster but stricter) lets you reason about performance, safety, and maintainability instead of guessing.

## 1d. Compilers and Run-Time Systems

**Compilation** and **interpretation** are the two main strategies for turning source code into something a CPU can run.

- A **compiler** reads the entire source file, translates it into machine code (or an intermediate form) *before* execution, and produces a standalone executable. Errors are caught up front, and the resulting program tends to run fast because no translation happens during execution. C and C++ work this way.
- An **interpreter** reads and executes code line by line (or statement by statement) *while the program runs*. There's no separate build step — you run the source directly. This makes development faster to iterate on, at some runtime cost.

**Python actually does both**, which is a common point of confusion:
1. The Python interpreter first compiles your `.py` source into **bytecode** (a low-level, platform-independent instruction set) — you'll sometimes see this cached in `__pycache__/*.pyc` files.
2. The **Python Virtual Machine (PVM)** then interprets that bytecode, executing it one instruction at a time.

This hybrid model is why Python feels interpreted (run it directly, no compile step you have to invoke) while still benefiting from a compile phase that catches syntax errors before any code executes.

**Run-time system** refers to everything that supports a program while it executes: memory management (Python's automatic garbage collection), the call stack, exception handling machinery, and built-in services like the standard library. You don't write this — the language implementation provides it.

## 1e. Comparing Programming Languages

| | Python | Java | C++ |
|---|---|---|---|
| Execution model | Interpreted (bytecode + PVM) | Compiled to bytecode, run on JVM | Compiled directly to machine code |
| Typing | Dynamic, strong | Static, strong | Static, strong |
| Memory management | Automatic (garbage collected) | Automatic (garbage collected) | Manual (or smart pointers) |
| Typical use | Scripting, data science, web back ends, automation | Enterprise systems, Android apps | Systems programming, games, performance-critical software |
| Learning curve | Gentle | Moderate | Steep |

No language is universally "better" — the right choice depends on the constraints: development speed vs. runtime speed, safety vs. flexibility, team familiarity, ecosystem/library support.

## 1f. Installing a Programming Language (Python)

1. Download the installer from **python.org** (the official source — avoid third-party mirrors).
2. During installation on Windows, check **"Add Python to PATH"** so it can be run from any terminal.
3. Verify the install by opening a terminal and running:
   ```
   python --version
   ```
4. Choose a development environment:
   - **IDLE** — ships with Python, minimal but fine for small scripts and learning.
   - **VS Code** — lightweight, extensible, the most common professional choice.
   - **Spyder** — geared toward data science, includes a variable explorer similar to MATLAB.

---

# Unit 2 — Common Syntaxes, Variables & Data Types

## 2a. Common Syntaxes

Python's syntax philosophy is "readability counts" — this shows up in a few structural choices:

- **Indentation defines blocks**, not braces `{}`. This is not just a style convention — it is *mandatory*, part of the grammar. Mixing tabs and spaces, or indenting inconsistently within a block, is a syntax error.
- **No semicolons required.** A newline ends a statement. You *can* put a semicolon to fit two statements on one line (`x = 1; y = 2`), but it's discouraged.
- **Comments**: `#` for a single line; triple quotes `'''...'''` or `"""..."""` for multi-line (these are technically string literals that Python allows to sit unused as documentation).
- **Case sensitivity**: `Name`, `name`, and `NAME` are three distinct identifiers.
- **Colons introduce a block**: `if`, `for`, `while`, `def`, `class` all end their header line with `:`, followed by an indented body.

```python
# comment
def greet(name):        # colon starts the function body
    if name:             # colon starts the if body
        print(f"Hello, {name}")
    else:
        print("Hello, stranger")
```

## 2b. Variables

A variable in Python is really a **name bound to an object** — think of it as a label attached to a value living in memory, not a labeled box that *contains* the value. This distinction matters once you deal with mutable objects like lists.

```python
x = 10          # x is bound to the int object 10
x = "ten"        # x is now bound to a completely different object — no type declaration needed
```

Naming rules:
- Must start with a letter or underscore, followed by letters, digits, or underscores.
- Cannot be a reserved keyword (`if`, `for`, `class`, `def`, etc.).
- Case-sensitive.
- Convention (PEP 8): `snake_case` for variables and functions, `PascalCase` for classes, `UPPER_CASE` for constants.

Multiple assignment:
```python
x, y, z = 1, 2, 3      # unpacking — each name gets the corresponding value
a = b = c = 0            # chained assignment — all three names bind to the same object
```

**Scope** — where a variable is visible — matters even in Unit 2 material: a variable assigned inside a function is *local* to that function unless declared `global`. This becomes more important once you reach Unit 6 (functions and classes).

## 2c. Data Types

Python's built-in types split into two important categories: **mutable** (can be changed after creation) and **immutable** (cannot).

| Type | Example | Mutable? | Description |
|---|---|---|---|
| `int` | `5` | No | Arbitrary-precision whole numbers |
| `float` | `3.14` | No | Double-precision decimal numbers |
| `str` | `"hello"` | No | Immutable sequence of Unicode characters |
| `bool` | `True` | No | Subtype of `int` (`True == 1`, `False == 0`) |
| `list` | `[1, 2, 3]` | **Yes** | Ordered, indexable, resizable collection |
| `tuple` | `(1, 2, 3)` | No | Ordered, indexable, fixed-size collection |
| `dict` | `{"key": "value"}` | **Yes** | Key → value mapping, insertion-ordered since 3.7 |
| `set` | `{1, 2, 3}` | **Yes** | Unordered collection of unique, hashable items |
| `NoneType` | `None` | — | The single "no value" object |

Why mutability matters:
```python
a = [1, 2, 3]
b = a           # b now points to the SAME list object as a
b.append(4)
print(a)        # [1, 2, 3, 4]  — a changed too, because there was only ever one list

x = 5
y = x           # y gets a copy of the value (ints are immutable)
y += 1
print(x)        # 5 — unaffected
```

Checking and converting types:
```python
type(5)              # <class 'int'>
isinstance(5, int)    # True — preferred for conditional checks

int("5")               # 5
str(5)                 # "5"
float("3.14")          # 3.14
list("abc")             # ['a', 'b', 'c']
```

## 2d. Date and Time

The `datetime` module is the standard tool for representing and manipulating dates and times.

```python
from datetime import datetime, date, timedelta

now = datetime.now()          # current date + time
today = date.today()           # current date only

# Formatting — strftime() turns a datetime object into a string
now.strftime("%Y-%m-%d")           # "2026-09-04"
now.strftime("%B %d, %Y %H:%M")     # "September 04, 2026 14:30"

# Parsing — strptime() turns a string into a datetime object
parsed = datetime.strptime("2026-09-04", "%Y-%m-%d")

# Arithmetic
next_week = today + timedelta(days=7)
duration = next_week - today       # a timedelta object, duration.days == 7
```

Common format codes: `%Y` (4-digit year), `%m` (month), `%d` (day), `%H`/`%M`/`%S` (hour/minute/second), `%B` (full month name), `%A` (full weekday name).

## String

Strings are **immutable sequences of characters** — every operation that looks like it "modifies" a string actually builds and returns a new one.

```python
s = "Hello, World!"

len(s)                # 13
s[0]                    # "H"           — indexing
s[-1]                   # "!"           — negative indexing from the end
s[0:5]                  # "Hello"       — slicing (start inclusive, end exclusive)
s[::-1]                 # "!dlroW ,olleH" — reversed, via slice step

s.upper()               # "HELLO, WORLD!"
s.lower()                # "hello, world!"
s.strip()                # removes leading/trailing whitespace
s.replace("World", "Python")
s.split(",")              # ["Hello", " World!"]
"-".join(["a", "b", "c"]) # "a-b-c"
s.find("World")           # 7 (index) or -1 if not found
```

**f-strings** (formatted string literals, Python 3.6+) are the modern standard for building strings with embedded values:
```python
name, age = "Albert", 30
f"{name} is {age} years old"          # "Albert is 30 years old"
f"{3.14159:.2f}"                       # "3.14" — inline formatting spec
```

---

# Unit 3 — Control Structures

## 3a. Conditional Statements

Conditionals let a program branch based on a **Boolean expression** — anything that evaluates to `True` or `False`.

```python
5 > 3            # True
5 == 5            # True
5 != 3             # True
```

Operators split into two families:
- **Comparison**: `==`, `!=`, `<`, `>`, `<=`, `>=` — compare two values and produce a `bool`.
- **Logical**: `and`, `or`, `not` — combine or invert Boolean expressions.

Python also supports **truthiness**: any object can be evaluated in a Boolean context. Empty collections (`[]`, `{}`, `""`), `0`, and `None` are "falsy"; nearly everything else is "truthy." This lets you write `if my_list:` instead of `if len(my_list) > 0:`.

## 3b. The if / elif / else Statement

```python
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"
```

Mechanics that matter:
- Conditions are checked **top to bottom**; the first `True` branch runs and the rest are skipped — even if a later condition would also be `True`.
- `elif` is Python's contraction of "else if" — it avoids the nested-indentation mess you'd get chaining plain `if`/`else` blocks.
- `else` is optional; when present it catches everything not matched above.

**Nested if** — placing a conditional inside another — is used when a second condition only makes sense given the first:
```python
if age >= 18:
    if has_id:
        print("Entry allowed")
    else:
        print("ID required")
else:
    print("Too young")
```
Rule of thumb: use `elif` for mutually exclusive ranges of *one* variable; use nesting when the inner check only applies *within* the outer condition. Often, nesting can be flattened with `and`:
```python
if age >= 18 and has_id:
    print("Entry allowed")
```

## The Switch Statement

Python has no traditional `switch`/`case` block like C, Java, or JavaScript. Historically, Python developers used one of two workarounds:
1. An `if`/`elif`/`else` chain.
2. A dictionary mapping values to functions or results (a "dispatch table"):
```python
actions = {
    1: lambda: print("Monday"),
    2: lambda: print("Tuesday"),
}
actions.get(day, lambda: print("Invalid day"))()
```

**Since Python 3.10**, the `match` / `case` statement provides native structural pattern matching — a real switch equivalent, and more powerful:
```python
match day:
    case 1:
        print("Monday")
    case 2:
        print("Tuesday")
    case 6 | 7:                 # match multiple values with |
        print("Weekend")
    case _:                       # default case, like `default` elsewhere
        print("Invalid day")
```

`match` can also **destructure** data structures, which `switch` in other languages cannot do:
```python
point = (0, 5)
match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"On the y-axis at {y}")
    case (x, 0):
        print(f"On the x-axis at {x}")
    case (x, y):
        print(f"Point at ({x}, {y})")
```

---

# Unit 4 — Iterations

## 4a. Loop Structure

Python provides two loop constructs:
- **`for`** — iterates over a known sequence (list, string, range, etc.). Use it when you know *what* you're iterating over.
- **`while`** — repeats as long as a condition stays `True`. Use it when you know a *condition* to stop on, but not necessarily how many iterations that will take.

Loop control keywords:
- **`break`** — exits the loop immediately, skipping any remaining iterations.
- **`continue`** — skips the rest of the current iteration and moves to the next one.
- **`else`** (on a loop) — runs once, after the loop finishes *normally* (i.e., not via `break`). This is a Python-specific feature many other languages lack.

```python
for n in range(10):
    if n == 5:
        break
    print(n)
else:
    print("completed without break")   # skipped here, because break fired

for n in range(3):
    print(n)
else:
    print("completed without break")   # this DOES run — no break occurred
```

## 4b. Looping Procedures

**`for` over a sequence:**
```python
for char in "Python":
    print(char)

for fruit in ["apple", "banana", "cherry"]:
    print(fruit)
```

**`while` with a condition:**
```python
count = 0
while count < 5:
    print(count)
    count += 1
```

**Nested loops** — a loop inside another loop, common for grid/table-like data:
```python
for i in range(3):
    for j in range(2):
        print(i, j)
```
The inner loop completes fully for each single pass of the outer loop — a 3×2 nested loop like the one above runs the inner `print` 6 times total.

**Ascending vs. descending loops:**
```python
for i in range(0, 10):        # ascending: 0..9
    print(i)

for i in range(10, 0, -1):     # descending: 10..1 (step -1)
    print(i)
```

## Looping Techniques

Python's standard library offers tools that make loops more expressive than manual index tracking:

| Technique | Purpose | Example |
|---|---|---|
| `range(start, stop, step)` | Generate a numeric sequence lazily | `range(0, 10, 2)` → 0,2,4,6,8 |
| `enumerate(seq)` | Get index + value together | `for i, v in enumerate(fruits):` |
| `zip(a, b)` | Iterate multiple sequences in parallel | `for name, age in zip(names, ages):` |
| List comprehension | Build a list in one expression | `[x**2 for x in range(5)]` |
| Dict comprehension | Build a dict in one expression | `{x: x**2 for x in range(5)}` |
| `.items()` | Iterate dict keys and values together | `for k, v in d.items():` |

```python
# list comprehension with a filter condition
evens = [x for x in range(10) if x % 2 == 0]     # [0, 2, 4, 6, 8]

# generator expression — like a list comprehension but lazy (memory-efficient)
total = sum(x**2 for x in range(1000000))         # doesn't build the whole list in memory
```

`range()` is worth understanding deeply: it doesn't create a list of numbers in memory — it's a lazy sequence that produces values on demand. This is why `range(10_000_000)` is instant and memory-cheap, unlike `list(range(10_000_000))`.

---

# Unit 5 — Arrays

## 5a. One-Dimensional Arrays

Python has no dedicated "array" keyword like C or Java. The **list** fills that role for general-purpose use, and it's more flexible: it can hold mixed types and resizes automatically.

```python
numbers = [10, 20, 30, 40, 50]

numbers[0]         # 10   — first item
numbers[-1]         # 50   — last item, negative indexing
numbers[1:3]         # [20, 30] — slice (index 1 up to, not including, 3)

numbers.append(60)       # add to the end        — O(1) amortized
numbers.insert(0, 5)      # insert at a position   — O(n), shifts everything right
numbers.remove(30)        # remove by value         — O(n), searches then shifts
numbers.pop()              # remove & return the last item
len(numbers)                 # current size
```

For memory-efficient, **fixed-type** numeric arrays (closer to a true C-style array), Python's standard library offers the `array` module:
```python
from array import array
nums = array('i', [1, 2, 3])   # 'i' = signed int typecode; every element must be that type
```
For heavy numerical work, the third-party **NumPy** library (`numpy.array`) is the real-world standard — it stores data contiguously and supports vectorized math, which plain lists cannot do efficiently.

## 5b. Sorting and Searching in an Array

**Sorting:**
```python
nums = [5, 2, 9, 1, 7]

nums.sort()                       # sorts IN PLACE, mutates nums → [1, 2, 5, 7, 9]
sorted(nums, reverse=True)         # returns a NEW list, leaves nums untouched
nums.sort(key=lambda x: -x)         # custom sort key — here, descending order
```
Python's built-in sort (Timsort) runs in O(n log n) and is stable — equal elements keep their relative order, which matters when sorting by one key but wanting a secondary tiebreaker preserved.

**Linear search** — check each element until a match is found. Works on unsorted data. O(n) worst case.
```python
target = 7
for i, v in enumerate(nums):
    if v == target:
        print(f"Found at index {i}")
        break
```

**Binary search** — repeatedly halves the search range by comparing against the middle element. Requires the data to be **already sorted**. O(log n) — dramatically faster on large datasets.
```python
import bisect
nums = [1, 2, 5, 7, 9]
index = bisect.bisect_left(nums, 7)     # 3
```
Manual implementation, to understand the mechanics:
```python
def binary_search(arr, target):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1     # not found
```
The trade-off: binary search is faster per lookup, but sorting first costs O(n log n) — worth it if you'll search the same data many times, not worth it for a single one-off lookup on unsorted data.

## 5c. Two- and Multidimensional Arrays

A **2D array** is a list of lists — the natural way to represent grids, matrices, and tables in Python.

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

grid[1][2]        # 6 — row 1, column 2
grid[0]             # [1, 2, 3] — the entire first row
```

Traversal uses nested loops — outer loop over rows, inner loop over each value in a row:
```python
for row in grid:
    for value in row:
        print(value, end=" ")
```

**Building a grid dynamically** — a common beginner mistake to avoid:
```python
# WRONG: all rows are the SAME list object (aliasing bug)
bad_grid = [[0] * 3] * 3
bad_grid[0][0] = 9
print(bad_grid)     # [[9,0,0],[9,0,0],[9,0,0]] — every row changed!

# CORRECT: a comprehension creates a distinct list per row
good_grid = [[0] * 3 for _ in range(3)]
good_grid[0][0] = 9
print(good_grid)     # [[9,0,0],[0,0,0],[0,0,0]] — only row 0 changed
```

**Multidimensional arrays** extend the same nesting idea further — a list of lists of lists, and so on:
```python
cube = [
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]],
]
cube[1][0][1]      # 6
```
Beyond 2–3 dimensions, plain nested lists get unwieldy and slow. **NumPy** arrays (`numpy.ndarray`) are the standard tool for genuine multidimensional numerical data — they support arbitrary dimensions, vectorized operations, and are dramatically faster for large datasets.

---

# Unit 6 — Classes / Functions

## 6a. Basic Class

A **class** is a blueprint for creating objects: it bundles **attributes** (data) and **methods** (behavior) into one unit. An **object** (or instance) is a concrete thing built from that blueprint.

```python
class Dog:
    def __init__(self, name):     # constructor — runs automatically on creation
        self.name = name             # an attribute, stored on this specific instance

    def bark(self):                 # a method — behavior tied to the object
        print(f"{self.name} says woof!")

d = Dog("Rex")        # creates a new Dog object, calling __init__ automatically
d.bark()                 # "Rex says woof!"
```

Key mechanics:
- **`self`** is the instance the method is being called on. It must be the first parameter of every instance method, though Python passes it automatically — you never write `d.bark(d)`.
- **`__init__`** is a "dunder" (double-underscore) method — Python's convention for special, language-recognized methods. It initializes a new object's state right after creation.
- Each object has its **own** copy of instance attributes — changing `d.name` never affects a different `Dog` instance.

## 6b. Class with Parameters

Constructors aren't limited to one argument — they can accept as many as needed, including defaults:

```python
class Rectangle:
    def __init__(self, w, h=1):     # h defaults to 1 if not provided
        self.w = w
        self.h = h

r1 = Rectangle(4, 5)     # w=4, h=5
r2 = Rectangle(3)          # w=3, h=1 (default)
```

## 6c. Class with Return Values

Methods behave like regular functions and can `return` a value computed from the object's own state:

```python
class Rectangle:
    def __init__(self, w, h):
        self.w = w
        self.h = h

    def area(self):
        return self.w * self.h

    def perimeter(self):
        return 2 * (self.w + self.h)

r = Rectangle(4, 5)
print(r.area())         # 20
print(r.perimeter())     # 18
```

## 6d. Complex Class

A "complex" class typically means one built using other classes as components — through **inheritance** or **composition**.

**Inheritance** — a subclass reuses and extends a parent class's behavior:
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Cat(Animal):                # Cat inherits from Animal
    def speak(self):                 # method OVERRIDING — Cat provides its own version
        super().speak()                # call the parent's version too, without duplicating it
        print(f"{self.name} says Meow")

c = Cat("Tom")
c.speak()
# Tom makes a sound
# Tom says Meow
```
`super()` gives access to the parent class's methods from inside the subclass — essential for extending behavior instead of copy-pasting it.

**Composition** — a class holds an instance of another class as an attribute, modeling "has-a" instead of "is-a":
```python
class Engine:
    def start(self):
        print("Engine starting...")

class Car:
    def __init__(self):
        self.engine = Engine()      # a Car HAS an Engine

    def start(self):
        self.engine.start()
        print("Car ready to drive")
```
Rule of thumb: use inheritance when a subclass genuinely **is a** more specific version of the parent (`Cat` is an `Animal`); use composition when one object **has a** part that isn't itself a specialization (`Car` has an `Engine`, but a `Car` is not a kind of `Engine`).

---

# Unit 7 — File Management

## 7a. Managing External Files

`open()` connects your program to a file on disk. It takes a path and a **mode** that controls what you can do:

| Mode | Meaning |
|---|---|
| `'r'` | Read (default) — file must already exist |
| `'w'` | Write — creates the file if missing, **overwrites** if it exists |
| `'a'` | Append — creates the file if missing, adds to the end if it exists |
| `'x'` | Exclusive create — fails if the file already exists |
| `'r+'` | Read and write, without truncating |
| `'b'` suffix | Binary mode, e.g. `'rb'`, `'wb'` — for non-text files |

The recommended pattern uses `with`, which guarantees the file is closed even if an error occurs partway through:
```python
with open("notes.txt", "r") as f:
    content = f.read()
# file is automatically closed here, even if an exception was raised inside the block
```
The manual alternative is error-prone because a forgotten `.close()` can leak file handles or leave writes unflushed:
```python
f = open("notes.txt", "r")
content = f.read()
f.close()          # easy to forget, especially if an exception happens before this line
```

## 7b. Reading and Writing to a File

**Reading** — three main methods, each suited to a different situation:
```python
with open("notes.txt", "r") as f:
    whole = f.read()          # entire file as one string — fine for small files

with open("notes.txt", "r") as f:
    one_line = f.readline()    # just the next line, including its \n

with open("notes.txt", "r") as f:
    all_lines = f.readlines()   # a list of strings, one per line

with open("notes.txt", "r") as f:
    for line in f:                # memory-efficient — reads one line at a time
        print(line.strip())         # .strip() removes the trailing newline
```
For large files, iterating line by line (the last example) is far more memory-efficient than `.read()` or `.readlines()`, which load the entire file at once.

**Writing:**
```python
with open("notes.txt", "w") as f:
    f.write("Hello, file!\n")            # writes a single string
    f.writelines(["line1\n", "line2\n"])  # writes a list of strings, no automatic newlines added
```
`'w'` mode **erases existing content** the moment the file is opened — a common source of accidental data loss. If you want to keep existing content, use `'a'` (append) instead.

## 7c. Appending and Deleting Content of a File

**Appending** — adds to the end without disturbing what's already there:
```python
with open("notes.txt", "a") as f:
    f.write("New line added\n")
```

**Deleting part of a file's content** — Python has no direct "delete these bytes" operation. The standard approach is: read everything in, modify the data in memory, then rewrite the file:
```python
with open("notes.txt", "r") as f:
    lines = f.readlines()

lines = [line for line in lines if "unwanted" not in line]   # filter out unwanted lines

with open("notes.txt", "w") as f:
    f.writelines(lines)
```

**Deleting an entire file** — use the `os` module, not a file-open mode:
```python
import os
if os.path.exists("old_notes.txt"):
    os.remove("old_notes.txt")
```

**Clearing a file's contents while keeping the file itself:**
```python
with open("notes.txt", "w") as f:
    f.truncate()          # opening in 'w' mode already truncates; explicit call for clarity
```

---

## Quick-Reference Summary

| Unit | Core Takeaway |
|---|---|
| 1. Introduction | Languages differ by paradigm, typing, and execution model; Python compiles to bytecode then interprets it |
| 2. Syntax/Variables/Types | Indentation defines blocks; variables are names bound to objects; know which types are mutable |
| 3. Control Structures | `if/elif/else` for branching; `match/case` is Python's structural "switch" |
| 4. Iterations | `for` for known sequences, `while` for conditions; `enumerate`/`zip`/comprehensions make loops expressive |
| 5. Arrays | Lists are Python's array; know Big-O for sort/search; avoid the `[[0]*n]*n` aliasing trap |
| 6. Classes/Functions | `__init__` initializes state; `self` ties methods to instances; inheritance vs. composition |
| 7. File Management | Always prefer `with`; know your file modes; `'w'` overwrites, `'a'` appends |
