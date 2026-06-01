# Study Guide — Rust School Session 4: Collections, Enums & Error Handling

> Use this after watching the class recording and reading the synthesis.
> This session gives you the four types that appear in virtually every Rust program — and every Solana instruction handler.

---

## 1. Lesson Recap

- `match` works like a switch statement but exhaustive — you must handle every possible case, or use `_` as a catch-all. Rust will not compile if you miss one
- `Vec<T>` stores a growable list of same-type values. Use `.get()` instead of `v[i]` for safe access — it returns `Option` instead of panicking on a bad index
- `String` vs `&str`: `String` is owned, heap-allocated, and can grow. `&str` is a borrowed reference to text stored in the binary. Use `String` for data you build or change at runtime
- `HashMap<K, V>` stores key-value pairs. Use `.entry(...).or_insert(...)` to set a default if the key does not exist yet
- `Option<T>` replaces `null` — a value is either `Some(value)` or `None`. You cannot use the inner value without handling both cases
- `Result<T, E>` replaces exceptions — an operation either returns `Ok(value)` or `Err(reason)`. Every Solana instruction handler returns `Result`

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| `match` | Checks a value against multiple patterns. Every case must be handled | Enums, Option, Result | Exhaustive — the compiler catches missed cases |
| Enum | A type that can be one of several named variants | `enum Color { Red, Blue }` | Models a fixed set of possible values |
| `Vec<T>` | A growable list of same-type values | Any ordered list of data | Safer and more flexible than arrays |
| `.push()` | Adds a value to the end of a `Vec` | `v.push(5);` | Main way to grow a Vec |
| `.get(i)` | Returns `Option` for index `i`. Does not panic | `v.get(0)` | Safe access — handles out-of-range without crashing |
| `HashMap<K, V>` | A key-value store. Look up values by key | Account balances, named configs | Fast lookup by key |
| `.entry().or_insert()` | Sets a default value if the key does not exist yet | Initializing account data | Does not overwrite existing values |
| `Option<T>` | A value that is either `Some(value)` or `None` | Any value that might not exist | Replaces null — forces you to handle both cases |
| `Some` | The `Option` variant that contains a value | `Some(42)` | Value is present |
| `None` | The `Option` variant that contains nothing | `None` | Value is absent |
| `Result<T, E>` | Either `Ok(value)` on success or `Err(reason)` on failure | Functions that can fail | Replaces exceptions — error is explicit |
| `Ok` | The `Result` variant for success | `Ok(new_balance)` | Operation worked |
| `Err` | The `Result` variant for failure | `Err(String::from("Insufficient funds"))` | Operation failed — reason included |
| `panic!` | Immediately stops the program | Programming bugs only | Not for expected errors — use `Result` instead |

---

## 3. Big Idea

`Option` eliminates null. `Result` eliminates silent failures. `Vec` and `HashMap` are the two main ways to store lists and lookups. `match` connects them all — it forces you to handle every case.

**Memory hook:** Rust makes every failure visible. You cannot ignore an `Option` or a `Result`.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| `match` | Check all possible values. Compiler requires exhaustiveness. | `match color { Color::Red => ..., Color::Blue => ... }` | `_` is the catch-all |
| `Vec<T>` | A list that can grow. All items are the same type. | `let mut v = Vec::new(); v.push(1);` | Use `.get()` for safe access |
| `HashMap<K, V>` | Look up a value by key. | `accounts.get("Alice")` | `.entry().or_insert()` for safe default |
| `Option<T>` | Value might or might not exist. | `.get(0)` returns `Option<&T>` | Must handle `Some` and `None` |
| `Result<T, E>` | Operation might succeed or fail. | `fn transfer(...) -> Result<u64, String>` | Must handle `Ok` and `Err` |
| Recoverable error | Expected failure — use `Result` | Insufficient funds | Program should continue |
| Unrecoverable error | Programming bug — use `panic!` | Index out of bounds | Program should stop |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| An ordered list of items | `Vec<T>` | Growable, type-safe list |
| Safe access to a Vec by index | `.get(i)` | Returns `Option` instead of panicking |
| A key-value lookup | `HashMap<K, V>` | Fast lookup by key |
| Default value if key is missing | `.entry(key).or_insert(default)` | Does not overwrite existing entries |
| A value that might not exist | `Option<T>` | Forces you to handle `None` |
| A function that might fail | `Result<T, E>` | Forces you to handle `Err` |
| A fixed set of possible values | Enum | Compiler catches missed cases in `match` |
| Expected failure (like bad input) | `Result` | Recoverable — program continues |
| A programming bug | `panic!` | Unrecoverable — stop the program |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| Why does `match` not compile if I miss a case? | `match` is exhaustive — all variants must be handled or use `_` | Add `_ => {}` to catch remaining cases |
| Why does `.get()` return `Option` instead of the value? | The index might not exist. `.get()` forces you to handle both cases. | `v.get(100)` returns `None`, not a crash |
| When do I use `String` vs `&str`? | `String` for data you build or change at runtime. `&str` for fixed text or read-only parameters. | Struct fields: `String`. Function params: `&str`. |
| What is `entry().or_insert()` for? | Setting a default value when a key does not exist yet. Does not overwrite existing values. | `accounts.entry("Carol").or_insert(0)` |
| When should I use `panic!`? | Only for genuine programming bugs where continuing is unsafe. | Index out of bounds is a bug. Insufficient funds is not. |
| What is the difference between `Option` and `Result`? | `Option` = value might be absent. `Result` = operation might fail with a reason. | `.get()` → `Option`. `transfer()` → `Result`. |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Forgetting `_` in `match` | Compiler error: non-exhaustive patterns | Add `_ => {}` if not all cases matter |
| Using `v[i]` instead of `v.get(i)` | Panics if `i` is out of range | Use `.get()` and handle `None` |
| Using `panic!` for expected errors | Program crashes on a normal situation | Use `Result` for expected failures |
| Using `or_insert` when you want to overwrite | Does not replace an existing value | Use `.insert()` to overwrite |
| Forgetting `use std::collections::HashMap` | Compiler cannot find `HashMap` | Add the import at the top of the file |
| Not handling both `Ok` and `Err` | Compiler error or silent wrong behavior | Always `match` on both variants |

---

## 8. Compiler Errors As Clues

| Error | What it means | What to check |
|---|---|---|
| `non-exhaustive patterns: ... not covered` | A `match` arm is missing | Add the missing variant or use `_` as catch-all |
| `cannot use as index, type is usize` | Wrong type used as Vec index | Vec indices must be `usize` |
| `use of moved value` | A `String` was passed into a `HashMap` and used again | Borrow with `&` or clone before inserting |
| `mismatched types: expected Option<...>` | You used `.get()` result directly | Unwrap with `match` or `if let` |
| `this function must return a value` | `Result` or `Option` function returned nothing | Return `Ok(value)` or `Err(...)` explicitly |

---

## 9. Practice

### Practice 1 — Exhaustive Match

**Prompt:** Fix the compile error.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
}

match coin {
    Coin::Penny => println!("1 cent"),
    Coin::Nickel => println!("5 cents"),
}
```

**Check:** Add the missing arm or catch-all.

```rust
match coin {
    Coin::Penny => println!("1 cent"),
    Coin::Nickel => println!("5 cents"),
    Coin::Dime => println!("10 cents"),
}
```

### Practice 2 — Safe Vec Access

**Prompt:** Rewrite this to not panic on a bad index.

```rust
let v = vec![10, 20, 30];
println!("{}", v[5]);  // panics
```

**Check:**

```rust
let v = vec![10, 20, 30];
match v.get(5) {
    Some(value) => println!("{}", value),
    None => println!("Index does not exist"),
}
```

### Practice 3 — HashMap Default

**Prompt:** Add "Dave" with a starting balance of `0` without overwriting Alice's balance.

```rust
use std::collections::HashMap;

let mut accounts: HashMap<String, u64> = HashMap::new();
accounts.insert(String::from("Alice"), 500);
```

**Check:**

```rust
accounts.entry(String::from("Dave")).or_insert(0);
// Alice's 500 is untouched. Dave gets 0.
```

### Practice 4 — Result For Failure

**Prompt:** Write a `withdraw` function that returns an error if the amount is more than the balance.

**Check:**

```rust
fn withdraw(balance: u64, amount: u64) -> Result<u64, String> {
    if amount > balance {
        Err(String::from("Insufficient funds"))
    } else {
        Ok(balance - amount)
    }
}

match withdraw(100, 200) {
    Ok(new_balance) => println!("New balance: {}", new_balance),
    Err(reason) => println!("Failed: {}", reason),
}
```

---

## 10. Tiny Drills

Short repetitions. These should take 1-3 minutes each.

- Say what `_` does in a `match`
- Say why `.get()` is safer than `v[i]`
- Say what `.entry().or_insert()` does
- Explain `Some` and `None` in one sentence each
- Explain `Ok` and `Err` in one sentence each
- Say when to use `panic!` vs `Result`

---

## 11. Active Recall

Answer without looking:

1. What makes `match` different from a switch statement in other languages?
2. What does `.get()` return and why is it safer than direct indexing?
3. What is the difference between `String` and `&str`?
4. What does `.entry().or_insert()` do?
5. What is `Option<T>`?
6. What is the difference between `Some` and `None`?
7. What is `Result<T, E>`?
8. What is the difference between `Ok` and `Err`?
9. What is the difference between a recoverable error and an unrecoverable error?
10. When should you use `panic!`?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| `Result<T, E>` | Every Solana instruction handler returns `Result<(), ProgramError>` | Failed instructions revert the transaction — no partial state |
| `Option<T>` | Optional accounts passed to instructions | Not all accounts are required in every instruction |
| `match` | Handling instruction types, account states | Programs branch based on enum variants |
| Enum | Instruction variants in an Anchor program | Each instruction type is a named variant |
| `HashMap` | Mental model for on-chain account storage | PDAs work like a map — keys are seeds, values are account data |
| Recoverable vs unrecoverable | Transfer fails vs programming bug | Use `Result` for expected failures, `panic!` only for impossible bugs |

**Memory hook:** Every Solana instruction either returns `Ok(())` — success — or `Err(reason)` — the whole transaction is reverted.

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| `Vec` | Task queues, message history, tool results | Agents accumulate data in ordered lists |
| `HashMap` | Tool registry, memory store, config | Agents look up tools and state by name |
| `Option` | Optional context or configuration | Not all agent runs have all inputs |
| `Result` | Tool call results, payment attempts | Agents must handle failure without crashing |
| `match` | Routing based on tool response | Agents branch on what the tool returned |
| Enum | Agent states or task types | A task can be Pending, Running, Done, or Failed |

**Memory hook:** An agent is a loop of `Result`-returning tool calls — every action might fail, and the agent must handle it.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| `match` — exhaustive pattern matching | Yes | Recap, terms, practice 1 |
| `_` as catch-all | Yes | Confusions, practice 1 |
| Enum definition and use | Yes | Terms, key concepts, practice 1 |
| `Vec` creation and `.push()` | Yes | Terms, key concepts |
| Safe access with `.get()` | Yes | Terms, practice 2 |
| `HashMap` — insert and lookup | Yes | Terms, practice 3 |
| `.entry().or_insert()` | Yes | Terms, practice 3 |
| `String` vs `&str` | Yes | Recap, confusions |
| `Option` — `Some` and `None` | Yes | Terms, drills, active recall |
| `Result` — `Ok` and `Err` | Yes | Terms, practice 4 |
| Recoverable vs unrecoverable | Yes | Terms, key concepts, confusions |
| `panic!` | Yes | Terms, confusions |
| Q&A: `match` as switch with fallback | Yes | Confusions |

---

## 15. Final Study Checklist

- [ ] I know what `match` does and why it is exhaustive
- [ ] I can use `Vec` and access it safely with `.get()`
- [ ] I can use `HashMap` and set defaults with `.entry().or_insert()`
- [ ] I know the difference between `String` and `&str`
- [ ] I know what `Option` is and can handle `Some` and `None`
- [ ] I know what `Result` is and can handle `Ok` and `Err`
- [ ] I know the difference between recoverable and unrecoverable errors
- [ ] I completed all four practice exercises
- [ ] I can explain the Solana/blockchain relevance
- [ ] I can explain the AI agent relevance
