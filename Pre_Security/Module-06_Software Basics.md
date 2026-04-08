# Module - 06
# Software Basics — TryHackMe Pre Security

## Rooms Covered
- Data Representation
- Data Encoding
- Python: Simple Demo
- JavaScript: Simple Demo
- Database SQL Basics

---

## Data Representation

Computers only understand two states — 0 and 1. Everything a computer
processes, from colors to numbers to text, is ultimately stored as
combinations of these binary digits (bits).

**Colors — From 8 to 16 Million:**
- Each color on screen is a mix of Red, Green, and Blue (RGB)
- 1 bit per color = 2 states = only 8 total colors
- 8 bits per color = 256 levels per channel
- 256 × 256 × 256 = **16,777,216 colors** (true color)
- Each color is stored as 3 bytes (24 bits) total

**Hex Color Representation:**
- Reading 24 bits of binary is impractical — hexadecimal solves this
- Every 4 bits map to one hex digit (0–F)
- Example: green color `10100011 11101010 00101010` → `#A3EA2A`

**Number Systems:**

| System | Base | Digits Used | Groups |
|--------|------|-------------|--------|
| Decimal | 10 | 0–9 | — |
| Binary | 2 | 0, 1 | — |
| Hexadecimal | 16 | 0–9, A–F | 4 bits |
| Octal | 8 | 0–7 | 3 bits |

**Binary to Decimal Conversion (positional math):**
1001 = 1×2³ + 0×2² + 0×2¹ + 1×2⁰ = 8 + 0 + 0 + 1 = 9
1111 = 1×2³ + 1×2² + 1×2¹ + 1×2⁰ = 8 + 4 + 2 + 1 = 15

**Key Terms:**
- `Bit` — a single binary digit; either 0 or 1
- `Byte` — 8 bits grouped together (also called an octet)
- `Hexadecimal` — base-16 system; shorthand for reading binary data

**Practical answers:**
- Hex `FF` in binary: `1111 1111`
- Hex `AB` in decimal: `171`
- Color `#3BC81E` appears: **green**

---

## Data Encoding

Numbers alone mean nothing without an agreed-upon mapping. Encoding
is the standard that says "this number means this character."

**ASCII (American Standard Code for Information Interchange):**
- Created in 1963; covers English letters, digits, punctuation
- Uses 7 bits → 128 characters (0–127)
- Example: `A` = decimal 65 = hex `41` = binary `01000001`
- "TryHackMe" stored as: `54 72 79 48 61 63 6B 4D 65 0A` (hex)

**Limitations of ASCII:**
- Only covers English — no ñ, ß, ç, ł, Arabic, Japanese, Chinese
- ISO-8859 series tried to extend it with regional standards
- Problem: opening a file with the wrong encoding produces gibberish

**Unicode — The Universal Solution:**
- Assigns a unique code point to every character across all languages
- Covers 157,000+ characters including emoji sequences
- Examples:
  - `U+0041` = Latin "A"
  - `U+03A9` = Greek "Ω"
  - `U+3042` = Japanese "あ"
  - `U+0001F525` = 🔥

**UTF Encoding Standards:**

| Standard | Bytes Used | Notes |
|----------|-----------|-------|
| UTF-8 | 1–4 bytes | Most common on the web; backward compatible with ASCII |
| UTF-16 | 2 or 4 bytes | Used by Windows, Java internally |
| UTF-32 | Always 4 bytes | Simplest but most memory wasteful |

**Practical answers:**
- UTF-32 of 😌 = `U+0001F60C`
- UTF-16 of シ = `U+30B7`
- `U+0001F60E` = 😎

---

## Python: Simple Demo

Python is a high-level, general-purpose language. This room built a
"Guess the Number" game step by step, introducing three core
programming concepts.

**Key Concepts Learned:**

**1. Variables**
```python
import random

secret = random.randint(1, 20)  # pick random number 1–20
tries = 0
guess = 0

print("I'm thinking of a number between 1 and 20")
text = input("Take a guess: ")
guess = int(text)     # convert string input to integer
tries = tries + 1
```

**2. Conditional Statements (if / elif / else)**
```python
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

**3. Iteration (while loop)**
```python
while guess != secret:
    text = input("Take a guess: ")
    guess = int(text)
    tries = tries + 1
    # conditional block repeats until correct guess
```

**Python syntax notes:**
- `elif` = "else if"
- `!=` = "not equal to"
- Indentation defines code blocks (no curly braces)
- `input()` always returns a string — must use `int()` to convert

---

## JavaScript: Simple Demo

JavaScript powers web browsers and server-side apps via Node.js.
This room rebuilt the same "Guess the Number" game in JavaScript,
making it easy to compare the two languages side by side.

**Key Concepts Learned:**

**1. Variables and Constants**
```javascript
const secret = Math.floor(Math.random() * 20) + 1; // constant
let tries = 0;   // variable (can change)
let guess = 0;
```

- `const` — value cannot change after declaration
- `let` — value can change throughout the program
- `console.log()` — displays output to screen
- `parseInt(text, 10)` — converts string input to integer

**2. Conditional Statements (if / else if / else)**
```javascript
if (guess < 1 || guess > 20) {
    console.log("That number is out of range. Try again.");
} else if (guess < secret) {
    console.log("Too low, try again.");
} else if (guess > secret) {
    console.log("Too high, try again.");
} else {
    console.log("You got it in", tries, "tries!");
}
```

**3. Iteration (while loop)**
```javascript
while (guess !== secret) {
    const text = await rl.question("Take a guess: ");
    guess = parseInt(text, 10);
    tries = tries + 1;
    // conditional block inside
}
```

**JavaScript syntax notes:**
- `||` = "or" (vs Python's `or`)
- `!==` = "not equal to" (vs Python's `!=`)
- Curly braces `{}` define code blocks (vs Python's indentation)
- `await` pauses execution until user responds

**Python vs JavaScript — Quick Comparison:**

| Concept | Python | JavaScript |
|---------|--------|-----------|
| Variable | `x = 0` | `let x = 0` |
| Constant | — | `const x = 0` |
| Not equal | `!=` | `!==` |
| Or operator | `or` | `\|\|` |
| Output | `print()` | `console.log()` |
| Input | `input()` | `rl.question()` |
| Code blocks | Indentation | Curly braces `{}` |

---

## Database SQL Basics

As data grows, storing it in files becomes slow and unmanageable.
Databases solve this by organizing data in structured tables that can
be queried instantly.

**Core Concepts:**
- `Database` — organized digital storage for structured information
- `Table` — data arranged in rows and columns (like a spreadsheet)
- `Column` — defines one type of information (e.g., drink name, price)
- `Row` — one complete record (e.g., a single café order)
- `SQL` — Structured Query Language; used to ask questions of a database

**SQL Queries Learned:**

```sql
-- View all data in a table
SELECT * FROM Orders;

-- View specific columns only
SELECT drink, price FROM Orders;

-- Filter rows by condition
SELECT * FROM Orders WHERE drink = 'Coffee';

-- Sort results ascending (default)
SELECT * FROM Orders ORDER BY price;

-- Sort results descending
SELECT * FROM Orders ORDER BY price DESC;

-- Combine filter and sort
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;
```

**SQL Keyword Summary:**

| Keyword | Purpose |
|---------|---------|
| `SELECT` | Choose which columns to display |
| `FROM` | Specify which table to query |
| `WHERE` | Filter rows by condition |
| `ORDER BY` | Sort results; add `DESC` to reverse |

---

## How This Connects to My SOC Work

This module connects to my SOC work in more ways than it first appears.

**Data Representation & Encoding** are directly relevant to malware
analysis and log investigation. When I examine suspicious file hashes
in ArcSight, those MD5 and SHA256 values are hexadecimal — the same
base-16 system covered in this module. IOC feeds, memory dumps, and
packet captures all use hex notation. Understanding how ASCII and
Unicode encoding works also helps when analysing phishing emails with
encoded payloads or obfuscated URLs using percent-encoding.

**Python and JavaScript** are the two most common languages used in
SOC automation and security tooling. Python especially is everywhere —
SIEM integrations, log parsers, threat intel scripts, and automation
playbooks are almost all written in Python. Understanding variables,
conditionals, and loops means I can now read and understand detection
scripts and automation code rather than treating them as a black box.

**SQL** is directly applicable to my day-to-day work. Many SIEM
platforms, including ArcSight, store event data in relational
databases under the hood. Writing SQL-like queries is a skill used in
threat hunting — filtering logs by source IP, ordering events by
timestamp, and joining tables of user activity with alert data.
Database query logic also underpins many SOC dashboards and reports.

---

## Key Takeaway

Software is built on layers — binary at the bottom, encoding on top
of that, programming languages to manipulate it, and databases to
store it. A SOC analyst who understands these layers can read tool
output more accurately, write basic automation scripts, and understand
exactly what an attacker is doing when they manipulate data at any
of these levels. 

