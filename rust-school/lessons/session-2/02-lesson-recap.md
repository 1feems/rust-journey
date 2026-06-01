# Lesson Recap #2: Rust Basics

Variables, types, functions, and loops are the building blocks of every Rust program — and Rust makes you be explicit about all of them.

---

## Key Takeaways

- Variables are immutable by default — add `mut` only when a value needs to change
- Types describe exactly what kind of value a variable holds — the compiler enforces this before anything runs
- Functions take typed inputs and return typed values — the last expression (no semicolon) is the return value
- `u64` is the correct type for SOL amounts — it cannot go negative like `i32` can
- Loop choice matters: `for` for known ranges, `while` for conditions, `loop` for manual control

---

## Why This Matters

Every Rust program — including every Solana smart contract — is built from these four ideas. Getting these solid means everything in Sessions 3 and 4 builds on something you already understand.

Immutability by default is not a limitation — it is a guarantee. When you see a variable without `mut`, you know nothing can change it unexpectedly. That makes code dramatically easier to reason about when real money is involved.

---

## Important Rules

- Variables are immutable by default — `mut` is required to change them
- `const` always requires a type annotation — Rust will not infer it
- Function return type must be declared with `->` if the function returns a value
- The last expression in a function — no semicolon — is the return value. Adding `;` makes it return nothing
- Range `1..5` stops before 5; `1..=5` includes 5
- `loop` runs forever — always pair it with a `break` condition

---

## Key Terms

| Term | Meaning |
|---|---|
| `let` | Declares a variable. Immutable by default |
| `mut` | Makes a variable mutable — allows its value to change |
| `const` | A constant value that never changes. Always needs a type annotation |
| Shadowing | Reusing a variable name by declaring a new binding with `let` — not the same as mutating |
| `i32` / `i64` | Signed integers — can be positive or negative |
| `u64` | Unsigned integer — can never be negative. Used for amounts like lamports |
| `bool` | A true or false value |
| `String` | Owned, growable text. Used for text you build or modify at runtime |
| `&str` | Borrowed text. Used for fixed string literals |

---

## Key Concepts

**Variables and Mutability:**
Variables are named containers for values. In Rust, they are immutable by default — the value cannot change unless you add `mut`.

```rust
let balance = 1000;       // immutable — cannot be reassigned
let mut balance = 1000;   // mutable — can be reassigned
balance = 1500;           // only works because of mut
```

---

**Constants vs Shadowing:**
`const` is for values that never change — always uppercase, always needs a type. Shadowing creates a new variable with the same name, not a mutation.

```rust
const MAX_SUPPLY: u64 = 1_000_000;

let a = 10;
let a = 12;   // shadows the original — a new binding, not a mutation
```

---

**Functions:**
Functions take typed parameters and return typed values. The last expression — no semicolon — is the return value.

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b   // no semicolon — this is the return value
}
```

---

**Loops:**
Three loop types for three different situations.

```rust
for i in 1..=5 { println!("{}", i); }   // runs exactly 5 times

let mut count = 0;
while count < 5 { count += 1; }         // runs while condition is true

loop { break; }                          // runs forever — must break manually
```

---

## Step-by-Step Walkthrough

Tracing what happens when `add(2, 3)` is called:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

1. Rust sees `add(2, 3)` — looks up the function definition
2. Assigns `a = 2`, `b = 3` inside the function's scope
3. Evaluates `a + b` → `5`
4. No semicolon on the last line → this is the return value
5. `5` is returned to the caller

Final state:

```rust
let result = add(2, 3);   // result = 5
```

---

## Common Confusions

* "Why does the function return nothing even though I wrote the value?"
  You probably added a semicolon to the last line. `a + b` returns the value. `a + b;` is a statement that returns nothing.

* "What is the difference between `const` and `let`?"
  `let` creates a variable — immutable by default but you can add `mut`. `const` is always immutable, always needs a type, and is for values that never change under any circumstance.

* "When do I use `String` vs `&str`?"
  `&str` for fixed text — labels, literals. `String` for text you build or change at runtime.

* "Why should I use `u64` for balances?"
  `i32` can go negative. A negative SOL balance makes no sense and can cause real bugs. `u64` makes a negative value impossible at the type level.

---

## Common Mistakes

- Forgetting `mut` when you need to reassign a variable
- Using `i32` for lamport or token amounts — use `u64` instead
- Adding a semicolon to the return expression — turns the value into a statement returning nothing
- Forgetting `->` return type in the function signature

These usually happen because Rust's rules about mutation and return values are stricter than most other languages.

---

# Source

- Recording: [Google Drive](https://drive.google.com/file/d/1jWbTlXqF8Z_VzeLJGISgqRaxj_aFt4dx/view?usp=sharing)
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-2-rust-basics.txt`
- Course: Rust School
- Session: 2
