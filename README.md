# Problem Solving and Python Programming complete notes in simple words, 

## This document covers every topic listed in the syllabus for **Problem Solving and Python Programming**, with theory explained through runnable Python code, algorithms written before each practice solution, and fully commented implementations. Feel free to fork, edit, and add your own notes as you go through the semester  that's the whole point of putting this on GitHub.

> Prepared for: IT Engineering, JSSSTU Mysuru
> Covers all 5 units of the syllabus with theory, diagrams (in text form), Python code for every concept, and unit-end practice questions with algorithm + fully commented code.
> Every code example is written and tested to run in Python 3.

---

## Table of Contents

1. [Unit 1: Introduction to Digital Computer & Problem Solving Strategies](#unit-1)
2. [Unit 2: Introduction to Python](#unit-2)
3. [Unit 3: Functions](#unit-3)
4. [Unit 4: Strings and Lists](#unit-4)
5. [Unit 5: Tuples, Dictionaries, Classes and Objects](#unit-5)

---

<a name="unit-1"></a>
# UNIT 1: Introduction to Digital Computer & Problem Solving Strategies

## 1.1 Introduction to Digital Computer

A **computer** is an electronic device that takes input, processes it according to a set of instructions, and produces output. It works on the basic cycle:

```
INPUT  --->  PROCESS  --->  OUTPUT
              ^     |
              |     v
            STORAGE (feedback / memory)
```

Every computer, no matter how complex, is built around this simple idea. What separates a calculator from a supercomputer is *how much* it can store and *how fast* it can process — not what it fundamentally does.

## 1.2 Von Neumann Concept (Stored Program Concept)

Before John von Neumann's architecture (1945), computers had their instructions physically wired in (like old plugboard machines) — reprogramming meant re-wiring the hardware. Von Neumann proposed a radical simplification: **store the program in the same memory as the data**.

Key ideas of the Von Neumann architecture:
- A single memory holds both **instructions** and **data** (as binary numbers).
- The **CPU** fetches instructions from memory one at a time, decodes them, and executes them — this is called the **Fetch-Decode-Execute cycle**.
- Because instructions live in memory like data, a program can be loaded, changed, or even modify itself, without rewiring hardware.

```
 ┌─────────────┐        ┌───────────────────────┐
 │   MEMORY    │<------>│          CPU          │
 │ (Program +  │        │  ┌─────┐   ┌────────┐ │
 │  Data)      │        │  │ CU  │   │  ALU   │ │
 └─────────────┘        │  └─────┘   └────────┘ │
        ^                └───────────────────────┘
        |                         ^
        v                         |
 ┌─────────────┐          ┌───────────────┐
 │ Input Unit  │          │ Output Unit   │
 └─────────────┘          └───────────────┘
```

- **CU (Control Unit):** directs the fetch-decode-execute cycle, tells other parts what to do.
- **ALU (Arithmetic Logic Unit):** performs actual math and logic (add, compare, AND/OR, etc.).
- **Registers:** tiny, extremely fast storage inside the CPU used mid-calculation.

**Analogy in code** — Think of the CPU cycle like a `while` loop that never stops as long as the machine is on:

```python
# A toy simulation of the Fetch-Decode-Execute cycle
# "memory" holds tiny made-up instructions as tuples: (operation, operand)
memory = [
    ("LOAD", 5),     # load value 5 into accumulator
    ("ADD", 10),     # add 10 to accumulator
    ("PRINT", None), # print accumulator
    ("HALT", None)   # stop the CPU
]

accumulator = 0     # this represents a CPU register
program_counter = 0 # points to the current instruction (like an address)

running = True
while running:
    # FETCH: get the instruction at the program counter
    operation, operand = memory[program_counter]

    # DECODE + EXECUTE: figure out what to do and do it
    if operation == "LOAD":
        accumulator = operand
    elif operation == "ADD":
        accumulator += operand
    elif operation == "PRINT":
        print("Accumulator:", accumulator)
    elif operation == "HALT":
        running = False

    program_counter += 1  # move to the next instruction
```

This is exactly what a real CPU does, just at billions of cycles per second with real binary machine code instead of English words.

## 1.3 Storage (Memory Hierarchy)

Computers use a **hierarchy** of storage, trading off speed vs. size vs. cost:

| Level              | Speed        | Size        | Volatile? |
|--------------------|--------------|-------------|-----------|
| Registers          | Fastest      | Few bytes   | Yes       |
| Cache (L1/L2/L3)   | Very fast    | KB-MB       | Yes       |
| RAM (Main memory)  | Fast         | GBs         | Yes       |
| Secondary (SSD/HDD)| Slow         | TBs         | No        |

**Primary storage** (RAM) is fast but loses its contents when power is removed (**volatile**). **Secondary storage** (hard disks, SSDs) is slower but keeps data permanently (**non-volatile**).

## 1.4 Programming Languages

A programming language is a formal notation for writing instructions a computer can (eventually) execute. Languages exist on a spectrum:

- **Machine language:** raw binary (`1010 1100...`) — directly understood by hardware, extremely tedious for humans.
- **Assembly language:** human-readable mnemonics that map 1:1 to machine instructions (`MOV`, `ADD`, `JMP`).
- **High-level languages:** Python, C, Java, etc. — closer to human/mathematical thinking, hardware-independent.

```python
# High-level Python — readable and hardware independent
a = 5
b = 10
c = a + b
print(c)
```

```text
; The same idea in (illustrative) assembly-style pseudocode
MOV AX, 5
MOV BX, 10
ADD AX, BX
PRINT AX
```

Notice how much easier the Python version is to read — that's the entire point of "high-level".

## 1.5 Translators

A computer's CPU only understands machine code, so anything written in a high-level language must be **translated**. There are three main types of translators:

1. **Compiler:** translates the *entire* source program into machine code **before** execution, producing a standalone executable. (e.g., C, C++)
2. **Interpreter:** translates and executes the program **line by line**, without producing a separate executable file. (e.g., Python, in its default CPython form, is compiled to bytecode and then *interpreted* by the Python Virtual Machine)
3. **Assembler:** translates assembly language directly into machine code.

```
Compiler:     Source Code --> [Compiler] --> Machine Code (.exe) --> Run
Interpreter:  Source Code --> [Interpreter reads + executes line by line] --> Output directly
```

**Why this matters for Python specifically:** when you run `python program.py`, Python first compiles your source into an intermediate form called **bytecode** (`.pyc` files), then the **Python Virtual Machine (PVM)** interprets that bytecode. This is why Python is often called an interpreted language even though a compilation step is technically involved.

```python
# You can literally see Python's bytecode using the built-in dis module
import dis

def add(a, b):
    return a + b

dis.dis(add)
# Output shows the low-level bytecode instructions the PVM executes,
# e.g., LOAD_FAST, BINARY_ADD, RETURN_VALUE
```

## 1.6 Hardware and Software

- **Hardware:** the physical, touchable parts of a computer — CPU, RAM, hard disk, keyboard, monitor.
- **Software:** the set of instructions (programs) that tell hardware what to do. Software is further divided into:
  - **System software:** manages the hardware itself (Operating Systems, device drivers).
  - **Application software:** programs that solve user problems (browsers, word processors, games, and the Python scripts you'll write in this course).

```
                HARDWARE
                   ^
                   |
         (talks through) SYSTEM SOFTWARE  (Operating System)
                   ^
                   |
            APPLICATION SOFTWARE   (your Python programs, Chrome, MS Word...)
                   ^
                   |
                  USER
```

## 1.7 Operating Systems

The **Operating System (OS)** is system software that acts as a middleman between hardware and application software / the user. Its core jobs:

- **Process management:** deciding which program gets CPU time and when.
- **Memory management:** allocating RAM to running programs.
- **File management:** organizing data into files and folders on storage.
- **Device management:** handling input/output devices (keyboard, printer, etc.) via drivers.

Examples: Windows, Linux, macOS, Android.

```python
# Python programs interact with the OS all the time via the `os` module
import os

print("Current working directory:", os.getcwd())   # asks the OS where we are
print("List of files here:", os.listdir('.'))       # asks the OS for a file listing
print("OS name:", os.name)                           # 'posix' (Linux/Mac) or 'nt' (Windows)
```

---

## 1.8 Problem Solving Strategies

Before writing a single line of code, a programmer must **think**. The typical problem-solving pipeline looks like this:

```
Problem Statement --> Problem Analysis --> Algorithm --> Flowchart --> Code --> Testing
```

### 1.8.1 Problem Analysis

Problem analysis means breaking the problem statement into three clear parts:

1. **Inputs:** What data do we start with?
2. **Process:** What steps/operations transform the input?
3. **Output:** What result do we need to produce?

**Example problem:** *"Find the average of three numbers entered by the user."*

- **Input:** three numbers, say `a`, `b`, `c`
- **Process:** add them, divide the sum by 3
- **Output:** the average

### 1.8.2 Algorithms

An **algorithm** is a finite, well-defined, step-by-step procedure to solve a problem. A good algorithm is:

- **Definite** — each step is unambiguous.
- **Finite** — it terminates after a finite number of steps.
- **Effective** — each step can actually be carried out.
- **General** — it should ideally work for a class of problems, not just one specific input.

**Algorithm for finding the average of three numbers:**

```
Step 1: Start
Step 2: Read three numbers a, b, c
Step 3: Compute sum = a + b + c
Step 4: Compute average = sum / 3
Step 5: Display average
Step 6: Stop
```

**Corresponding Python code:**

```python
# Step-by-step translation of the algorithm above into Python

# Step 2: Read three numbers
a = float(input("Enter first number: "))
b = float(input("Enter second number: "))
c = float(input("Enter third number: "))

# Step 3: Compute the sum
total = a + b + c

# Step 4: Compute the average
average = total / 3

# Step 5: Display the result
print("The average is:", average)
```

### 1.8.3 Flowcharts

A **flowchart** is the *visual* counterpart of an algorithm — it uses standard symbols to represent the flow of logic:

| Symbol      | Meaning                          |
|-------------|-----------------------------------|
| Oval        | Start / End                       |
| Parallelogram | Input / Output                 |
| Rectangle   | Process (computation, assignment) |
| Diamond     | Decision (yes/no branching)       |
| Arrow       | Flow of control                   |

**Flowchart for the average-of-three-numbers algorithm** (text representation):

```
        ┌───────┐
        │ Start │
        └───┬───┘
            v
   ╱ Read a, b, c ╲       <- Parallelogram (input)
            v
   ┌──────────────────┐
   │ sum = a + b + c  │   <- Rectangle (process)
   └────────┬─────────┘
            v
   ┌──────────────────┐
   │ avg = sum / 3     │   <- Rectangle (process)
   └────────┬─────────┘
            v
   ╱ Display avg ╲          <- Parallelogram (output)
            v
        ┌───────┐
        │  End  │
        └───────┘
```

**Flowchart with a decision (diamond) — check if a number is even or odd:**

```
        ┌───────┐
        │ Start │
        └───┬───┘
            v
    ╱ Read number n ╲
            v
      ┌───────────┐
      │ n % 2 == 0│----NO----> ╱ Print "Odd" ╲---+
      └─────┬─────┘                              |
           YES                                   |
            v                                    |
     ╱ Print "Even" ╲                            |
            v                                    |
            +<-----------------------------------+
            v
        ┌───────┐
        │  End  │
        └───────┘
```

```python
# The even/odd flowchart translated directly into Python
n = int(input("Enter a number: "))

if n % 2 == 0:      # <- this is the diamond decision box
    print("Even")   # <- YES branch
else:
    print("Odd")     # <- NO branch
```

### 1.8.4 More Examples of Algorithms and Flowcharts

**Example: Algorithm + Flowchart + Code to find the largest of three numbers.**

Algorithm:
```
Step 1: Start
Step 2: Read a, b, c
Step 3: If a > b and a > c, then largest = a
Step 4: Else if b > c, then largest = b
Step 5: Else largest = c
Step 6: Display largest
Step 7: Stop
```

Flowchart:
```
        ┌───────┐
        │ Start │
        └───┬───┘
            v
   ╱ Read a, b, c ╲
            v
     ┌─────────────┐
     │ a>b AND a>c │--NO--+
     └──────┬──────┘      |
           YES             v
            v        ┌───────────┐
    largest = a       │  b > c ? │--NO--> largest = c
            |          └────┬─────┘             |
            |               YES                 |
            |                v                   |
            |         largest = b                |
            |                |                   |
            +----------------+-------------------+
                             v
                    ╱ Display largest ╲
                             v
                         ┌───────┐
                         │  End  │
                         └───────┘
```

```python
# Code for finding the largest of three numbers
a = float(input("Enter first number: "))
b = float(input("Enter second number: "))
c = float(input("Enter third number: "))

if a > b and a > c:
    largest = a
elif b > c:
    largest = b
else:
    largest = c

print("The largest number is:", largest)
```

---

## Unit 1 — Practice Questions

For each question: a short **algorithm** first (the plan), then a fully **commented Python solution** (the implementation).

### Q1. Write an algorithm and program to check whether a given year is a leap year.

**Algorithm:**
```
Step 1: Start
Step 2: Read year
Step 3: If year is divisible by 4 and not divisible by 100, OR divisible by 400 -> leap year
Step 4: Else -> not a leap year
Step 5: Display result
Step 6: Stop
```

```python
# Program: Check if a year is a leap year
year = int(input("Enter a year: "))   # take year as input

# A year is a leap year if:
#  - divisible by 4 AND not divisible by 100, OR
#  - divisible by 400
is_leap = (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)

if is_leap:
    print(year, "is a leap year")
else:
    print(year, "is not a leap year")
```

### Q2. Write an algorithm and program to compute simple interest.

**Algorithm:**
```
Step 1: Start
Step 2: Read principal (p), rate (r), time (t)
Step 3: Compute SI = (p * r * t) / 100
Step 4: Display SI
Step 5: Stop
```

```python
# Program: Compute Simple Interest
principal = float(input("Enter principal amount: "))
rate = float(input("Enter rate of interest (%): "))
time = float(input("Enter time period (years): "))

# Simple Interest formula: SI = (P * R * T) / 100
simple_interest = (principal * rate * time) / 100

print("Simple Interest =", simple_interest)
```

### Q3. Write an algorithm and program to swap two numbers without using a third variable.

**Algorithm:**
```
Step 1: Start
Step 2: Read a, b
Step 3: a = a + b
Step 4: b = a - b   (this now equals the original a)
Step 5: a = a - b   (this now equals the original b)
Step 6: Display a, b
Step 7: Stop
```

```python
# Program: Swap two numbers without a temporary variable
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))

print("Before swapping: a =", a, ", b =", b)

# Arithmetic trick to swap without a third variable
a = a + b   # a now holds sum of both
b = a - b   # subtract original b to recover original a into b
a = a - b   # subtract new b (original a) from sum to recover original b into a

print("After swapping: a =", a, ", b =", b)

# NOTE: Python actually allows direct tuple swapping: a, b = b, a
# but the arithmetic trick above demonstrates the classic algorithmic idea
# taught in this unit (no extra memory location used).
```

### Q4. Write an algorithm, flowchart (described), and program to find the factorial of a number using a loop.

**Algorithm:**
```
Step 1: Start
Step 2: Read n
Step 3: Set fact = 1, i = 1
Step 4: Repeat while i <= n:
             fact = fact * i
             i = i + 1
Step 5: Display fact
Step 6: Stop
```

Flowchart sketch:
```
Start -> Read n -> fact=1, i=1 -> [i<=n?] --NO--> Display fact -> End
                                    | YES
                                    v
                          fact = fact*i, i = i+1  --(loop back to check)
```

```python
# Program: Factorial of a number using a while loop
n = int(input("Enter a number: "))

fact = 1   # accumulator for the product
i = 1      # loop counter

while i <= n:        # repeat until i exceeds n
    fact = fact * i  # multiply running factorial by i
    i += 1           # move to the next number

print("Factorial of", n, "is", fact)
```

### Q5. Write an algorithm and program to check whether a number is prime.

**Algorithm:**
```
Step 1: Start
Step 2: Read n
Step 3: If n <= 1, print "Not Prime" and stop
Step 4: Set is_prime = True
Step 5: For i from 2 to n-1:
             If n % i == 0, set is_prime = False, break
Step 6: If is_prime, print "Prime", else print "Not Prime"
Step 7: Stop
```

```python
# Program: Check whether a number is prime
n = int(input("Enter a number: "))

if n <= 1:
    print(n, "is not prime")
else:
    is_prime = True                 # assume prime until proven otherwise
    for i in range(2, n):            # check divisibility by every number below n
        if n % i == 0:                # found a divisor other than 1 and n
            is_prime = False
            break                     # no need to check further

    if is_prime:
        print(n, "is prime")
    else:
        print(n, "is not prime")
```


---

<a name="unit-2"></a>
# UNIT 2: Introduction to Python

## 2.1 Introduction & Python Overview

**Python** is a high-level, interpreted, general-purpose programming language created by **Guido van Rossum**, first released in 1991. Its design philosophy emphasizes code readability (using indentation instead of curly braces `{}`), which is why Python code looks almost like structured English.

Key characteristics:
- **Interpreted:** no separate compile step needed to run a script directly.
- **Dynamically typed:** you don't declare variable types; Python figures it out at runtime.
- **Multi-paradigm:** supports procedural, object-oriented, and functional programming styles.
- **Batteries included:** a huge standard library ships with Python.

```python
# The most famous first program in any language
print("Hello, World!")
```

## 2.2 Getting Started with Python

Python code can be run in two main modes:

1. **Interactive mode:** type commands one at a time at the `>>>` prompt and see immediate results.
2. **Script mode:** write code in a `.py` file and run the whole file at once (`python filename.py`).

```python
# script_mode_example.py
# Everything in this file runs top-to-bottom when you execute:
#     python script_mode_example.py
print("This is script mode")
name = "Zash"
print("Hello,", name)
```

## 2.3 Comments

Comments are ignored by the interpreter — they exist purely for humans reading the code.

```python
# This is a single-line comment

"""
This is a multi-line comment (technically a string literal),
often used as a docstring at the start of a file, function, or class.
"""

x = 5  # inline comment explaining what x is used for
```

## 2.4 Python Identifiers

An **identifier** is the name given to variables, functions, classes, etc. Rules for valid identifiers:

- Must start with a letter (a-z, A-Z) or underscore `_`, never a digit.
- Can contain letters, digits, and underscores after the first character.
- Case-sensitive (`age` and `Age` are different identifiers).
- Cannot be a reserved keyword (see below).

```python
# Valid identifiers
student_name = "Zash"
_score = 95
total2 = 100

# Invalid identifiers (would cause SyntaxError if uncommented)
# 2total = 100     # cannot start with a digit
# student-name = 5 # hyphen not allowed
# class = "IT"     # 'class' is a reserved keyword
```

## 2.5 Reserved Keywords

Keywords are words that have a special meaning to Python and **cannot** be used as identifiers: `False, None, True, and, as, assert, async, await, break, class, continue, def, del, elif, else, except, finally, for, from, global, if, import, in, is, lambda, nonlocal, not, or, pass, raise, return, try, while, with, yield`.

```python
import keyword
print(keyword.kwlist)   # Python can print its own full keyword list for you
print(keyword.iskeyword("for"))    # True
print(keyword.iskeyword("total"))  # False
```

## 2.6 Variables

A **variable** is a named location in memory that stores a value. In Python, you don't declare a type — assignment creates the variable.

```python
age = 20          # an integer variable
price = 49.99     # a float variable
name = "Zash"     # a string variable
is_student = True # a boolean variable

# Python allows re-assigning a variable to a completely different type
age = "twenty"    # now age refers to a string instead of an int — perfectly legal
```

## 2.7 Standard Data Types

Python's built-in data types include:

| Type    | Example         | Description                          |
|---------|-----------------|---------------------------------------|
| int     | `42`            | whole numbers                         |
| float   | `3.14`          | decimal numbers                       |
| complex | `2 + 3j`        | complex numbers                       |
| bool    | `True`/`False`  | boolean (truth) values                |
| str     | `"hello"`       | text                                  |
| list    | `[1, 2, 3]`     | ordered, mutable sequence             |
| tuple   | `(1, 2, 3)`     | ordered, immutable sequence           |
| dict    | `{"a": 1}`      | key-value pairs                       |
| set     | `{1, 2, 3}`     | unordered collection of unique items  |

```python
a = 10          # int
b = 3.5         # float
c = 2 + 4j      # complex
d = True        # bool
e = "Python"    # str
f = [1, 2, 3]   # list
g = (1, 2, 3)   # tuple
h = {"key": "value"}  # dict
i = {1, 2, 3}   # set

# The type() function tells you a variable's current type
print(type(a), type(b), type(c), type(d), type(e), type(f), type(g), type(h), type(i))
```

## 2.8 Operators

Python supports several categories of operators:

```python
# Arithmetic operators
print(7 + 3)   # 10  addition
print(7 - 3)   # 4   subtraction
print(7 * 3)   # 21  multiplication
print(7 / 3)   # 2.333...  true division (always returns float)
print(7 // 3)  # 2   floor division (drops the decimal part)
print(7 % 3)   # 1   modulus (remainder)
print(7 ** 3)  # 343 exponentiation (7 to the power 3)

# Relational (comparison) operators -> always return a bool
print(5 == 5)  # True
print(5 != 4)  # True
print(5 > 3)   # True
print(5 < 3)   # False
print(5 >= 5)  # True
print(5 <= 4)  # False

# Logical operators -> combine boolean expressions
print(True and False)  # False (both must be True)
print(True or False)   # True  (at least one True)
print(not True)         # False (flips the value)

# Assignment operators -> shorthand for update-and-assign
x = 10
x += 5   # same as x = x + 5  -> 15
x -= 3   # same as x = x - 3  -> 12
x *= 2   # same as x = x * 2  -> 24
x //= 5  # same as x = x // 5 -> 4

# Bitwise operators -> operate on the binary representation
print(5 & 3)   # 1  AND
print(5 | 3)   # 7  OR
print(5 ^ 3)   # 6  XOR
print(~5)      # -6 NOT
print(5 << 1)  # 10 left shift
print(5 >> 1)  # 2  right shift

# Membership operators -> test if a value exists inside a sequence
print(3 in [1, 2, 3])       # True
print(9 not in [1, 2, 3])   # True

# Identity operators -> test if two names refer to the SAME object in memory
a = [1, 2]
b = [1, 2]
print(a == b)  # True  (equal contents)
print(a is b)  # False (different objects in memory)
```

## 2.9 Statement and Expression

An **expression** is anything that evaluates to a value (`3 + 4`, `x * 2`). A **statement** is a complete instruction that Python executes (an assignment, an `if`, a `for` loop, etc.) — a statement may *contain* expressions.

```python
# 3 + 4 is an EXPRESSION -> it evaluates to 7
# The whole line below is a STATEMENT (an assignment statement)
result = 3 + 4

print(result)   # print(...) is a statement whose argument is an expression
```

## 2.10 String Operations

Strings support concatenation, repetition, indexing, and many built-in methods (covered fully in Unit 4).

```python
first = "Hello"
second = "World"

# Concatenation with +
greeting = first + " " + second
print(greeting)              # Hello World

# Repetition with *
print("ab" * 3)                # ababab

# Indexing (0-based)
print(greeting[0])             # H
print(greeting[-1])            # d (negative index counts from the end)

# Basic built-in methods
print(greeting.upper())        # HELLO WORLD
print(greeting.lower())        # hello world
print(len(greeting))           # 11 (length of the string)
```

## 2.11 Boolean Expressions

A **Boolean expression** evaluates to either `True` or `False`. They are the backbone of decision-making in programs.

```python
age = 20
has_id = True

# Compound boolean expression using logical operators
can_vote = age >= 18 and has_id
print(can_vote)   # True

# Boolean expressions are often used directly inside 'if'
if age >= 18:
    print("Eligible to vote")
else:
    print("Not eligible to vote")
```

## 2.12 Control Statements

Control statements decide the order in which code executes.

### 2.12.1 `if`, `elif`, `else`

```python
marks = 72

if marks >= 90:
    grade = "A+"
elif marks >= 75:
    grade = "A"
elif marks >= 60:
    grade = "B"
else:
    grade = "C"

print("Grade:", grade)
```

### 2.12.2 `for` loop

```python
# for loop iterates over a sequence (here, a range of numbers)
for i in range(1, 6):     # produces 1, 2, 3, 4, 5
    print("Count:", i)

# for loop can also iterate directly over any collection
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

## 2.13 Iteration — `while` Statement

The `while` loop repeats a block of code **as long as** a condition remains `True`. Unlike `for`, it does not automatically know how many times to run — you control that yourself.

```python
# while loop: print numbers 1 to 5
i = 1
while i <= 5:
    print(i)
    i += 1   # CRITICAL: without this, the loop would run forever (infinite loop)

# while loop with a "sentinel value" to stop
total = 0
number = int(input("Enter a number (-1 to stop): "))
while number != -1:
    total += number
    number = int(input("Enter a number (-1 to stop): "))
print("Total entered:", total)

# break and continue inside loops
i = 0
while i < 10:
    i += 1
    if i == 5:
        continue   # skip printing 5, jump back to the condition check
    if i == 8:
        break       # stop the loop entirely once i reaches 8
    print(i)
```

## 2.14 Input from Keyboard

The built-in `input()` function reads a line of text typed by the user. It **always returns a string**, so numeric input must be explicitly converted.

```python
name = input("Enter your name: ")           # stays a string
age_text = input("Enter your age: ")         # this is a string like "20"
age = int(age_text)                          # convert to int for arithmetic

print(name, "will be", age + 1, "next year")

# Can be done in one line too
height = float(input("Enter your height in meters: "))
print("Your height in cm is:", height * 100)
```

---

## Unit 2 — Practice Questions

### Q1. Write an algorithm and program to classify a triangle based on the length of its sides (equilateral, isosceles, scalene).

**Algorithm:**
```
Step 1: Start
Step 2: Read sides a, b, c
Step 3: If a == b == c -> Equilateral
Step 4: Else if any two sides are equal -> Isosceles
Step 5: Else -> Scalene
Step 6: Display type
Step 7: Stop
```

```python
# Program: Classify a triangle by its side lengths
a = float(input("Enter side a: "))
b = float(input("Enter side b: "))
c = float(input("Enter side c: "))

if a == b == c:
    print("Equilateral Triangle")
elif a == b or b == c or a == c:
    print("Isosceles Triangle")
else:
    print("Scalene Triangle")
```

### Q2. Write an algorithm and program to print the multiplication table of a number using a `while` loop.

**Algorithm:**
```
Step 1: Start
Step 2: Read number n
Step 3: Set i = 1
Step 4: While i <= 10:
             Print n * i
             i = i + 1
Step 5: Stop
```

```python
# Program: Multiplication table using a while loop
n = int(input("Enter a number: "))

i = 1
while i <= 10:
    print(n, "x", i, "=", n * i)   # print each row of the table
    i += 1                          # move to the next multiplier
```

### Q3. Write an algorithm and program to reverse the digits of a given integer.

**Algorithm:**
```
Step 1: Start
Step 2: Read number n
Step 3: Set reversed = 0
Step 4: While n > 0:
             digit = n % 10
             reversed = reversed * 10 + digit
             n = n // 10
Step 5: Display reversed
Step 6: Stop
```

```python
# Program: Reverse the digits of an integer
n = int(input("Enter an integer: "))
original = n           # keep a copy for the final message
reversed_num = 0

while n > 0:
    digit = n % 10                     # extract the last digit
    reversed_num = reversed_num * 10 + digit  # shift and append the digit
    n = n // 10                         # remove the last digit from n

print("Reverse of", original, "is", reversed_num)
```

### Q4. Write an algorithm and program that reads marks in 5 subjects and determines pass/fail and grade using `if-elif`.

**Algorithm:**
```
Step 1: Start
Step 2: Read marks of 5 subjects
Step 3: Compute total and percentage
Step 4: If any subject < 35 -> Fail
Step 5: Else assign grade based on percentage ranges
Step 6: Display result
Step 7: Stop
```

```python
# Program: Grade calculator based on 5 subject marks
m1 = float(input("Enter marks of subject 1: "))
m2 = float(input("Enter marks of subject 2: "))
m3 = float(input("Enter marks of subject 3: "))
m4 = float(input("Enter marks of subject 4: "))
m5 = float(input("Enter marks of subject 5: "))

total = m1 + m2 + m3 + m4 + m5
percentage = total / 5

# Fail condition: any single subject below the passing threshold (35)
if m1 < 35 or m2 < 35 or m3 < 35 or m4 < 35 or m5 < 35:
    print("Result: FAIL")
else:
    if percentage >= 90:
        grade = "A+"
    elif percentage >= 75:
        grade = "A"
    elif percentage >= 60:
        grade = "B"
    elif percentage >= 35:
        grade = "C"
    print("Result: PASS")
    print("Percentage:", percentage)
    print("Grade:", grade)
```

### Q5. Write an algorithm and program to print the Fibonacci series up to n terms using a `while` loop.

**Algorithm:**
```
Step 1: Start
Step 2: Read n (number of terms)
Step 3: Set a = 0, b = 1, count = 0
Step 4: While count < n:
             Print a
             a, b = b, a + b
             count = count + 1
Step 5: Stop
```

```python
# Program: Fibonacci series up to n terms
n = int(input("Enter number of terms: "))

a, b = 0, 1     # first two Fibonacci numbers
count = 0

while count < n:
    print(a, end=" ")     # print current term without a newline
    a, b = b, a + b        # update: next pair of Fibonacci numbers
    count += 1

print()  # final newline for clean output
```


---

<a name="unit-3"></a>
# UNIT 3: Functions

## 3.1 Introduction

A **function** is a named, reusable block of code that performs a specific task. Functions let us avoid repeating ourselves (the **DRY principle: Don't Repeat Yourself**), break big problems into small manageable pieces, and make code easier to test and debug.

```python
# A simple function definition and call
def greet():
    print("Hello!")

greet()   # calling the function executes its body
greet()   # can be called as many times as needed
```

## 3.2 Built-in Functions

Python ships with many ready-to-use functions.

### 3.2.1 Type Conversion

Type conversion functions convert a value from one type to another **explicitly**.

```python
a = int("25")       # string -> int:   25
b = float("3.14")   # string -> float: 3.14
c = str(100)         # int -> string:  "100"
d = list("abc")      # string -> list: ['a', 'b', 'c']
e = bool(0)           # int -> bool:    False (0 is falsy)
f = bool(5)           # int -> bool:    True  (any nonzero number is truthy)

print(a, b, c, d, e, f)
```

### 3.2.2 Type Coercion

**Type coercion** is *implicit* conversion done automatically by Python during an operation (as opposed to type conversion, which is explicit and done by the programmer).

```python
x = 5        # int
y = 2.5      # float

# Python automatically "coerces" the int to a float here so the addition works
z = x + y
print(z, type(z))   # 7.5 <class 'float'>

# Booleans are coerced to integers in arithmetic (True=1, False=0)
print(True + True)   # 2
print(False + 5)      # 5
```

### 3.2.3 Mathematical Functions

```python
import math

print(abs(-7))          # 7        built-in: absolute value
print(round(3.567, 2))  # 3.57     built-in: rounding
print(pow(2, 5))         # 32       built-in: power (2^5)
print(max(4, 9, 2))       # 9        built-in: maximum
print(min(4, 9, 2))       # 2        built-in: minimum

print(math.sqrt(16))     # 4.0      from the math module: square root
print(math.floor(3.9))    # 3        round down
print(math.ceil(3.1))     # 4        round up
print(math.factorial(5))  # 120      5!
print(math.pi)             # 3.14159...
```

### 3.2.4 Date and Time

```python
import datetime

now = datetime.datetime.now()          # current date and time
print("Now:", now)

today = datetime.date.today()           # just today's date
print("Today:", today)

# Formatting dates for display
print(now.strftime("%d-%m-%Y %H:%M:%S"))

# Date arithmetic
future = today + datetime.timedelta(days=10)
print("10 days from today:", future)
```

### 3.2.5 The `dir()` Function

`dir()` lists all the attributes and methods available on an object — extremely useful for exploring what you can do with something.

```python
print(dir(str))   # lists every method available on strings, e.g. 'upper', 'lower', 'split'...

my_list = [1, 2, 3]
print(dir(my_list))  # lists list methods like 'append', 'sort', 'reverse'...
```

### 3.2.6 The `help()` Function

`help()` prints documentation for a function, module, or object — Python's built-in manual.

```python
help(print)     # shows how the print() function works and its parameters
help(str.upper) # shows documentation specific to the upper() string method
```

## 3.3 Composition of Functions

Function composition means using the **output of one function as the input to another**, building complex behavior from simple pieces.

```python
def square(x):
    return x * x

def add_one(x):
    return x + 1

# Composing: first square, then add one to the result
result = add_one(square(4))    # square(4) = 16, then add_one(16) = 17
print(result)   # 17

# Composition is common with built-in functions too
numbers = ["3", "1", "4", "1", "5"]
total = sum(map(int, numbers))   # map converts each string to int, sum adds them all
print(total)   # 14
```

## 3.4 User Defined Functions

Functions you write yourself, using the `def` keyword.

```python
def add_numbers(a, b):
    """Returns the sum of two numbers."""   # this is a docstring — describes the function
    result = a + b
    return result

sum_value = add_numbers(10, 20)
print(sum_value)   # 30
```

## 3.5 Parameters and Arguments

- **Parameters** are the variable names listed in the function definition.
- **Arguments** are the actual values passed when calling the function.

```python
def introduce(name, age):   # 'name' and 'age' are PARAMETERS
    print(f"My name is {name} and I am {age} years old.")

introduce("Zash", 19)       # "Zash" and 19 are ARGUMENTS

# Default parameter values
def introduce_with_default(name, age=18):
    print(f"My name is {name} and I am {age} years old.")

introduce_with_default("Ravi")          # uses the default age = 18
introduce_with_default("Ravi", 22)      # overrides the default

# Keyword arguments — pass by name instead of by position
introduce_with_default(age=25, name="Meera")

# Variable-length arguments
def total_of_many(*numbers):    # *numbers collects any number of positional args into a tuple
    return sum(numbers)

print(total_of_many(1, 2, 3, 4))   # 10

def print_details(**info):        # **info collects keyword args into a dictionary
    for key, value in info.items():
        print(key, "->", value)

print_details(name="Zash", branch="IT", year=1)
```

## 3.6 Function Calls

A function does nothing until it is **called** (invoked) using its name followed by parentheses.

```python
def square(n):
    return n * n

# Simply defining the function above does not execute it.
# Only calling it triggers execution:
print(square(5))    # this is the function call -> prints 25
```

## 3.7 The `return` Statement

`return` sends a value back to the caller and immediately ends the function's execution. A function without an explicit `return` implicitly returns `None`.

```python
def is_even(n):
    if n % 2 == 0:
        return True      # exits immediately with True
    return False          # only reached if the 'if' was False

print(is_even(4))   # True
print(is_even(7))   # False

def no_return_example():
    x = 10 + 5   # does something, but never returns a value

value = no_return_example()
print(value)   # None

# A function can return multiple values (packed as a tuple)
def min_max(numbers):
    return min(numbers), max(numbers)

low, high = min_max([4, 9, 1, 7])
print("Min:", low, "Max:", high)
```

---

## Unit 3 — Practice Questions

### Q1. Write an algorithm and function to check whether a number is a palindrome.

**Algorithm:**
```
Step 1: Start
Step 2: Define function is_palindrome(n)
Step 3: Reverse the digits of n
Step 4: Return True if reversed number equals original, else False
Step 5: Stop
```

```python
def is_palindrome(n):
    """Returns True if the integer n reads the same forwards and backwards."""
    original = n
    reversed_num = 0

    while n > 0:
        digit = n % 10                        # extract last digit
        reversed_num = reversed_num * 10 + digit  # build reversed number
        n = n // 10                            # drop last digit

    return reversed_num == original

# --- using the function ---
num = int(input("Enter a number: "))
if is_palindrome(num):
    print(num, "is a palindrome")
else:
    print(num, "is not a palindrome")
```

### Q2. Write an algorithm and function to compute the GCD (Greatest Common Divisor) of two numbers using the Euclidean algorithm.

**Algorithm:**
```
Step 1: Start
Step 2: Define function gcd(a, b)
Step 3: While b != 0:
             a, b = b, a % b
Step 4: Return a
Step 5: Stop
```

```python
def gcd(a, b):
    """Computes the GCD of a and b using the Euclidean algorithm."""
    while b != 0:
        a, b = b, a % b   # classic Euclidean step: replace (a, b) with (b, a mod b)
    return a

x = int(input("Enter first number: "))
y = int(input("Enter second number: "))
print("GCD of", x, "and", y, "is", gcd(x, y))
```

### Q3. Write an algorithm and function to check whether a number is an Armstrong number (e.g., 153 = 1^3 + 5^3 + 3^3).

**Algorithm:**
```
Step 1: Start
Step 2: Define function is_armstrong(n)
Step 3: Count number of digits, d
Step 4: For each digit, raise it to power d and sum
Step 5: Return True if sum equals original number
Step 6: Stop
```

```python
def is_armstrong(n):
    """Returns True if n is an Armstrong number."""
    original = n
    num_digits = len(str(n))   # e.g. 153 has 3 digits
    total = 0

    while n > 0:
        digit = n % 10
        total += digit ** num_digits   # raise each digit to the power of digit-count
        n = n // 10

    return total == original

value = int(input("Enter a number: "))
if is_armstrong(value):
    print(value, "is an Armstrong number")
else:
    print(value, "is not an Armstrong number")
```

### Q4. Write an algorithm and function to find the sum of digits of a number recursively (introducing recursive user-defined functions).

**Algorithm:**
```
Step 1: Start
Step 2: Define function digit_sum(n)
Step 3: Base case: if n == 0, return 0
Step 4: Recursive case: return (n % 10) + digit_sum(n // 10)
Step 5: Stop
```

```python
def digit_sum(n):
    """Recursively computes the sum of digits of n."""
    if n == 0:               # base case: stops the recursion
        return 0
    return (n % 10) + digit_sum(n // 10)   # recursive case: last digit + sum of the rest

num = int(input("Enter a number: "))
print("Sum of digits:", digit_sum(num))
```

### Q5. Write an algorithm and function that composes built-in and user-defined functions to find the average of even numbers in a list.

**Algorithm:**
```
Step 1: Start
Step 2: Define function average_of_evens(numbers)
Step 3: Filter out even numbers using a user-defined helper
Step 4: Use built-in sum() and len() to compute the average
Step 5: Return the average
Step 6: Stop
```

```python
def is_even(n):
    """Helper function: returns True if n is even."""
    return n % 2 == 0

def average_of_evens(numbers):
    """Returns the average of all even numbers in the given list."""
    evens = [n for n in numbers if is_even(n)]   # composition: uses is_even() inside a filter
    if len(evens) == 0:
        return 0   # avoid division by zero if there are no even numbers
    return sum(evens) / len(evens)   # composition of built-in sum() and len()

data = [12, 7, 18, 3, 9, 24, 5]
print("Average of even numbers:", average_of_evens(data))
```


---

<a name="unit-4"></a>
# UNIT 4: Strings and Lists

## 4.1 Strings

### 4.1.1 Strings as a Compound Datatype

A string is a **sequence** of characters — a "compound" datatype because it's built up of smaller units (individual characters) arranged in order.

```python
s = "Python"
# s is a single object, but it is composed of the characters: P, y, t, h, o, n
print(s)
```

### 4.1.2 `len()` Function

```python
s = "Programming"
print(len(s))   # 11 -> counts the number of characters
```

### 4.1.3 String Slices

Slicing extracts a **substring** using `string[start:stop:step]`. `start` is inclusive, `stop` is exclusive.

```python
s = "Hello World"

print(s[0:5])     # "Hello"      characters from index 0 up to (not including) 5
print(s[6:])       # "World"      from index 6 to the end
print(s[:5])       # "Hello"      from the start up to index 5
print(s[::-1])     # "dlroW olleH"  step -1 reverses the whole string
print(s[::2])      # "HloWrd"     every second character
```

### 4.1.4 Immutable Strings

Once created, a string's characters **cannot be changed in place**. Any "modification" actually creates a brand-new string object.

```python
s = "Hello"
# s[0] = "J"    # This would raise: TypeError: 'str' object does not support item assignment

# Instead, you build a NEW string:
s_new = "J" + s[1:]
print(s_new)    # "Jello" -- s itself is unchanged
print(s)         # still "Hello"
```

### 4.1.5 String Traversal

Traversal means visiting each character one at a time, usually with a `for` loop.

```python
s = "Python"

for ch in s:              # traverse character by character
    print(ch)

for i in range(len(s)):    # traverse using an index
    print(i, "->", s[i])
```

### 4.1.6 Escape Characters

Escape characters let you include special characters (like a newline or a quote) inside a string using a backslash `\`.

```python
print("Line1\nLine2")     # \n -> newline
print("Column1\tColumn2")  # \t -> tab
print("She said \"hello\"") # \" -> literal double quote inside a double-quoted string
print('It\'s sunny today')  # \' -> literal single quote inside a single-quoted string
print("Path: C:\\Users\\Zash")  # \\ -> a literal backslash
```

### 4.1.7 String Formatting Operator (`%`)

The `%` operator is the classic (older-style) way to format strings, similar to C's `printf`.

```python
name = "Zash"
age = 19

print("My name is %s and I am %d years old." % (name, age))
# %s -> string placeholder, %d -> integer placeholder

pi = 3.14159
print("Value of pi is %.2f" % pi)   # %.2f -> float rounded to 2 decimal places
```

### 4.1.8 String Formatting Functions

Modern Python favors `.format()` and **f-strings** (formatted string literals) for readability.

```python
name = "Zash"
age = 19

# .format() method
print("My name is {} and I am {} years old.".format(name, age))
print("My name is {0} and I am {1} years old. Hi {0}!".format(name, age))  # reuse by index

# f-strings (Python 3.6+, most modern and readable style)
print(f"My name is {name} and I am {age} years old.")
print(f"Next year I will be {age + 1} years old.")   # expressions allowed directly inside {}
```

---

## 4.2 Lists

### 4.2.1 Values and Accessing Elements

A **list** is an ordered, mutable collection that can hold items of any type.

```python
numbers = [10, 20, 30, 40, 50]

print(numbers[0])    # 10  -> first element (0-indexed)
print(numbers[-1])   # 50  -> last element
print(numbers[1:3])  # [20, 30] -> slicing works just like strings

mixed = [1, "two", 3.0, True]   # lists can hold mixed data types
print(mixed)
```

### 4.2.2 Mutable Lists

Unlike strings, lists **can** be changed in place after creation.

```python
fruits = ["apple", "banana", "cherry"]
fruits[1] = "blueberry"    # replace element at index 1
print(fruits)               # ['apple', 'blueberry', 'cherry']

fruits.append("mango")      # add a new item at the end
print(fruits)                # ['apple', 'blueberry', 'cherry', 'mango']
```

### 4.2.3 Traversing a List

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:              # traverse directly over elements
    print(fruit)

for index, fruit in enumerate(fruits):   # traverse with both index and value
    print(index, "->", fruit)
```

### 4.2.4 Deleting Elements from a List

```python
fruits = ["apple", "banana", "cherry", "mango"]

del fruits[0]              # deletes by index -> removes "apple"
print(fruits)                # ['banana', 'cherry', 'mango']

fruits.remove("cherry")     # deletes by value -> removes "cherry"
print(fruits)                 # ['banana', 'mango']

last_item = fruits.pop()     # removes and RETURNS the last item
print(last_item, fruits)      # mango ['banana']

fruits.clear()                 # removes everything
print(fruits)                   # []
```

### 4.2.5 Built-in List Operators

```python
a = [1, 2, 3]
b = [4, 5, 6]

print(a + b)         # [1, 2, 3, 4, 5, 6]  -> concatenation
print(a * 2)         # [1, 2, 3, 1, 2, 3]  -> repetition
print(3 in a)         # True                -> membership test
print(len(a))          # 3                   -> length
```

### 4.2.6 Built-in List Methods

```python
numbers = [5, 2, 8, 1, 9]

numbers.append(10)        # add to the end            -> [5, 2, 8, 1, 9, 10]
numbers.insert(0, 100)     # insert 100 at index 0      -> [100, 5, 2, 8, 1, 9, 10]
numbers.sort()               # sort ascending in place    -> [1, 2, 5, 8, 9, 10, 100]
numbers.reverse()            # reverse the list in place  -> [100, 10, 9, 8, 5, 2, 1]
print(numbers.index(8))       # find position of value 8
print(numbers.count(2))        # count how many times 2 appears
copy_list = numbers.copy()      # make a shallow copy
numbers.extend([200, 300])       # append multiple items at once

print(numbers)
print(copy_list)
```

---

## Unit 4 — Practice Questions

### Q1. Write an algorithm and program to check whether a given string is a palindrome (using slicing).

**Algorithm:**
```
Step 1: Start
Step 2: Read string s
Step 3: Reverse s using slicing s[::-1]
Step 4: If s == reversed(s), it's a palindrome
Step 5: Display result
Step 6: Stop
```

```python
# Program: Check whether a string is a palindrome
s = input("Enter a string: ")

reversed_s = s[::-1]   # slicing trick: step -1 reverses the string

if s == reversed_s:
    print(s, "is a palindrome")
else:
    print(s, "is not a palindrome")
```

### Q2. Write an algorithm and program to count vowels and consonants in a string.

**Algorithm:**
```
Step 1: Start
Step 2: Read string s, convert to lowercase
Step 3: Initialize vowel_count=0, consonant_count=0
Step 4: Traverse each character:
             If it's a letter and in "aeiou" -> vowel_count += 1
             Elif it's a letter -> consonant_count += 1
Step 5: Display counts
Step 6: Stop
```

```python
# Program: Count vowels and consonants in a string
s = input("Enter a string: ").lower()   # normalize to lowercase for easy comparison

vowel_count = 0
consonant_count = 0
vowels = "aeiou"

for ch in s:            # traverse the string character by character
    if ch.isalpha():       # only consider alphabetic characters
        if ch in vowels:
            vowel_count += 1
        else:
            consonant_count += 1

print("Vowels:", vowel_count)
print("Consonants:", consonant_count)
```

### Q3. Write an algorithm and program to find the second largest element in a list without using the built-in `sort()`.

**Algorithm:**
```
Step 1: Start
Step 2: Read list of numbers
Step 3: Initialize first = second = negative infinity
Step 4: Traverse the list:
             If number > first: second = first, first = number
             Elif number > second and number != first: second = number
Step 5: Display second
Step 6: Stop
```

```python
# Program: Find the second largest element in a list
numbers = [12, 45, 2, 41, 45, 9]

first = second = float('-inf')   # start with the smallest possible values

for num in numbers:        # traverse the list once (efficient, single pass)
    if num > first:
        second = first        # old largest becomes second largest
        first = num             # new largest found
    elif num > second and num != first:
        second = num             # found a new second-largest

print("Largest:", first)
print("Second largest:", second)
```

### Q4. Write an algorithm and program that removes duplicate elements from a list while preserving order.

**Algorithm:**
```
Step 1: Start
Step 2: Read list of elements
Step 3: Create an empty result list and an empty "seen" set
Step 4: For each element:
             If not in "seen": add to result and to seen
Step 5: Display result
Step 6: Stop
```

```python
# Program: Remove duplicates from a list while preserving order
original = [4, 5, 4, 2, 5, 9, 1, 2, 9]

seen = set()          # tracks elements we've already added
result = []             # final list without duplicates

for item in original:      # traverse original list once
    if item not in seen:     # only add if we haven't seen it before
        result.append(item)
        seen.add(item)

print("Original:", original)
print("Without duplicates:", result)
```

### Q5. Write an algorithm and program to count the frequency of each word in a sentence (combining string split and list/dict operations).

**Algorithm:**
```
Step 1: Start
Step 2: Read a sentence
Step 3: Split the sentence into words using split()
Step 4: For each word, increment its count in a dictionary
Step 5: Display word:count pairs
Step 6: Stop
```

```python
# Program: Count frequency of each word in a sentence
sentence = input("Enter a sentence: ").lower()

words = sentence.split()    # splits sentence into a list of words by whitespace

frequency = {}                # dictionary to hold word -> count

for word in words:              # traverse the list of words
    if word in frequency:
        frequency[word] += 1       # word already seen -> increment count
    else:
        frequency[word] = 1         # first time seeing this word

for word, count in frequency.items():
    print(word, ":", count)
```


---

<a name="unit-5"></a>
# UNIT 5: Tuples, Dictionaries, Classes and Objects

## 5.1 Tuples

### 5.1.1 Creating Tuples

A **tuple** is an ordered, **immutable** collection — once created, it cannot be changed. Tuples are written with parentheses `()` (though the parentheses are technically optional).

```python
t1 = (1, 2, 3)
t2 = 1, 2, 3          # parentheses are optional -> still a tuple
t3 = (5,)              # a single-element tuple NEEDS a trailing comma
t4 = ()                 # empty tuple
t5 = tuple([1, 2, 3])   # converting a list to a tuple

print(t1, t2, t3, t4, t5)
print(type(t3))          # <class 'tuple'>
```

### 5.1.2 Accessing Values in Tuples

```python
coordinates = (10, 20, 30)

print(coordinates[0])     # 10  -> indexing works just like lists
print(coordinates[-1])    # 30  -> negative indexing
print(coordinates[0:2])   # (10, 20) -> slicing returns a tuple
```

### 5.1.3 Immutable Tuples

```python
t = (1, 2, 3)
# t[0] = 99   # This raises: TypeError: 'tuple' object does not support item assignment

# To "change" a tuple, you must build a new one
t_new = (99,) + t[1:]
print(t_new)   # (99, 2, 3)

# NOTE: if a tuple contains a mutable object (like a list), THAT object can still be changed
t2 = (1, [2, 3], 4)
t2[1].append(100)   # allowed! We're mutating the LIST inside, not the tuple itself
print(t2)             # (1, [2, 3, 100], 4)
```

### 5.1.4 Tuple Assignment

Python allows **tuple unpacking** — assigning multiple variables from a tuple in one line.

```python
point = (3, 4)
x, y = point       # unpacking: x=3, y=4
print(x, y)

# Swapping variables using tuple assignment (very common Python idiom)
a, b = 5, 10
a, b = b, a          # right side builds a tuple (10, 5), then unpacks into a, b
print(a, b)            # 10 5
```

### 5.1.5 Basic Tuple Operations

```python
t1 = (1, 2, 3)
t2 = (4, 5, 6)

print(t1 + t2)     # (1, 2, 3, 4, 5, 6)  -> concatenation
print(t1 * 2)       # (1, 2, 3, 1, 2, 3)  -> repetition
print(3 in t1)       # True                 -> membership
print(len(t1))        # 3                    -> length
```

### 5.1.6 Built-in Tuple Functions

```python
t = (4, 1, 9, 2, 7)

print(len(t))       # 5   -> number of elements
print(max(t))        # 9   -> largest element
print(min(t))         # 1   -> smallest element
print(sum(t))          # 23  -> sum of elements
print(t.count(9))       # 1   -> occurrences of value 9
print(t.index(9))        # 2   -> position of first occurrence of value 9
print(sorted(t))          # [1, 2, 4, 7, 9] -> returns a NEW sorted LIST (tuple itself unaffected)
```

---

## 5.2 Dictionaries

### 5.2.1 Creating a Dictionary

A **dictionary** stores data as **key-value pairs**. Keys must be unique and immutable (strings, numbers, tuples); values can be anything.

```python
student = {
    "name": "Zash",
    "age": 19,
    "branch": "IT"
}

empty_dict = {}
another_way = dict(name="Meera", age=20)   # using the dict() constructor

print(student)
print(another_way)
```

### 5.2.2 Accessing Values

```python
student = {"name": "Zash", "age": 19, "branch": "IT"}

print(student["name"])          # "Zash" -> direct key access (KeyError if key missing)
print(student.get("age"))        # 19     -> safer access, returns None if missing
print(student.get("cgpa", "N/A")) # "N/A"  -> get() with a default fallback value

print(student.keys())             # dict_keys(['name', 'age', 'branch'])
print(student.values())            # dict_values(['Zash', 19, 'IT'])
print(student.items())              # dict_items([('name','Zash'), ('age',19), ('branch','IT')])
```

### 5.2.3 Updating a Dictionary

```python
student = {"name": "Zash", "age": 19}

student["age"] = 20                 # update an existing key's value
student["branch"] = "IT"             # add a brand-new key-value pair
student.update({"year": 1, "cgpa": 9.1})  # update/add multiple pairs at once

print(student)
```

### 5.2.4 Deleting Elements

```python
student = {"name": "Zash", "age": 19, "branch": "IT", "temp": "remove_me"}

del student["temp"]           # delete a specific key
removed_value = student.pop("age")   # remove a key AND get its value back
print(removed_value, student)

student.clear()                  # remove everything
print(student)                     # {}
```

---

## 5.3 Classes and Objects

### 5.3.1 Overview of OOP (Object-Oriented Programming)

OOP organizes code around **objects** — bundles of data (**attributes**) and behavior (**methods**) — rather than a sequence of instructions. Core ideas:

- **Class:** a blueprint/template describing what attributes and methods objects of this type will have.
- **Object:** a specific instance created from a class.
- **Encapsulation:** bundling data and the methods that operate on it together.

```
        CLASS "Student" (the blueprint)
        ┌─────────────────────────┐
        │ attributes: name, marks │
        │ methods: display()      │
        └─────────────────────────┘
                    |
        creates many independent OBJECTS
                    v
   ┌───────────┐        ┌───────────┐
   │ Object 1  │        │ Object 2  │
   │ name=Zash │        │ name=Ravi │
   │ marks=88  │        │ marks=76  │
   └───────────┘        └───────────┘
```

### 5.3.2 Class Definition

```python
class Student:
    """A simple class representing a student."""

    # The __init__ method is a special "constructor" — it runs automatically
    # whenever a new object of this class is created.
    def __init__(self, name, marks):
        self.name = name    # 'self.name' is an ATTRIBUTE belonging to this object
        self.marks = marks

    # A regular method — defines behavior that objects of this class can perform
    def display(self):
        print(f"Name: {self.name}, Marks: {self.marks}")

    def is_passed(self):
        return self.marks >= 35
```

### 5.3.3 Creating Objects

```python
# Creating (instantiating) objects from the Student class
s1 = Student("Zash", 88)
s2 = Student("Ravi", 30)

s1.display()          # Name: Zash, Marks: 88
s2.display()           # Name: Ravi, Marks: 30

print(s1.is_passed())    # True
print(s2.is_passed())     # False

# Each object has its OWN independent copy of the attributes
s1.marks = 95
print(s1.marks)   # 95
print(s2.marks)    # 30 -> unaffected by the change to s1
```

---

## Unit 5 — Practice Questions

### Q1. Write an algorithm and program to find the frequency of the maximum occurring element in a tuple.

**Algorithm:**
```
Step 1: Start
Step 2: Read a tuple of numbers
Step 3: For each unique number, count its occurrences
Step 4: Track the number with the highest count
Step 5: Display the number and its frequency
Step 6: Stop
```

```python
# Program: Find the most frequent element in a tuple
data = (4, 2, 4, 7, 2, 4, 9, 4)

max_count = 0
max_element = None

for item in set(data):           # set(data) gives us each unique value once
    count = data.count(item)       # built-in tuple method: count occurrences
    if count > max_count:
        max_count = count
        max_element = item

print("Most frequent element:", max_element, "-> appears", max_count, "times")
```

### Q2. Write an algorithm and program to merge two dictionaries, summing values for common keys.

**Algorithm:**
```
Step 1: Start
Step 2: Read dict1, dict2
Step 3: Create result as a copy of dict1
Step 4: For each key, value in dict2:
             If key exists in result, add value to it
             Else, add the key-value pair directly
Step 5: Display result
Step 6: Stop
```

```python
# Program: Merge two dictionaries, summing values for shared keys
dict1 = {"apple": 10, "banana": 5, "mango": 7}
dict2 = {"banana": 3, "mango": 2, "cherry": 8}

result = dict1.copy()     # start with a copy of the first dictionary

for key, value in dict2.items():   # traverse the second dictionary
    if key in result:
        result[key] += value          # key exists in both -> sum the values
    else:
        result[key] = value            # key only in dict2 -> add it fresh

print("Merged dictionary:", result)
```

### Q3. Write an algorithm and program using a class to model a Bank Account with deposit and withdraw methods.

**Algorithm:**
```
Step 1: Start
Step 2: Define class BankAccount with attribute balance
Step 3: Define method deposit(amount): balance += amount
Step 4: Define method withdraw(amount): if balance >= amount, subtract; else show error
Step 5: Create an account object and perform transactions
Step 6: Stop
```

```python
class BankAccount:
    """A simple class modeling a bank account."""

    def __init__(self, owner, balance=0):
        self.owner = owner        # attribute: who owns the account
        self.balance = balance      # attribute: current balance

    def deposit(self, amount):
        """Add money to the account."""
        self.balance += amount
        print(f"Deposited {amount}. New balance: {self.balance}")

    def withdraw(self, amount):
        """Remove money from the account, if sufficient funds exist."""
        if amount > self.balance:
            print("Insufficient balance!")
        else:
            self.balance -= amount
            print(f"Withdrew {amount}. New balance: {self.balance}")

    def display_balance(self):
        print(f"{self.owner}'s balance: {self.balance}")


# --- using the class ---
account = BankAccount("Zash", 1000)
account.display_balance()   # Zash's balance: 1000
account.deposit(500)          # Deposited 500. New balance: 1500
account.withdraw(2000)         # Insufficient balance!
account.withdraw(300)           # Withdrew 300. New balance: 1200
```

### Q4. Write an algorithm and program to convert a list of tuples into a dictionary and find the key with the maximum value.

**Algorithm:**
```
Step 1: Start
Step 2: Read a list of (key, value) tuples
Step 3: Convert the list to a dictionary using dict()
Step 4: Traverse the dictionary to find the key with the maximum value
Step 5: Display the result
Step 6: Stop
```

```python
# Program: Convert list of tuples to dict, then find the max-value key
data = [("apple", 30), ("banana", 55), ("mango", 42), ("cherry", 18)]

fruit_prices = dict(data)   # built-in conversion: list of pairs -> dictionary

max_key = None
max_value = float('-inf')

for key, value in fruit_prices.items():   # traverse the dictionary
    if value > max_value:
        max_value = value
        max_key = key

print("Prices:", fruit_prices)
print("Most expensive fruit:", max_key, "at", max_value)
```

### Q5. Write an algorithm and program using classes to model simple Rectangle objects and compare their areas.

**Algorithm:**
```
Step 1: Start
Step 2: Define class Rectangle with attributes length, width
Step 3: Define method area() returning length * width
Step 4: Create two Rectangle objects
Step 5: Compare their areas and display which is larger
Step 6: Stop
```

```python
class Rectangle:
    """A simple class representing a rectangle."""

    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        """Returns the area of the rectangle."""
        return self.length * self.width

    def perimeter(self):
        """Returns the perimeter of the rectangle."""
        return 2 * (self.length + self.width)


# --- using the class ---
r1 = Rectangle(10, 5)
r2 = Rectangle(7, 8)

print("Rectangle 1 area:", r1.area())
print("Rectangle 2 area:", r2.area())

if r1.area() > r2.area():
    print("Rectangle 1 has the larger area")
elif r2.area() > r1.area():
    print("Rectangle 2 has the larger area")
else:
    print("Both rectangles have equal area")
```

---

## End of Notes

