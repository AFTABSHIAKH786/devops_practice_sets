# 🐍 Python Learning Roadmap for DevOps Engineers

> **Goal**
>
> Learn Python from the ground up with a strong understanding of concepts first, then practice them through progressively harder exercises, and finally apply them to real DevOps automation projects.
>
> **Target Audience**
>
> - DevOps Engineers
> - SREs
> - Cloud Engineers
> - Platform Engineers
> - Infrastructure Engineers
>
> **Philosophy**
>
> Learn → Understand → Practice → Build → Review
>
> **Never jump directly into solving exercises before learning the concept.**

---

# Learning Workflow

Every chapter follows exactly the same structure.

```
📖 Learn
        ↓
🧠 Understand
        ↓
💻 Guided Examples
        ↓
🟢 Easy Exercises
        ↓
🟡 Intermediate Exercises
        ↓
🔴 Challenge Problem
        ↓
🏗 DevOps Mini Project
        ↓
✅ Self Check
```

---

# Resources Used Throughout

## Official Documentation

https://docs.python.org/3/tutorial/

---

## Real Python

https://realpython.com/

---

## Python Crash Course

Book by Eric Matthes

---

## Automate the Boring Stuff

Book by Al Sweigart

https://automatetheboringstuff.com/

---

## Python Cheatsheet

https://www.pythoncheatsheet.org/

---

# Stage 0 — Environment Setup

## Learn

- Installing Python
- Running Python
- Python REPL
- pip
- Virtual Environments
- VS Code
- Debugger
- Formatting with Black
- Linting

---

## Practice

- Hello World
- User Input
- Print Formatting

---

## DevOps Project

Create a script that prints

- Hostname
- Username
- Current Time
- Python Version

---

## Self Check

Can I...

- Run a Python file?
- Install a package?
- Create a virtual environment?

---

# Stage 1 — Variables & Data Types

## Learn

- Variables
- int
- float
- bool
- string
- None
- Type Conversion

---

## Understand

- Mutable vs Immutable
- Dynamic Typing

---

## Guided Examples

- User input
- String formatting
- Type conversion

---

## Exercises

### Easy

- Swap variables
- Area of circle
- Celsius to Fahrenheit
- Seconds to Hours
- Currency conversion

### Medium

- Calculator
- BMI Calculator
- Percentage Calculator

### Challenge

Build a shopping bill calculator.

---

## DevOps Project

Calculate disk usage percentage.

---

## Self Check

Can I explain why

```python
input()
```

returns a string?

---

# Stage 2 — Operators & Conditions

## Learn

- Comparison Operators
- Logical Operators
- if
- elif
- else

---

## Exercises

### Easy

- Even Odd
- Largest Number
- Positive Negative
- Leap Year

### Medium

- Grade Calculator
- Login Validation
- ATM Withdrawal

### Challenge

Simple Banking CLI

---

## DevOps Project

If CPU > 80%

Print

```
High CPU Usage
```

Else

```
Healthy
```

---

# Stage 3 — Loops

## Learn

- for
- while
- break
- continue
- range()

---

## Exercises

### Easy

- Multiplication Table
- Factorial
- Prime Number
- Fibonacci
- Reverse Number

### Medium

- Guessing Game
- Number Patterns
- Star Patterns

### Challenge

FizzBuzz

---

## DevOps Project

Loop through

```
servers = [
    "web1",
    "web2",
    "db1"
]
```

Ping every server.

---

# Stage 4 — Strings

## Learn

- Indexing
- Slicing
- String Methods

---

## Exercises

### Easy

- Reverse String
- Count Vowels
- Count Words

### Medium

- Palindrome
- Anagram
- Character Frequency

### Challenge

Longest Unique Substring

---

## DevOps Project

Parse nginx logs.

Extract

- IP
- Status
- URL

---

# Stage 5 — Lists

## Learn

- Creating Lists
- Indexing
- Slicing
- Append
- Extend
- Remove
- Pop
- Sort

---

## Exercises

### Easy

- Largest Number
- Smallest Number
- Sum
- Average

### Medium

- Remove Duplicates
- Rotate List
- Merge Lists

### Challenge

Build your own Stack

---

## DevOps Project

Maintain a list of running Docker containers.

---

# Stage 6 — Tuples & Sets

## Learn

Tuples

Sets

Set Operations

---

## Exercises

### Easy

- Common Elements
- Unique Values

### Medium

- Set Difference
- Remove Duplicates

### Challenge

Mini Permission System

---

# Stage 7 — Dictionaries

## Learn

Dictionary Operations

Nested Dictionaries

---

## Exercises

### Easy

- Word Counter
- Student Marks

### Medium

- Frequency Counter
- Group Objects

### Challenge

Mini Inventory System

---

## DevOps Project

Represent

```
Server

↓

Services

↓

Status
```

using nested dictionaries.

---

# Stage 8 — Functions

## Learn

- Functions
- Parameters
- Return
- Scope
- *args
- **kwargs

---

## Exercises

### Easy

Calculator Functions

Prime Checker

Factorial

### Medium

Password Generator

Temperature Converter

### Challenge

CLI Calculator

---

## DevOps Project

Create reusable monitoring functions.

---

# Stage 9 — Modules & Packages

## Learn

- import
- from
- packages
- pip

---

## Exercises

Split a project into

```
utils.py

main.py

config.py
```

---

## DevOps Project

Create your own utilities package.

---

# Stage 10 — Recursion

## Learn

- Base Case
- Recursive Case
- Stack Frames

---

## Understand

When recursion should NOT be used.

---

## Guided Examples

- Countdown
- Factorial
- Sum
- Reverse String

---

## Exercises

### Easy

Factorial

Power

Countdown

### Medium

Palindrome

Binary Search

Directory Traversal

### Challenge

Flatten Nested List

Merge Nested Dictionary

Depth First Search

---

## DevOps Project

Walk through

```
/etc

/var

/home
```

recursively.

---

# Stage 11 — File Handling

## Learn

- Read
- Write
- Append
- CSV
- JSON

---

## Exercises

- Read Log File
- Count Lines
- Search Errors

---

## DevOps Project

Create a log analyzer.

---

# Stage 12 — Exception Handling

## Learn

- try
- except
- finally
- raise

---

## Exercises

- Safe Calculator
- File Validation

---

## DevOps Project

Retry failed API requests.

---

# Stage 13 — OOP

## Learn

- Classes
- Objects
- Inheritance
- Encapsulation
- Polymorphism

---

## Exercises

Build

- Car
- Bank Account
- Student

---

## DevOps Project

Create

```
Server

Database

DockerContainer

KubernetesPod
```

classes.

---

# Stage 14 — Iterators & Generators

## Learn

yield

Generators

---

## Exercises

Generate

- Fibonacci
- Infinite Counter

---

## DevOps Project

Stream log files.

---

# Stage 15 — Decorators

## Learn

- Function Wrappers
- Decorators

---

## Exercises

Create

```
@timer

@retry

@logger
```

---

## DevOps Project

Automatically log execution time.

---

# Stage 16 — Context Managers

## Learn

```
with
```

Custom Context Managers

---

## Exercises

Build

```
Database Connection

File Manager
```

---

# Stage 17 — Standard Library

## Learn

- pathlib
- os
- shutil
- subprocess
- logging
- argparse
- json
- re
- datetime

---

## Exercises

Create utilities using each module.

---

## DevOps Project

Build

```
backup.py

cleanup.py

health_check.py
```

---

# Stage 18 — Networking

## Learn

- requests
- APIs
- JSON

---

## Exercises

Call

GitHub API

Weather API

---

## DevOps Project

Monitor website uptime.

---

# Stage 19 — Concurrency

## Learn

- Threading
- Multiprocessing
- AsyncIO

---

## Exercises

Download multiple URLs.

Run multiple health checks.

---

## DevOps Project

Ping 100 servers concurrently.

---

# Stage 20 — Testing

## Learn

- unittest
- pytest
- mocking

---

## DevOps Project

Test your automation scripts.

---

# Stage 21 — Capstone Projects

## Beginner

- Calculator
- Todo CLI
- Password Generator
- Unit Converter

---

## Intermediate

- Log Analyzer
- File Organizer
- Backup Script
- SSH Automation

---

## Advanced

- Docker Health Checker
- Kubernetes Health Checker
- Prometheus API Client
- AWS EC2 Manager
- Terraform Wrapper

---

## Expert

- Full Monitoring CLI
- Deployment Automation
- Production Backup Tool
- Kubernetes Operator Helper
- Incident Response Automation

---

# Daily Study Routine

```
30 min

Read Concept

↓

20 min

Watch one tutorial

↓

30 min

Guided Examples

↓

45 min

Solve Exercises

↓

30 min

Build Something

↓

15 min

Review
```

Total

~2.5 Hours

---

# Rules

## Never copy solutions.

Think for at least 30 minutes.

---

## If stuck

1. Read the concept again.
2. Draw the logic on paper.
3. Try a simpler version.
4. Ask for hints, not answers.
5. Compare with the solution only after solving.

---

# Progress Checklist

For every topic, ask yourself:

- [ ] Can I explain this concept in my own words?
- [ ] Can I solve easy problems?
- [ ] Can I solve medium problems?
- [ ] Can I solve one challenge problem?
- [ ] Can I build something useful with it?
- [ ] Could I explain this in an interview?

---

# Final Goal

By the end of this roadmap, you should be able to confidently:

- Build production-quality Python scripts.
- Automate Linux administration tasks.
- Work with APIs.
- Build CLI tools.
- Process logs.
- Parse JSON/YAML.
- Interact with Docker.
- Interact with Kubernetes.
- Automate AWS tasks.
- Write monitoring and backup scripts.
- Build complete DevOps automation tools from scratch.

> **Remember:**
>
> Learning Python isn't about memorizing syntax.
> It's about learning how to think like a programmer.
> Once you can think in algorithms, the syntax becomes the easy part.