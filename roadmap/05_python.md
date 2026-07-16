# 🐍 Python for DevOps & Platform Engineers

> **Goal:** Learn Python from scratch with a strong understanding of concepts first, then practice through progressively harder exercises, and finally apply to real DevOps automation.
>
> **Target:** 0–1 year → confident Python scripter building production automation tools
>
> **Philosophy:** Learn → Understand → Practice → Build → Review

---

## 📚 Resources

### Primary
- **Official Python docs:** https://docs.python.org/3/tutorial/
- **Real Python:** https://realpython.com/ (best tutorial site)
- **Python Cheatsheet:** https://www.pythoncheatsheet.org/

### Books
- *Python Crash Course* — Eric Matthes (best beginner book)
- *Automate the Boring Stuff with Python* — Al Sweigart (free online: https://automatetheboringstuff.com/)
- *Fluent Python* — Luciano Ramalho (advanced, essential for serious practitioners)

### Courses / Videos
- **freeCodeCamp Python full course:** https://youtu.be/rfscVS0vtbw
- **Corey Schafer Python Tutorials:** https://www.youtube.com/@coreyms
- **TechWorld with Nana — Python for DevOps:** https://youtu.be/t8pPdKYpowI
- **Sentdex Python Programming:** https://www.youtube.com/@sentdex

### Practice
- **LeetCode (Python):** https://leetcode.com/
- **HackerRank Python:** https://www.hackerrank.com/domains/python
- **Exercism Python track:** https://exercism.org/tracks/python
- **Codewars:** https://www.codewars.com/

### DevOps-Specific Python Libraries
| Library | Use | Docs |
|---|---|---|
| `requests` | HTTP clients | https://docs.python-requests.org/ |
| `boto3` | AWS SDK | https://boto3.amazonaws.com/v1/documentation/api/latest/ |
| `paramiko` | SSH automation | https://www.paramiko.org/ |
| `PyYAML` | YAML parsing | https://pyyaml.org/wiki/PyYAMLDocumentation |
| `prometheus_client` | Custom exporters | https://github.com/prometheus/client_python |
| `docker` | Docker SDK | https://docker-py.readthedocs.io/ |
| `kubernetes` | K8s client | https://github.com/kubernetes-client/python |
| `click` | CLI framework | https://click.palletsprojects.com/ |
| `loguru` | Better logging | https://loguru.readthedocs.io/ |
| `pydantic` | Data validation | https://docs.pydantic.dev/ |

---

## Stage 0 — Environment Setup

### Learn
- Installing Python 3, verifying with `python --version`
- Python REPL
- pip and virtual environments (`python -m venv`)
- VS Code + Python extension
- Black formatter + flake8 linting
- Debugger (pdb)

### Resources
- https://docs.python.org/3/library/venv.html
- **pip docs:** https://pip.pypa.io/en/stable/

### 🟢 Easy Exercises
1. Install Python 3, verify the version, and open the REPL — run `print("Hello DevOps")` and `2 ** 10`
2. Create a virtual environment, activate it, install `requests`, verify with `pip list`
3. Write a script that prints: Hostname, Username, Python version, Current time

### 🟡 Medium Exercises
1. Configure VS Code with Python extension, Black formatter (format on save), and flake8 linting
2. Set up a `.flake8` config file with `max-line-length = 100` and lint a file to zero warnings

### 🔴 Hard Exercises
1. Set up a project with `pyproject.toml`, `src/` layout, and editable install via `pip install -e .`
2. Configure pre-commit hooks that auto-format and lint on every commit

### 🏗 DevOps Project
Write a `system_info.py` script: prints Hostname, Username, Current Time, Python version, OS info — using `platform`, `datetime`, and `os` modules.

---

## Stage 1 — Variables & Data Types

### Learn
- Variables, dynamic typing
- Types: `int`, `float`, `bool`, `str`, `None`
- Type conversion: `int()`, `str()`, `float()`, `bool()`
- `type()` and `isinstance()`
- f-strings: `f"Hello {name}!"`
- Mutable vs immutable

### 🟢 Easy Exercises
1. Swap two variables without a temp variable (tuple unpacking)
2. Calculate area of a circle given radius as user input
3. Convert Celsius to Fahrenheit and print formatted to 2 decimal places
4. Convert seconds to hours, minutes, seconds (e.g., 3661 → 1h 1m 1s)

### 🟡 Medium Exercises
1. Build a BMI calculator: take weight and height as input, classify the result
2. Write a currency converter: USD ↔ EUR ↔ GBP ↔ INR
3. Calculate disk usage percentage: given total and used bytes, output as formatted string

### 🔴 Hard Exercises
1. Demonstrate mutable vs immutable: show modifying a list inside a function affects the caller but modifying an int does not
2. Build a shopping bill calculator: ask for item name, price, quantity in a loop, print itemized total

### 🏗 DevOps Project
Calculate disk usage percentage using `shutil.disk_usage()` — print used/total/free in GB and as percentage with color (red if > 90%).

---

## Stage 2 — Operators & Conditions

### Learn
- Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logical: `and`, `or`, `not`
- `if`, `elif`, `else`
- Ternary: `value if condition else other`
- `match`/`case` (Python 3.10+)

### 🟢 Easy Exercises
1. Check if a number is even or odd
2. Find the largest of three numbers
3. Check if a year is a leap year
4. Write a login validator: check username AND password

### 🟡 Medium Exercises
1. Build a grade calculator: given a score, print A/B/C/D/F
2. Build an ATM withdrawal: check balance, minimum amount, multiples of 10
3. Simulate CPU health check: if CPU > 80% print "High CPU Usage", else "Healthy"

### 🔴 Hard Exercises
1. Build a simple banking CLI: deposit, withdraw, balance — with validation
2. Write a firewall rule checker: given IP and port, match against allow/deny rules

### 🏗 DevOps Project
CPU/Memory health checker: read `/proc/loadavg` and `/proc/meminfo`, calculate current usage, print HEALTHY/WARNING/CRITICAL with thresholds.

---

## Stage 3 — Loops

### Learn
- `for` loop, `while` loop
- `break`, `continue`, `else` on loops
- `range()`, `enumerate()`, `zip()`

### 🟢 Easy Exercises
1. Print multiplication table for a given number (1–10)
2. Calculate factorial using a for loop
3. Check if a number is prime using a loop
4. Print first N Fibonacci numbers
5. Implement FizzBuzz — extend to support custom rules from a dict

### 🟡 Medium Exercises
1. Build a number-guessing game with limited attempts and hints
2. Loop through `["web1","web2","db1"]` and simulate ping (print ONLINE/OFFLINE)
3. Write a retry loop with exponential backoff (doubles delay, max 5 attempts)

### 🔴 Hard Exercises
1. Implement the Sieve of Eratosthenes and benchmark against naive prime checker
2. Build a chunked file reader: process large files in N-line batches using a while loop

---

## Stage 4 — Strings

### Learn
- Indexing, slicing `[start:end:step]`
- String methods: `.split()`, `.join()`, `.strip()`, `.replace()`, `.format()`, `.startswith()`, `.endswith()`
- f-strings (Python 3.6+)
- `str.format()` mini-language
- `re` module basics

### 🟢 Easy Exercises
1. Reverse a string using slicing
2. Count vowels in a string
3. Count words in a sentence
4. Check if a string is a palindrome (ignore case and spaces)

### 🟡 Medium Exercises
1. Check if two strings are anagrams
2. Count character frequency and print sorted
3. Parse a single nginx log line: extract IP, status code, URL

### 🔴 Hard Exercises
1. Find the longest substring without repeating characters
2. Write a regex-based log parser: extract IP, timestamp, method, URL, status, bytes from nginx combined format

### 🏗 DevOps Project
Nginx log analyzer: read access.log, parse each line with regex, count: total requests, unique IPs, top 10 URLs, error rate, requests by status code.

---

## Stages 5–21

> Stages 5–21 cover: Lists, Tuples & Sets, Dictionaries, Functions, Modules & Packages, Recursion, File Handling, Exception Handling, OOP, Iterators & Generators, Decorators, Context Managers, Standard Library, Networking & APIs, Concurrency & Async, Testing, and Capstone Projects.
>
> These stages are fully represented with 4-tier exercises (Basic → Expert) in the `index.html` exercise tracker. Open `index.html` in a browser to access the interactive exercise checklist.

---

## Key DevOps Python Projects (milestone builds)

### Beginner
- [ ] System info script (Stage 0 DevOps Project)
- [ ] Disk usage calculator (Stage 1)
- [ ] CPU/Memory health checker (Stage 2)
- [ ] Server ping monitor (Stage 3)
- [ ] Nginx log analyzer (Stage 4)
- [ ] Docker container list manager (Stage 5)

### Intermediate
- [ ] Log analyzer CLI (Stage 11)
- [ ] Retry API client with backoff (Stage 12)
- [ ] Backup script with S3 upload (Stage 11)
- [ ] SSH automation with paramiko (Stage 18)

### Advanced
- [ ] Docker health checker with SDK (Stage 19)
- [ ] Prometheus custom exporter (Stage 18)
- [ ] Kubernetes resource lister via API (Stage 18)
- [ ] AWS EC2 manager with boto3 (Stage 18)

### Expert
- [ ] Full monitoring CLI with subcommands (Stage 21)
- [ ] Incident response automator (Stage 21)
- [ ] Production backup + restore tool (Stage 21)
- [ ] GitOps sync validator (Stage 21)

---

## 🔗 Additional Python Resources

### Reference
- **Python Standard Library:** https://docs.python.org/3/library/
- **PyPI (package index):** https://pypi.org/
- **Python Packaging User Guide:** https://packaging.python.org/

### DevOps-Specific
- **boto3 examples:** https://boto3.amazonaws.com/v1/documentation/api/latest/guide/examples.html
- **Awesome Python (curated list):** https://github.com/vinta/awesome-python
- **Python for DevOps (book):** https://www.oreilly.com/library/view/python-for-devops/9781492057680/

---

*Progress: Use `index.html` to track exercises per stage · Est. time to complete: 8–14 weeks at 3 hrs/day*
