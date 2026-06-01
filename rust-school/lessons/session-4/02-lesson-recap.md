# Lesson Recap #4: Collections, Enums & Error Handling

Rust replaces null and exceptions with explicit types — `Option` when a value might not exist, `Result` when an operation might fail.

---

## Key Takeaways

- `Vec` stores growable lists of same-type values — use `.get()` for safe access instead of direct indexing
- `HashMap` stores key-value pairs — look up by key, not by position
- Enums name every possible state — `match` handles each one, and the compiler enforces completeness
- `Option` replaces null — `Some(value)` or `None`, always handled explicitly
- `Result` replaces exceptions — `Ok(value)` or `Err(reason)`, always handled explicitly

---

## Why This Matters

Real programs track lists of accounts, look up balances by name, and model whether a transaction succeeded or failed. `Vec`, `HashMap`, `Option`, and `Result` appear in nearly every Rust program — and in every Solana instruction handler.

The big idea: Rust makes failure visible in the type signature. A function that returns `Result<u64, String>` is telling you "this can fail." You cannot call it and ignore the failure — the compiler requires you to handle it.

---

## Important Rules

- All values in a `Vec` must be the same type
- `Vec` indexes start at zero — use `.get()` for safe access, not `v[i]`
- `HashMap` keys must be unique — inserting the same key twice overwrites the value
- `match` must be exhaustive — cover every variant or add `_` as a catch-all
- `Option` and `Result` must be matched before you can use the inner value
- Always `use std::collections::HashMap;` — it is not imported automatically

---

## Key Terms

| Term | Meaning |
|---|---|
| `match` | Checks a value against multiple patterns. Every possible case must be handled |
| Enum | A type that can be one of several named variants |
| `Vec<T>` | A growable list of same-type values |
| `HashMap<K, V>` | A key-value store — look up values by key |
| `Option<T>` | A value that is either `Some(value)` or `None`. Rust's replacement for null |
| `Some` | The `Option` variant that contains a value |
| `None` | The `Option` variant that contains nothing |
| `Result<T, E>` | Either `Ok(value)` on success or `Err(reason)` on failure |
| `Ok` | The success variant of `Result` |
| `Err` | The failure variant of `Result` |
| `panic!` | Immediately stops the program. For unrecoverable bugs only |

---

## Key Concepts

**`Vec<T>` — Growable Lists:**
A vector stores multiple values of the same type. Use `.get()` to access safely — it returns `Option` instead of panicking.

```rust
let mut wallets: Vec<String> = Vec::new();
wallets.push(String::from("Alice"));

match wallets.get(0) {
    Some(name) => println!("{}", name),
    None       => println!("not found"),
}
```

---

**`HashMap<K, V>` — Key-Value Lookup:**
Look up values by key. `.entry().or_insert()` inserts a default if the key doesn't exist.

```rust
use std::collections::HashMap;
let mut balances: HashMap<String, u64> = HashMap::new();
balances.insert(String::from("Alice"), 5000);
balances.entry(String::from("Bob")).or_insert(0);
```

---

**`Option<T>` — Values That Might Not Exist:**
Rust has no null. When a value might not be there, the type is `Option<T>`. You must handle both cases.

```rust
match balances.get("Carol") {
    Some(amount) => println!("Balance: {}", amount),
    None         => println!("Wallet not found"),
}
```

---

**`Result<T, E>` — Operations That Might Fail:**
Every Solana instruction handler returns `Result`. `Ok` means success, `Err` means failure with a reason.

```rust
fn withdraw(balance: u64, amount: u64) -> Result<u64, String> {
    if amount > balance {
        Err(String::from("Insufficient funds"))
    } else {
        Ok(balance - amount)
    }
}
```

---

## Step-by-Step Walkthrough

Tracing a balance lookup:

```rust
let result = balances.get("Alice");
```

1. `balances.get("Alice")` searches the map for the key `"Alice"`
2. Found → returns `Some(&5000)` — a reference to the value
3. Not found → returns `None`
4. `match result` forces you to handle both cases before the compiler is satisfied
5. You cannot use the value without first checking it exists

Final state:

```rust
// Some(&5000) if Alice exists
// None if she doesn't — handled either way
```

---

## Common Confusions

* "What is the difference between `Option` and `Result`?"
  `Option` is for values that might not exist — present or absent. `Result` is for operations that might fail — success or failure with a reason.

* "When do I use `panic!` vs `Result`?"
  `panic!` is for bugs — situations that should never happen. `Result` is for expected failures — things a normal user might trigger like an overdraft or missing account.

* "Why can't I just use `v[i]` to access a Vec?"
  `v[i]` panics if the index is out of bounds. `.get(i)` returns `Option` — you handle the missing case instead of crashing.

* "Why does HashMap need `use std::collections::HashMap`?"
  HashMap is not in Rust's prelude — the set of things automatically imported. You must bring it in explicitly.

---

## Common Mistakes

- Indexing a Vec directly with `v[i]` — panics if out of bounds. Use `.get(i)` instead
- Calling `.unwrap()` without being certain the value is `Some` — panics if it's `None`
- Forgetting `use std::collections::HashMap` before using HashMap
- Writing a non-exhaustive `match` — the compiler catches this, but it's worth knowing why

These usually happen because the explicit error-handling style is new — most languages let you ignore failures silently.

---

# Source

- Recording: [Google Drive](https://drive.google.com/file/d/1lEWFvHlaUxhzTcLFQje7kCbM6dkQ6KeN/view?usp=sharing)
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-4-collections-enums-error-handling.txt`
- Course: Rust School
- Session: 4
